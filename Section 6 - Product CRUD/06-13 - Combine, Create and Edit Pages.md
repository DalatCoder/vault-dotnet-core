## Kết hợp Create và Edit thành Upsert Page

### Khái niệm

Thay vì có hai trang riêng biệt cho **Create** và **Edit**, nhiều ứng dụng sử dụng một trang duy nhất cho cả hai chức năng. Trang này được gọi là **Upsert** (Update + Insert):

- **Upsert = UP (Update) + SERT (Insert)**
- Nếu có ID: Thực hiện **Update** (hiển thị dữ liệu có sẵn)
- Nếu không có ID: Thực hiện **Create** (form trống)
- Giảm code duplication và dễ bảo trì hơn


### Ưu điểm của Upsert Pattern

- **Single Page**: Chỉ cần một view thay vì hai
- **Code reuse**: Logic form được sử dụng chung
- **Consistency**: Giao diện nhất quán giữa Create và Edit
- **Maintenance**: Dễ bảo trì và cập nhật


### Cách thực hiện

**Bước 1: Sửa đổi Controller Action**

Đổi tên từ `Create` thành `Upsert` và thêm logic xử lý ID:

```csharp
public IActionResult Upsert(int? id)
{
    ProductViewModel productVM = new ProductViewModel()
    {
        CategoryList = _unitOfWork.Category.GetAll()
            .Select(u => new SelectListItem
            {
                Text = u.Name,
                Value = u.Id.ToString()
            }),
        Product = new Product()
    };
    
    // Kiểm tra ID để phân biệt Create vs Update
    if (id == null || id == 0)
    {
        // CREATE: Trả về form trống
        return View(productVM);
    }
    else
    {
        // UPDATE: Lấy dữ liệu existing product
        productVM.Product = _unitOfWork.Product.Get(u => u.Id == id);
        return View(productVM);
    }
}
```

**Bước 2: Cập nhật POST Action Method**

```csharp
[HttpPost]
public IActionResult Upsert(ProductViewModel productVM, IFormFile? file)
{
    if (ModelState.IsValid)
    {
        // Logic xử lý file upload sẽ được thêm sau
        
        if (productVM.Product.Id == 0)
        {
            // CREATE
            _unitOfWork.Product.Add(productVM.Product);
        }
        else
        {
            // UPDATE
            _unitOfWork.Product.Update(productVM.Product);
        }
        
        _unitOfWork.Save();
        TempData["success"] = "Product created/updated successfully";
        return RedirectToAction("Index");
    }
    else
    {
        // Populate dropdown lại khi có lỗi
        productVM.CategoryList = _unitOfWork.Category.GetAll()
            .Select(u => new SelectListItem
            {
                Text = u.Name,
                Value = u.Id.ToString()
            });
        
        return View(productVM);
    }
}
```

**Bước 3: Cập nhật View (Upsert.cshtml)**

Đổi tên file từ `Create.cshtml` thành `Upsert.cshtml` và thêm logic động:

```html
@model ProductViewModel

<form method="post" enctype="multipart/form-data">
    <div class="border p-3">
        <div class="form-floating py-2 col-12">
            <!-- Title động dựa trên ID -->
            <h2 class="text-primary">
                @(Model.Product.Id != 0 ? "Update" : "Create") Product
            </h2>
            <hr />
        </div>
        
        <!-- Hidden field để lưu ID khi POST -->
        <input asp-for="Product.Id" hidden />
        
        <!-- Các input fields khác -->
        <div class="mb-3">
            <label asp-for="Product.Title" class="form-label"></label>
            <input asp-for="Product.Title" class="form-control" />
            <span asp-validation-for="Product.Title" class="text-danger"></span>
        </div>
        
        <!-- ... other fields ... -->
        
        <!-- Submit button động -->
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
</form>
```

**Bước 4: Cập nhật Navigation Links**

Trong `Index.cshtml` và `_Layout.cshtml`, cập nhật các links:

```html
<!-- Thay vì Create -->
<a asp-controller="Product" asp-action="Upsert" class="btn btn-primary">
    Create New Product
</a>

<!-- Thay vì Edit -->
<a asp-controller="Product" asp-action="Upsert" asp-route-id="@item.Id" class="btn btn-primary">
    Edit
</a>
```


### Các điểm quan trọng

**Hidden ID Field:**

- `<input asp-for="Product.Id" hidden />` đảm bảo ID được POST về server
- Quan trọng để phân biệt Create (ID = 0) vs Update (ID > 0)

**Dynamic Title và Button:**

- Sử dụng Razor syntax `@(Model.Product.Id != 0 ? "Update" : "Create")`
- Hiển thị phù hợp với ngữ cảnh sử dụng

**IFormFile Parameter:**

- Thêm `IFormFile? file` vào POST action để xử lý file upload
- Logic xử lý file sẽ được implement trong video sau

**Conditional Logic:**

- Sử dụng if-else thay vì inline condition để dễ mở rộng validation sau này
- Chuẩn bị cho việc thêm các validation rule khác nhau cho Create vs Update


### Kết quả

- **Create**: `/Product/Upsert` hiển thị form trống với title "Create Product"
- **Edit**: `/Product/Upsert/5` hiển thị form với dữ liệu có sẵn và title "Update Product"
- **Consistent UI**: Cùng giao diện và validation rules
- **Simplified Navigation**: Chỉ cần quản lý một action và một view


### Ghi chú thêm

- Pattern này phổ biến trong các ứng dụng enterprise
- Giảm thiểu code duplication đáng kể
- Dễ dàng áp dụng business rules chung cho cả Create và Update
- IFormFile parameter chuẩn bị cho chức năng upload hình ảnh
- Validation có thể được tùy chỉnh khác nhau giữa Create và Update mode
- Cần restart application để navigation links hoạt động đúng

**Liên kết:** [[ProductViewModel]], [[Product Controller]], [[CRUD Operations]], [[File Upload]], [[IFormFile]], [[ASP.NET Core MVC]], [[Razor Syntax]], [[Model Binding]], [[TempData]], [[UnitOfWork]], [[Entity Framework Core]], [[Bootstrap Forms]]

