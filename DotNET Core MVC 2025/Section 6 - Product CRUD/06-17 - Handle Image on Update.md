## Xử lý Update Product với chức năng thay thế hình ảnh

### Khái niệm logic Update

Khi cập nhật sản phẩm (Product), cần xử lý các tình huống khác nhau với hình ảnh:

- **Không thay đổi hình ảnh**: Nếu user chỉ cập nhật mô tả mà không upload hình mới, giữ nguyên hình ảnh cũ
- **Thay thế hình ảnh**: Nếu user upload hình mới, cần xóa hình cũ và lưu hình mới
- **Phân biệt Create/Update**: Dựa vào ID để xác định thao tác thêm mới hay cập nhật


### Cách thực hiện

**Logic xử lý trong Upsert POST Action:**

```csharp
[HttpPost]
public IActionResult Upsert(ProductViewModel productVM, IFormFile? file)
{
    if (ModelState.IsValid)
    {
        string wwwRootPath = _webHostEnvironment.WebRootPath;
        
        if (file != null)
        {
            // Tạo tên file mới
            string fileName = Guid.NewGuid().ToString() + Path.GetExtension(file.FileName);
            string productPath = Path.Combine(wwwRootPath, @"images\product");
            
            // XỬ LÝ XÓA HÌNH ẢNH CŨ KHI UPDATE
            if (!string.IsNullOrEmpty(productVM.Product.ImageURL))
            {
                // Xây dựng đường dẫn hình ảnh cũ
                var oldImagePath = Path.Combine(wwwRootPath, 
                    productVM.Product.ImageURL.TrimStart('\\'));
                
                // Xóa file cũ nếu tồn tại
                if (System.IO.File.Exists(oldImagePath))
                {
                    System.IO.File.Delete(oldImagePath);
                }
            }
            
            // Lưu file mới
            using (var fileStream = new FileStream(Path.Combine(productPath, fileName), FileMode.Create))
            {
                file.CopyTo(fileStream);
            }
            
            // Cập nhật ImageURL
            productVM.Product.ImageURL = @"\images\product\" + fileName;
        }
        
        // PHÂN BIỆT CREATE VÀ UPDATE DỰA VÀO ID
        if (productVM.Product.Id == 0)
        {
            // CREATE: Thêm sản phẩm mới
            _unitOfWork.Product.Add(productVM.Product);
        }
        else
        {
            // UPDATE: Cập nhật sản phẩm có sẵn
            _unitOfWork.Product.Update(productVM.Product);
        }
        
        _unitOfWork.Save();
        TempData["success"] = "Product created/updated successfully";
        return RedirectToAction("Index");
    }
    
    // Xử lý lỗi validation...
}
```


### Các bước xử lý chi tiết

**Bước 1: Kiểm tra file upload mới**

- Nếu `file != null`: User đã chọn hình ảnh mới
- Nếu `file == null`: Giữ nguyên hình ảnh hiện tại

**Bước 2: Xóa hình ảnh cũ (nếu có)**

```csharp
if (!string.IsNullOrEmpty(productVM.Product.ImageURL))
{
    var oldImagePath = Path.Combine(wwwRootPath, 
        productVM.Product.ImageURL.TrimStart('\\'));
    
    if (System.IO.File.Exists(oldImagePath))
    {
        System.IO.File.Delete(oldImagePath);
    }
}
```

**Đặc điểm quan trọng:**

- **TrimStart('\\')**: Loại bỏ dấu gạch chéo đầu tiên vì `Path.Combine` không cần
- **File.Exists()**: Kiểm tra file có tồn tại trước khi xóa
- **File.Delete()**: Xóa file khỏi hệ thống

**Bước 3: Phân biệt Create vs Update**

```csharp
if (productVM.Product.Id == 0)
{
    _unitOfWork.Product.Add(productVM.Product);    // CREATE
}
else
{
    _unitOfWork.Product.Update(productVM.Product); // UPDATE
}
```


### Tình huống xử lý

**Tình huống 1: Update không thay đổi hình ảnh**

- User chỉ sửa mô tả, giá cả
- `file == null`
- Giữ nguyên `ImageURL` cũ
- Chỉ cập nhật các field khác

**Tình huống 2: Update với hình ảnh mới**

- User upload hình mới
- `file != null` và `ImageURL` có giá trị
- Xóa file cũ từ server
- Lưu file mới với tên GUID
- Cập nhật `ImageURL` mới

**Tình huống 3: Create sản phẩm mới**

- `productVM.Product.Id == 0`
- Lưu hình ảnh mới (nếu có)
- Thêm record mới vào database


### Lưu ý quan trọng

**Xử lý đường dẫn file:**

- Database lưu `ImageURL` có dạng: `\images\product\filename.jpg`
- Cần `TrimStart('\\')` để loại bỏ dấu gạch chéo đầu
- `Path.Combine()` sẽ kết hợp đúng đường dẫn

**Quản lý file system:**

- Luôn kiểm tra file tồn tại trước khi xóa
- Sử dụng `System.IO.File` cho các thao tác file
- Tránh lỗi runtime khi file không tồn tại

**Performance consideration:**

- Chỉ xóa file cũ khi thực sự có file mới
- Không làm gì nếu user không upload hình mới


### Chuẩn bị cho bước tiếp theo

Video tiếp theo sẽ đề cập đến:

- Hoàn thiện logic Update
- Test các tình huống khác nhau
- Xử lý validation cho Update mode
- Cải thiện user experience

**Liên kết:** [[ProductViewModel]], [[Product Controller]], [[File Upload]], [[Image Management]], [[Upsert]], [[System.IO.File]], [[Path.Combine]], [[GUID]], [[Entity Framework Core]], [[UnitOfWork]], [[File System]], [[ASP.NET Core MVC]]

