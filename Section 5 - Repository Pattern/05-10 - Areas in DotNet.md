## Areas trong ASP.NET Core MVC

### Khái niệm

**Areas** (khu vực) là tính năng trong ASP.NET Core MVC cho phép chia nhỏ dự án thành các phần riêng biệt, tạo thêm một tầng phân cấp trên controller và action.

#### Cấu trúc routing mở rộng

- **Trước đây**: Controller → Action
- **Với Areas**: Area → Controller → Action


#### Ứng dụng thực tế

- **Admin panel**: Quản trị viên
- **Customer area**: Khách hàng
- **Different modules**: Các module khác nhau trong dự án lớn


### Cách thêm Areas vào dự án

#### Bước 1: Scaffold MVC Area

- Click phải vào project → **Add** → **New Scaffolded Item**
- Chọn **MVC Area**
- Đặt tên area (ví dụ: `Admin`, `Customer`)


#### Bước 2: Cập nhật Routing

```csharp
// Program.cs
app.MapControllerRoute(
    name: "default",
    pattern: "{area=Customer}/{controller=Home}/{action=Index}/{id?}"
);
```

- Thêm parameter `{area=Customer}` vào route pattern
- Đặt area mặc định là `Customer`


#### Bước 3: Di chuyển Controllers

- **CategoryController** → Areas/Admin/Controllers/
- **HomeController** → Areas/Customer/Controllers/


#### Bước 4: Thêm Area Attribute

```csharp
[Area("Admin")]
public class CategoryController : Controller
{
    // Controller code...
}

[Area("Customer")]  
public class HomeController : Controller
{
    // Controller code...
}
```


#### Bước 5: Di chuyển Views

- **Category views** → Areas/Admin/Views/Category/
- **Home views** → Areas/Customer/Views/Home/


#### Bước 6: Copy View Files cần thiết

- Copy `_ViewStart.cshtml` và `_ViewImports.cshtml` vào:
    - Areas/Admin/Views/
    - Areas/Customer/Views/


### Cập nhật Navigation

#### Thêm asp-area vào links

```html
<!-- Trước khi có Areas -->
<a asp-controller="Category" asp-action="Index">Category</a>

<!-- Sau khi có Areas -->
<a asp-area="Admin" asp-controller="Category" asp-action="Index">Category</a>
<a asp-area="Customer" asp-controller="Home" asp-action="Index">Home</a>
```


#### Quy tắc Area trong navigation

- **Cùng area**: Có thể bỏ qua `asp-area` (tự động sử dụng area hiện tại)
- **Khác area**: Bắt buộc phải chỉ định `asp-area`


### Cấu trúc thư mục sau khi có Areas

```
BulkyBookWeb/
├── Areas/
│   ├── Admin/
│   │   ├── Controllers/
│   │   │   └── CategoryController.cs
│   │   └── Views/
│   │       ├── Category/
│   │       ├── _ViewStart.cshtml
│   │       └── _ViewImports.cshtml
│   └── Customer/
│       ├── Controllers/
│       │   └── HomeController.cs
│       └── Views/
│           ├── Home/
│           ├── _ViewStart.cshtml
│           └── _ViewImports.cshtml
```


### Lỗi thường gặp và cách khắc phục

#### Lỗi "View was not found"

- **Nguyên nhân**: Di chuyển controller nhưng chưa di chuyển views
- **Khắc phục**: Di chuyển views tương ứng vào area folder


#### Lỗi Namespace

- **Nguyên nhân**: Namespace không được cập nhật khi di chuyển
- **Khắc phục**: Cập nhật namespace thủ công nếu cần


#### Lỗi thiếu _ViewImports

- **Nguyên nhân**: Model binding không hoạt động
- **Khắc phục**: Copy `_ViewImports.cshtml` vào từng area


### Ưu điểm của Areas

#### Tổ chức code tốt hơn

- Tách biệt rõ ràng giữa các chức năng
- Dễ maintain và phát triển
- Team có thể làm việc độc lập trên các areas khác nhau


#### Flexibility trong routing

- URL structure rõ ràng hơn
- Dễ dàng mở rộng và thêm areas mới


#### Security và Authorization

- Có thể áp dụng authorization riêng cho từng area
- Admin area có thể có security policy khác Customer area


### Ghi chú quan trọng

#### Default Area

- Area mặc định được định nghĩa trong routing
- Nếu không chỉ định area, sẽ sử dụng area mặc định


#### Navigation trong cùng Area

- Khi ở trong một area, có thể bỏ qua `asp-area` attribute
- Hệ thống tự động sử dụng area hiện tại


#### Best Practices

- **Admin functions**: Đặt trong Admin area
- **Customer-facing features**: Đặt trong Customer area
- **API controllers**: Có thể tạo API area riêng

**Liên kết:** [[ASP.NET Core MVC]], [[Controller]], [[Action Method]], [[Routing]], [[Scaffolding]], [[Area Attribute]], [[View]], [[Navigation]], [[Program.cs]], [[MVC Pattern]]

