## Tổng hợp thay đổi sau khi triển khai Identity vào dự án .NET

### 1. Thay đổi trong ApplicationDbContext

- Thêm hoặc điều chỉnh phương thức `OnModelCreating` để gọi `base.OnModelCreating(modelBuilder)`, đảm bảo Identity mapping chính xác trong database.
- Giữ lại một phiên bản ApplicationDbContext kế thừa từ `IdentityDbContext`.


### 2. Thay đổi trong Program.cs

- Khi scaffold Identity, hệ thống sẽ thêm cấu hình dịch vụ sau vào Program.cs:

```csharp
builder.Services.AddDefaultIdentity<IdentityUser>(options =>
{
    options.SignIn.RequireConfirmedAccount = false; // Có thể bật/tắt xác thực email khi đăng nhập
})
.AddEntityFrameworkStores<ApplicationDbContext>();
```

- `AddEntityFrameworkStores<ApplicationDbContext>()`: chỉ định Entity Framework sẽ sử dụng `ApplicationDbContext` để quản lý các bảng liên quan đến Identity (user, roles, claims...).
- Luồng xử lý xác thực:
    - **Authentication** (`app.UseAuthentication();`) kiểm tra danh tính đăng nhập (username/password).
    - **Authorization** (`app.UseAuthorization();`) kiểm tra quyền truy cập (theo vai trò).
    - Luôn gọi `UseAuthentication()` trước `UseAuthorization()` trong pipeline.


### 3. Chỉnh sửa trong appsettings.json

- Có thể hệ thống sẽ tự động thêm một chuỗi kết nối Database mới khi scaffold, nhưng bạn chỉ nên giữ lại chuỗi kết nối bạn đang sử dụng và xóa chuỗi không cần thiết.


### 4. Partial View: _LoginPartial.cshtml

- Scaffold tự động tạo ra file partial view `_LoginPartial.cshtml` trong thư mục `Views/Shared`.
- File này chứa các liên kết Đăng nhập (Login), Đăng ký (Register), Đăng xuất (Logout), và liên kết quản lý tài khoản nếu user đã đăng nhập.


### 5. Thư mục Areas/Identity

- Xuất hiện thư mục mới: `Areas/Identity` chứa các trang liên quan tới Identity user (login, logout, register, set password, ...).
- Tất cả các trang này là Razor Pages, không phải MVC View. Razor Pages có file `.cshtml` chứa giao diện và file `.cshtml.cs` chứa logic (PageModel).
- Microsoft chuyển hoàn toàn Identity sang Razor Pages để giảm tải việc bảo trì framework.


### 6. Hiển thị nút Đăng nhập/Đăng ký trên trang web

- Để hiện 2 nút này trên thanh điều hướng, chỉ cần thêm partial view `_LoginPartial` vào trong file `_Layout.cshtml`:

```csharp
@await Html.PartialAsync("_LoginPartial")
```

- Đặt ngay sau phần `ul` trong `navbar-nav` để chúng xuất hiện đúng vị trí trên giao diện.


### 7. Kết quả

- Khi chạy dự án, phần giao diện sẽ có hai nút **REGISTER** và **LOGIN** ở thanh điều hướng. Khi nhấn vào các nút này, sẽ điều hướng đến Razor Page của Identity tương ứng.
- Nếu có vấn đề không hiển thị hoặc giao diện chưa đúng, kiểm tra lại việc nhúng partial view và CSS.

**Liên kết:**
[[Identity]], [[ApplicationDbContext]], [[IdentityDbContext]], [[OnModelCreating]], [[AddDefaultIdentity]], [[AddEntityFrameworkStores]], [[Authentication]], [[Authorization]], [[Razor Pages]], [[Partial View]], [[_Layout.cshtml]], [[_LoginPartial.cshtml]], [[Areas/Identity]]

