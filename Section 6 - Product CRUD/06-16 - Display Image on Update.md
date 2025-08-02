## Cải thiện Layout và chuẩn bị chức năng Update cho Product Form

### Khái niệm

Thay vì để form Product chiếm toàn bộ chiều rộng, cần chia layout thành **hai cột**:

- **Cột 10**: Chứa form nhập liệu
- **Cột 2**: Hiển thị hình ảnh sản phẩm bên phải

Đồng thời cần chuẩn bị để implement logic **Update** trong POST method của Upsert action.

### Cách thực hiện Layout Changes

**Bước 1: Cập nhật cấu trúc HTML trong Upsert.cshtml**

```html
@model ProductViewModel

<form method="post" enctype="multipart/form-data">
    <div class="border p-3">
        <!-- Thêm row container -->
        <div class="row">
            <!-- Cột 10 cho form -->
            <div class="col-10">
                <div class="form-floating py-2 col-12">
                    <h2 class="text-primary">
                        @(Model.Product.Id != 0 ? "Update" : "Create") Product
                    </h2>
                    <hr />
                </div>
                
                <!-- Tất cả form fields ở đây -->
                <input asp-for="Product.Id" hidden />
                
                <div class="mb-3">
                    <label asp-for="Product.Title" class="form-label"></label>
                    <input asp-for="Product.Title" class="form-control" />
                    <span asp-validation-for="Product.Title" class="text-danger"></span>
                </div>
                
                <!-- ... các fields khác ... -->
                
                <div class="row">
                    <div class="col-6 col-md-3">
                        @if (Model.Product.Id != 0)
                        {
                            <button type="submit" class="btn btn-primary form-control">Update</button>
                        }
                        else
                        {
                            <button type="submit" class="btn btn-primary form-control">Create</button>
                        }
                    </div>
                    <div class="col-6 col-md-3">
                        <a asp-controller="Product" asp-action="Index" class="btn btn-outline-secondary border form-control">
                            Back to List
                        </a>
                    </div>
                </div>
            </div>
            
            <!-- Cột 2 cho hình ảnh -->
            <div class="col-2">
                <img src="@Model.Product.ImageURL" width="100%" style="border-radius:5px; border:1px solid #bbb9b9" />
            </div>
        </div>
    </div>
</form>
```


### Đặc điểm Layout mới

**Bootstrap Grid System:**

- **`col-10`**: Form chiếm 10/12 cột (83.33% chiều rộng)
- **`col-2`**: Hình ảnh chiếm 2/12 cột (16.67% chiều rộng)
- **Responsive**: Tự động adapt trên các thiết bị khác nhau

**Image Display:**

- **`width="100%"`**: Hình ảnh fill toàn bộ cột
- **Custom styling**: Border radius và border color để đẹp mắt
- **Dynamic src**: Hiển thị ImageURL từ model


### Hành vi theo chế độ

**Create Mode:**

- Hình ảnh sẽ trống (ImageURL = null hoặc empty)
- Form hiển thị bình thường với layout mới
- User có thể thấy preview area cho hình ảnh

**Edit Mode:**

- Hình ảnh hiện tại của sản phẩm được hiển thị
- User có thể thấy hình ảnh đang có khi chỉnh sửa
- Tạo trải nghiệm tốt hơn cho người dùng


### Vấn đề cần giải quyết

**POST Method chưa hoàn chỉnh:**

Hiện tại trong `ProductController`, POST action chỉ xử lý **Create**:

```csharp
[HttpPost]
public IActionResult Upsert(ProductViewModel productVM, IFormFile? file)
{
    if (ModelState.IsValid)
    {
        // Logic xử lý file upload...
        
        // CHỈ XỬ LÝ CREATE
        if (productVM.Product.Id == 0)
        {
            _unitOfWork.Product.Add(productVM.Product);  // ✅ Đã implement
        }
        else
        {
            // ❌ CHƯA IMPLEMENT UPDATE LOGIC
            _unitOfWork.Product.Update(productVM.Product);
        }
        
        _unitOfWork.Save();
        return RedirectToAction("Index");
    }
    
    // Handle validation errors...
}
```

**Những gì cần implement cho Update:**

- Xử lý việc thay thế hình ảnh cũ
- Logic để giữ hình ảnh cũ nếu không upload hình mới
- Xóa file hình ảnh cũ khi upload hình mới
- Validation cho Update mode


### Kết quả hiện tại

- **✅ Layout**: Form đã được chia cột đẹp mắt
- **✅ Image Display**: Hiển thị hình ảnh bên phải form
- **✅ Create Function**: Hoạt động hoàn chỉnh với file upload
- **❌ Update Function**: Chưa được implement trong POST method


### Chuẩn bị cho video tiếp theo

Video tiếp theo sẽ focus vào:

- **Update Logic**: Implement đầy đủ chức năng cập nhật sản phẩm
- **Image Replacement**: Xử lý thay thế hình ảnh trong Update mode
- **File Management**: Quản lý việc xóa file cũ khi có file mới
- **Error Handling**: Xử lý các trường hợp lỗi trong Update


### Ghi chú thêm

- Layout mới cải thiện đáng kể user experience
- Bootstrap grid system đảm bảo responsive design
- Cần cẩn thận với việc handle existing images trong Update mode
- Image preview giúp user xác nhận hình ảnh trước khi submit
- Custom styling tạo giao diện professional hơn

**Liên kết:** [[ProductViewModel]], [[Bootstrap Grid]], [[Upsert]], [[Image Display]], [[File Upload]], [[Product Controller]], [[Layout Design]], [[Responsive Design]], [[Update Logic]], [[ASP.NET Core MVC]]

