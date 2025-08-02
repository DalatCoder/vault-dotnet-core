## Thêm File Upload cho ImageURL trong Product Form

### Khái niệm

Thay vì sử dụng input text để nhập đường dẫn **ImageURL**, cần chuyển đổi thành **file upload** để người dùng có thể upload hình ảnh trực tiếp lên server. Điều này cải thiện trải nghiệm người dùng và đảm bảo tính nhất quán của dữ liệu hình ảnh.

### Phạm vi thực hiện

- **Hiện tại**: Chỉ thực hiện trên **Create form**
- **Edit form**: Sẽ được xử lý trong video tiếp theo (có lý do kỹ thuật riêng)
- **Mục đích**: Tập trung hoàn thiện một chức năng trước khi mở rộng


### Cách thực hiện

**Bước 1: Thay đổi input type trong Create.cshtml**

Tìm phần ImageURL trong form và sửa đổi:

```html
<!-- Trước đây: Input text thông thường -->
<div class="mb-3">
    <label asp-for="Product.ImageURL" class="form-label"></label>
    <input asp-for="Product.ImageURL" class="form-control" />
    <span asp-validation-for="Product.ImageURL" class="text-danger"></span>
</div>

<!-- Sau khi sửa đổi: File upload -->
<div class="mb-3">
    <label asp-for="Product.ImageURL" class="form-label"></label>
    <input type="file" class="form-control" />
</div>
```

**Các thay đổi quan trọng:**

- **Loại bỏ** `asp-for="Product.ImageURL"` khỏi input
- **Thêm** `type="file"` để tạo file upload field
- **Loại bỏ** validation span vì không cần thiết cho file upload
- **Giữ nguyên** `class="form-control"` cho styling Bootstrap

**Bước 2: Thêm enctype vào form tag**

Điều quan trọng nhất là cập nhật thẻ `<form>`:

```html
<!-- Thêm enctype="multipart/form-data" -->
<form method="post" enctype="multipart/form-data">
    <!-- Các input fields khác -->
    
    <div class="mb-3">
        <label asp-for="Product.ImageURL" class="form-label"></label>
        <input type="file" class="form-control" />
    </div>
    
    <!-- Submit button -->
</form>
```


### Tại sao cần multipart/form-data?

**Encoding Type mặc định:**

- Form HTML thông thường sử dụng `application/x-www-form-urlencoded`
- Encoding này **không hỗ trợ** file upload

**Multipart/form-data:**

- **Bắt buộc** cho tất cả forms có file upload
- Cho phép truyền **binary data** (hình ảnh, documents)
- Chia dữ liệu thành nhiều phần (parts) để xử lý
- **Không thêm = file upload sẽ không hoạt động**


### Cấu trúc form hoàn chỉnh

```html
@model ProductViewModel

<form method="post" enctype="multipart/form-data">
    <div class="border p-3">
        <div class="form-floating py-2 col-12">
            <h2 class="text-primary">Create Product</h2>
            <hr />
        </div>
        
        <!-- Các fields khác như Title, Description, etc. -->
        
        <div class="mb-3">
            <label asp-for="Product.ImageURL" class="form-label"></label>
            <input type="file" class="form-control" />
        </div>
        
        <div class="mb-3">
            <label asp-for="Product.CategoryId" class="form-label"></label>
            <select asp-for="Product.CategoryId" asp-items="@Model.CategoryList" class="form-select">
                <option disabled selected>Select Category</option>
            </select>
            <span asp-validation-for="Product.CategoryId" class="text-danger"></span>
        </div>
        
        <div class="row">
            <div class="col-6 col-md-3">
                <button type="submit" class="btn btn-primary form-control">Create</button>
            </div>
            <div class="col-6 col-md-3">
                <a asp-controller="Product" asp-action="Index" class="btn btn-outline-secondary border form-control">
                    Back to List
                </a>
            </div>
        </div>
    </div>
</form>
```


### Kết quả giao diện

Sau khi thực hiện các thay đổi:

- **ImageURL field** hiển thị như một button "Choose File"
- **Giao diện**: Phù hợp với Bootstrap styling
- **Functionality**: Cho phép người dùng chọn file từ máy tính
- **Form**: Sẵn sàng để xử lý file upload trong Controller


### Lưu ý quan trọng

- **Enctype bắt buộc**: Không có `multipart/form-data` = file upload không hoạt động
- **Validation**: Loại bỏ client-side validation cho file field
- **Label**: Vẫn giữ label để UI consistency
- **Bootstrap**: `form-control` class vẫn hoạt động với input file
- **Next step**: Xử lý file upload logic trong Controller action


### Ghi chú thêm

- Việc chỉ implement Create trước có lý do kỹ thuật sẽ được giải thích
- Edit form cần logic phức tạp hơn để handle existing images
- File upload cần validation ở server-side (file type, size, etc.)
- Cần xử lý lưu trữ file (local storage hoặc cloud storage)
- Security considerations cho file upload sẽ được đề cập later

**Liên kết:** [[ProductViewModel]], [[File Upload]], [[ASP.NET Core MVC]], [[HTML Forms]], [[Multipart Form Data]], [[Bootstrap Forms]], [[Product Model]], [[ImageURL]], [[Form Encoding]], [[Server-side Validation]]

