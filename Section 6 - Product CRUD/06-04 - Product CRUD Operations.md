## Thực hiện CRUD Operations cho Product

### Tạo Product Controller

**Phương pháp nhanh:**

- Copy toàn bộ `CategoryController` trong thư mục `Areas/Admin/Controllers`
- Đổi tên file thành `ProductController.cs`
- Sửa đổi class name và constructor name thành `ProductController`

**Thay thế tham chiếu:**

- Sử dụng `Ctrl + Shift + F` (Replace in Files)
- Thay thế tất cả `Category` thành `Product` trong current document
- Đảm bảo chọn "Match case" để thay thế chính xác
- Thay thế `category` (lowercase) thành `product` (lowercase)

**Điều chỉnh code:**

```csharp
public class ProductController : Controller
{
    private readonly IUnitOfWork _unitOfWork;
    
    public ProductController(IUnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }
    
    // Các action methods sử dụng _unitOfWork.Product thay vì _unitOfWork.Category
}
```

- Loại bỏ custom validation trong Create action (không cần thiết cho Product)
- Tất cả các thao tác CRUD sử dụng `_unitOfWork.Product` thay vì `_unitOfWork.Category`


### Tạo Product Views

**Bước 1: Copy Views từ Category**

- Copy toàn bộ thư mục `Views/Category`
- Đổi tên thành `Views/Product`
- Mở tất cả 4 files view (Index, Create, Edit, Delete)

**Bước 2: Thay thế tham chiếu**

- Sử dụng `Ctrl + Shift + F` với option "All Open Documents"
- Thay thế `Category` thành `Product` trong tất cả views đang mở

**Bước 3: Cập nhật thuộc tính cho Create.cshtml và Edit.cshtml**

```html
<!-- Thay vì chỉ có Name, giờ có đầy đủ thuộc tính Product -->
<div class="mb-3">
    <label asp-for="Title" class="form-label"></label>
    <input asp-for="Title" class="form-control" />
    <span asp-validation-for="Title" class="text-danger"></span>
</div>

<div class="mb-3">
    <label asp-for="Description" class="form-label"></label>
    <textarea asp-for="Description" class="form-control"></textarea>
    <span asp-validation-for="Description" class="text-danger"></span>
</div>

<div class="mb-3">
    <label asp-for="ISBN" class="form-label"></label>
    <input asp-for="ISBN" class="form-control" />
    <span asp-validation-for="ISBN" class="text-danger"></span>
</div>

<div class="mb-3">
    <label asp-for="Author" class="form-label"></label>
    <input asp-for="Author" class="form-control" />
    <span asp-validation-for="Author" class="text-danger"></span>
</div>

<div class="mb-3">
    <label asp-for="ListPrice" class="form-label"></label>
    <input asp-for="ListPrice" class="form-control" />
    <span asp-validation-for="ListPrice" class="text-danger"></span>
</div>

<div class="mb-3">
    <label asp-for="Price" class="form-label"></label>
    <input asp-for="Price" class="form-control" />
    <span asp-validation-for="Price" class="text-danger"></span>
</div>

<div class="mb-3">
    <label asp-for="Price50" class="form-label"></label>
    <input asp-for="Price50" class="form-control" />
    <span asp-validation-for="Price50" class="text-danger"></span>
</div>

<div class="mb-3">
    <label asp-for="Price100" class="form-label"></label>
    <input asp-for="Price100" class="form-control" />
    <span asp-validation-for="Price100" class="text-danger"></span>
</div>
```

**Bước 4: Cập nhật Delete.cshtml**

- Copy cấu trúc form từ Edit
- Thêm `disabled` cho tất cả input fields
- Loại bỏ validation spans

**Bước 5: Cập nhật Index.cshtml**

```html
<table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th>Title</th>
            <th>ISBN</th>
            <th>List Price</th>
            <th>Author</th>
            <th>Category</th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        @foreach (var obj in Model)
        {
            <tr>
                <td>@obj.Title</td>
                <td>@obj.ISBN</td>
                <td>@obj.ListPrice</td>
                <td>@obj.Author</td>
                <td>[Category sẽ được thêm sau]</td>
                <td>
                    <!-- Edit và Delete buttons -->
                </td>
            </tr>
        }
    </tbody>
</table>
```


### Thêm Navigation Menu

Trong `Views/Shared/_Layout.cshtml`:

```html
<li class="nav-item dropdown">
    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown"
        aria-expanded="false">
        Content Management
    </a>
    <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#">Category</a></li>
        <li><a class="dropdown-item" href="#">Product</a></li>
    </ul>
</li>
```


### Cải thiện UI

**Khoảng cách margin:**

- Thay thế `mt-4` (margin-top) thành `my-4` (margin-y) trong tất cả views
- Áp dụng cho Create, Edit, Delete, và Index views
- Tạo khoảng cách đều và đẹp mắt hơn

**Cải thiện Description field:**

- Sử dụng `<textarea>` thay vì `<input>` cho Description
- Đảm bảo có explicit closing tag `</textarea>`


### Kiểm tra và Test

**Các chức năng đã hoạt động:**

- ✅ Hiển thị danh sách products
- ✅ Tạo product mới
- ✅ Chỉnh sửa product
- ✅ Xóa product
- ✅ Navigation menu hoạt động

**Lưu ý:**

- Category field trong Index view chưa hiển thị đúng (sẽ được xử lý trong video sau)
- UI cơ bản đã hoàn chình, sẽ được cải thiện thêm trong các video tiếp theo


### Ghi chú thêm

- Phương pháp copy-paste và modify giúp tiết kiệm thời gian đáng kể
- Sử dụng Find \& Replace (Ctrl + Shift + F) hiệu quả cho việc rename hàng loạt
- Cần kiểm tra kỹ các thuộc tính mới của Product model khi tạo form
- Textarea cần explicit closing tag để hoạt động đúng
- Category relationship sẽ được implement trong các video sau

**Liên kết:** [[Product Model]], [[CategoryController]], [[CRUD Operations]], [[Product Repository]], [[ASP.NET Core MVC]], [[Razor Views]], [[Bootstrap Forms]], [[Navigation Menu]], [[UnitOfWork]], [[Entity Framework Core]]

