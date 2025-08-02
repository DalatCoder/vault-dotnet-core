## Hoàn thiện chức năng Update Product và xử lý Image Replacement

### Test chức năng Update với Image Replacement

**Vấn đề phát hiện:**

- Khi update product với hình ảnh mới, hình ảnh cũ không bị xóa khỏi server
- Logic xóa file cũ không được thực thi

**Nguyên nhân:**

- Trường `ImageURL` không được gửi về server khi POST form
- Do không có input field nào chứa giá trị `ImageURL` hiện tại


### Giải pháp: Thêm Hidden Field cho ImageURL

**Cập nhật Upsert.cshtml:**

```html
@model ProductViewModel

<form method="post" enctype="multipart/form-data">
    <!-- Hidden fields bắt buộc -->
    <input asp-for="Product.Id" hidden />
    <input asp-for="Product.ImageURL" hidden />
    
    <div class="border p-3">
        <div class="row">
            <div class="col-10">
                <!-- Form fields khác -->
            </div>
            <div class="col-2">
                <img src="@Model.Product.ImageURL" width="100%" />
            </div>
        </div>
    </div>
</form>
```

**Tại sao cần Hidden Field:**

- Form POST chỉ gửi dữ liệu từ các input elements
- `ImageURL` không có input field visible nên không được POST
- Hidden field đảm bảo `ImageURL` hiện tại được gửi về Controller
- Controller cần giá trị này để xây dựng đường dẫn xóa file cũ


### Logic Update hoạt động đúng

**Sau khi thêm hidden field:**

```csharp
[HttpPost]
public IActionResult Upsert(ProductViewModel productVM, IFormFile? file)
{
    if (ModelState.IsValid)
    {
        string wwwRootPath = _webHostEnvironment.WebRootPath;
        
        if (file != null)
        {
            // Tạo file name mới
            string fileName = Guid.NewGuid().ToString() + Path.GetExtension(file.FileName);
            string productPath = Path.Combine(wwwRootPath, @"images\product");
            
            // XÓA FILE CŨ - Giờ hoạt động đúng
            if (!string.IsNullOrEmpty(productVM.Product.ImageURL))
            {
                var oldImagePath = Path.Combine(wwwRootPath, 
                    productVM.Product.ImageURL.TrimStart('\\'));
                
                if (System.IO.File.Exists(oldImagePath))
                {
                    System.IO.File.Delete(oldImagePath);  // ✅ Xóa thành công
                }
            }
            
            // Lưu file mới
            using (var fileStream = new FileStream(Path.Combine(productPath, fileName), FileMode.Create))
            {
                file.CopyTo(fileStream);
            }
            
            productVM.Product.ImageURL = @"\images\product\" + fileName;
        }
        
        // Update logic...
    }
}
```


### Entity Framework Core Behavior

**Smart Update của EF Core:**

- Khi gọi `_unitOfWork.Product.Update(product)`, EF Core tự động update **tất cả properties**
- Nếu chỉ thay đổi Title, EF Core vẫn sẽ update cả ImageURL, Description, Price, etc.
- Đây là hành vi mặc định và thường không gây vấn đề

**Potential Issue:**

- Nếu `ImageURL` bị null trong quá trình update, field này sẽ bị xóa khỏi database
- Hidden field đảm bảo `ImageURL` luôn có giá trị đúng


### Tùy chọn Manual Mapping (Advanced)

**Cập nhật ProductRepository để có control tốt hơn:**

```csharp
public void Update(Product obj)
{
    // Lấy object hiện tại từ database
    var objFromDb = _db.Products.FirstOrDefault(u => u.Id == obj.Id);
    
    if (objFromDb != null)
    {
        // Manual mapping các properties cần update
        objFromDb.Title = obj.Title;
        objFromDb.ISBN = obj.ISBN;
        objFromDb.Price = obj.Price;
        objFromDb.Price50 = obj.Price50;
        objFromDb.Price100 = obj.Price100;
        objFromDb.ListPrice = obj.ListPrice;
        objFromDb.Description = obj.Description;
        objFromDb.CategoryId = obj.CategoryId;
        objFromDb.Author = obj.Author;
        
        // Chỉ update ImageURL nếu có giá trị mới
        if (obj.ImageURL != null)
        {
            objFromDb.ImageURL = obj.ImageURL;
        }
    }
}
```

**Ưu điểm Manual Mapping:**

- **Selective Update**: Chỉ update những field thực sự thay đổi
- **Custom Logic**: Có thể thêm business rules cho từng property
- **Safety**: Tránh việc accidentally clear một số fields
- **Flexibility**: Dễ dàng mở rộng logic validation

**Tại sao có Update trong Individual Repository:**

- Generic Repository không thể handle custom update logic
- Mỗi entity có thể có yêu cầu update khác nhau
- Flexibility cho business requirements phức tạp


### Test Cases đã hoàn thành

**✅ Update với hình ảnh mới:**

- File cũ được xóa thành công
- File mới được lưu với GUID name
- Database được cập nhật ImageURL mới

**✅ Update chỉ text fields:**

- Hình ảnh giữ nguyên
- Chỉ các text fields được cập nhật
- ImageURL không bị thay đổi

**✅ Thêm hình ảnh cho tất cả products:**

- Fortune of Time ✅
- Dark Skies ✅
- Vanish in the Sunset ✅
- Rock in the Ocean ✅
- Leaves and Wonders ✅


### Chuẩn bị cho bước tiếp theo

**Vấn đề còn lại:**

- Product Index hiện đang hiển thị `CategoryId` (số) thay vì tên Category
- Cần cải thiện để hiển thị tên Category thực tế
- Sẽ được xử lý trong video tiếp theo


### Ghi chú thêm

- **Hidden Fields** rất quan trọng cho việc maintain state trong form POST
- **File Management** cần được test kỹ lưỡng với các edge cases
- **EF Core** có behavior mặc định update all properties, cần hiểu rõ để tránh bất ngờ
- **Manual Mapping** là pattern hữu ích cho complex update scenarios
- **Debugging** giúp hiểu rõ data flow và phát hiện issues sớm
- Luôn kiểm tra file system để đảm bảo old files được cleanup đúng cách

**Liên kết:** [[ProductViewModel]], [[Product Controller]], [[Hidden Fields]], [[Entity Framework Core]], [[File Management]], [[Manual Mapping]], [[Repository Pattern]], [[Image Upload]], [[Form POST]], [[System.IO.File]], [[Upsert]], [[GUID]], [[Path.Combine]], [[UnitOfWork]]

