## Hiển thị Danh sách Sản phẩm trên Trang Chủ với .NET Core MVC

### Khái niệm

- Trang chủ của ứng dụng sẽ hiển thị tất cả sản phẩm.
- Người dùng có thể thấy danh sách này mà không cần đăng nhập admin.
- Cần lấy dữ liệu sản phẩm cùng thông tin *danh mục* (Category) từ cơ sở dữ liệu.


### Xử lý trong Controller

- Làm việc trong khu vực `Customer`, cụ thể là ở HomeController, action Index.
- Sử dụng mẫu *Unit of Work* với *Dependency Injection* để quản lý truy cập dữ liệu.
- Inject `IUnitOfWork` vào controller:

```csharp
private readonly IUnitOfWork _unitOfWork;

public HomeController(IUnitOfWork unitOfWork)
{
    _unitOfWork = unitOfWork;
}
```

- Trong action `Index`, truy vấn danh sách sản phẩm, bao gồm dữ liệu liên quan về danh mục:

```csharp
public IActionResult Index()
{
    IEnumerable<Product> productList = _unitOfWork.Product.GetAll(includeProperties: "Category");
    return View(productList);
}
```


### Xử lý trong View

- Model của view là `IEnumerable<Product>`.
- Sử dụng Razor syntax để thao tác với dữ liệu.
- Cấu trúc Bootstrap được dùng để bố cục hiển thị sản phẩm.

```csharp
@model IEnumerable<Product>
<div class="row pb-3">
    @foreach (var product in Model)
    {
        <div class="col-lg-3 col-sm-6">
            <div class="row p-2">
                <div class="col-12 p-1">
                    <div class="card border-3 p-3 shadow rounded">
                        <img src="@product.ImageUrl" class="card-img-top rounded" />
                        <div class="card-body pb-0">
                            <div class="pl-1">
                                <p class="card-title">@product.Title</p>
                                <p>@product.Author</p>
                            </div>
                            <div class="pl-1">
                                <p class="text-dark text-opacity-75 text-center mb-0">
                                    <span class="text-decoration-line-through">@product.ListPrice.ToString("c")</span>
                                </p>
                                <p class="text-dark opacity-75 text-center mb-0">
                                    As low as <span>@product.Price100.ToString("c")</span>
                                </p>
                            </div>
                        </div>
                        <div>
                            <a href="#" class="btn btn-primary">Details</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    }
</div>
```

- Chú ý các đoạn code sử dụng class Bootstrap như `col-lg-3`, `col-sm-6`, `card`, `shadow`, `rounded` để tăng tính thẩm mỹ.


### Ghi chú thêm

- Kiểm tra kỹ các thẻ div đóng/mở để tránh lỗi bố cục.
- Luôn định dạng lại code (Ctrl+A, Ctrl+KD) để code dễ đọc và bảo trì.
- Giá sản phẩm được format sang kiểu tiền tệ với `"c"`.
- Có thể mở rộng liên kết chi tiết sản phẩm ở thẻ `<a>` với đường dẫn động về trang chi tiết.


### Liên kết thuật ngữ

**Liên kết:** [[Product]], [[Category]], [[Dependency Injection]], [[Unit of Work]], [[IUnitOfWork]], [[Product Controller]], [[Bootstrap]], [[Razor View]], [[Trang chủ]], [[Model-View-Controller]], [[Index Action]], [[Chi tiết sản phẩm]]

