## Hiển thị Trang Chi tiết Sản phẩm (Product Details Page) trong .NET Core MVC

### Khái niệm

- Khi người dùng bấm vào xem chi tiết, trang sẽ hiển thị đầy đủ thông tin của một sản phẩm.
- Model sử dụng cho trang chi tiết là một đối tượng *sản phẩm* (Product).


### Xử lý trong Controller

- Thêm action method `Details` trong `HomeController` (kế thừa từ Index):
    - Phương thức nhận tham số `id` và lấy thông tin sản phẩm dựa trên ID này.

```csharp
public IActionResult Details(int id)
{
    var product = _unitOfWork.Product.GetFirstOrDefault(u => u.Id == id, includeProperties: "Category");
    return View(product);
}
```

- Action nhận `id`, trả về đối tượng `product` cho view chi tiết.


### Cập nhật View Trang Chủ (Index) để Điều hướng tới Trang Chi tiết

- Trong view Index, cập nhật thương hiệu sản phẩm để khi click vào sẽ chuyển đến trang chi tiết:
    - Sử dụng ASP.NET Tag Helper `asp-action` và `asp-route-id` để truyền ID sản phẩm:

```html
<a asp-action="Details" asp-route-id="@product.Id" class="btn btn-primary">Details</a>
```

- Nhờ sử dụng đúng tên route (`id`), ASP.NET sẽ truyền tham số phù hợp.


### Tạo View cho Trang Chi tiết

- Tạo Razor View mới tên là `Details.cshtml` trong đúng thư mục khu vực `Customer`.
    - Model của view là `Product`.
    - Hiển thị mọi thông tin cần thiết về sản phẩm, ví dụ:

```csharp
@model Product
<div class="card border-3 p-3 shadow rounded">
    <img src="@Model.ImageUrl" class="card-img-top rounded" />
    <div class="card-body">
        <h3>@Model.Title</h3>
        <p>Tác giả: @Model.Author</p>
        <p>Danh mục: @Model.Category.Name</p>
        <p class="text-muted text-decoration-line-through">@Model.ListPrice.ToString("c")</p>
        <p class="fw-bold">Giá chỉ: @Model.Price100.ToString("c")</p>
        <!-- Có thể thêm mô tả, thông số kỹ thuật, v.v. -->
    </div>
</div>
```


### Lưu ý

- Cần đảm bảo routes và tham số trùng khớp giữa controller và view (ví dụ: `id`).
- Nếu muốn thay đổi tên tham số, phải đồng bộ cả trong action method và các nơi truyền route.


### Liên kết thuật ngữ

**Liên kết:** [[Product]], [[Product Details]], [[Controller]], [[Action Method]], [[HomeController]], [[asp-route-id]], [[Razor View]], [[Danh mục]], [[UnitOfWork]], [[Dependency Injection]]

