## Program.cs - File cấu hình chính trong .NET Core MVC

### Khái niệm tổng quan

- Trong các phiên bản .NET Core cũ: sử dụng 2 file riêng biệt là `Program.cs` và `Startup.cs`
- Phiên bản mới: đội ngũ .NET đã gộp cả hai file thành một file duy nhất là `Program.cs`


### Hai nhiệm vụ chính khi cấu hình ứng dụng .NET

#### 1. Thêm services vào container (Services Registration)

- Sử dụng `builder.Services` để đăng ký các dịch vụ
- Ví dụ: `builder.Services.AddControllersWithViews()` - đăng ký dịch vụ [[MVC]] với controllers và views
- Trong tương lai sẽ thêm nhiều services khác qua [[Dependency Injection]]


#### 2. Cấu hình request pipeline (Request Pipeline Configuration)

- [[Pipeline]] = cách xử lý request khi nó đến ứng dụng
- Sử dụng các middleware để xử lý request theo thứ tự


### Cấu hình Environment Variables

```csharp
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
}
```

- `IsDevelopment()`: kiểm tra môi trường development (từ [[launch settings]])
- Các helper khác: `IsProduction()`, `IsStaging()`, `IsEnvironment("tên-môi-trường")`
- **Logic**:
    - Môi trường **không phải** development: sử dụng exception handler, redirect về trang lỗi
    - Môi trường development: hiển thị exception trực tiếp để debug


### Cấu hình Request Pipeline

Thứ tự các middleware quan trọng:

1. **HTTPS Redirection**: `app.UseHttpsRedirection()`
2. **Static Files**: `app.UseStaticFiles()` - cấu hình thư mục `wwwroot` cho các file tĩnh
3. **Routing**: `app.UseRouting()` - kích hoạt routing
4. **Authorization**: `app.UseAuthorization()` - xử lý phân quyền (sẽ học sau)

### Default Route Pattern

```csharp
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

- **Controller mặc định**: `Home`
- **Action mặc định**: `Index`
- **ID parameter**: tùy chọn (dấu `?` = có thể null)
- **Ý nghĩa**: nếu không định nghĩa route cụ thể, sẽ đi đến `HomeController.Index()`


### Khởi chạy ứng dụng

- `app.Run()`: khởi chạy ứng dụng và lắng nghe request


### Ghi chú quan trọng

- **Điều cần nhớ**: `Program.cs` là nơi duy nhất để:
    - Thêm services vào container
    - Cấu hình [[middleware]] pipeline
- Nội dung chi tiết sẽ được giải thích rõ hơn trong các bài học tiếp theo
- Hiện tại chỉ cần hiểu **vai trò** và **vị trí** của file này trong kiến trúc ứng dụng

---
**Liên kết**: [[MVC Architecture]], [[Dependency Injection]], [[Middleware Pipeline]], [[Environment Variables]]

