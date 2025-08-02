## Thiết lập Authentication và Authorization với Identity trong .NET

### Giới thiệu về Identity

- *Identity* trong .NET hỗ trợ các chức năng như đăng ký (registration), đăng nhập (log-in), quản lý mật khẩu, tạo bảng quản lý người dùng và đảm bảo an ninh cho ứng dụng.
- .NET cung cấp sẵn một hệ thống *Identity* mặc định do chính đội ngũ phát triển duy trì, giúp tiết kiệm thời gian và đảm bảo an toàn bảo mật cho dự án.
- Khi triển khai *Identity*, lập trình viên không cần tự viết lại các chức năng này — chỉ cần cấu hình và tận dụng thư viện có sẵn.


### Thêm Identity vào Dự Án

- Trong dự án web, nhấn chuột phải → *Add* → *New Scaffolded Item* → chọn *Identity*.
- Tại đây, bạn có thể chọn các trang cần thiết (login, register, logout, set password, reset password, ...). Không nhất thiết phải scaffold tất cả.
- Có thể chọn giao diện master page tùy ý, hoặc giữ mặc định khi thêm các trang *Identity*.


### Cấu hình DbContext cho Identity

- Để sử dụng *Identity*, DbContext cần kế thừa từ `IdentityDbContext` thay vì chỉ là `DbContext` thông thường.
- Trong file ApplicationDbContext:
    - Sửa lại để kế thừa từ `IdentityDbContext`.
    - Cài đặt package NuGet: `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.
    - Thêm dòng `using` tương ứng sau khi cài đặt xong package để tránh lỗi.


#### Ví dụ thay đổi kế thừa DbContext:

```csharp
public class ApplicationDbContext : IdentityDbContext
{
    // ... các cấu hình khác
}
```

- Khi chuyển sang dùng `IdentityDbContext`, cần đảm bảo phương thức `OnModelCreating` gọi thêm `base.OnModelCreating(modelBuilder)`. Nếu không, sẽ gặp lỗi như:
> "The entity type IdentityUserLogin requires a primary key to be defined."
- Đoạn mã cần thêm vào:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);
    // ... Các thiết lập khác (nếu có)
}
```


### Scaffolding và cập nhật cấu trúc project

- Sau khi cấu hình đúng `ApplicationDbContext`, quay lại quá trình scaffold, hệ thống sẽ tự động nhận diện DbContext mới.
- Chọn *Scaffold* để thêm các trang liên quan đến *Identity*.
- Quá trình này sẽ cập nhật và thêm nhiều file (có thể lên tới hàng chục file mới cho các chức năng liên quan).
- Sau khi hoàn tất, các trang như đăng nhập, đăng ký, quản lý tài khoản… sẽ được tạo tự động và tích hợp vào ứng dụng.


### Ghi chú

- Nếu có cập nhật trên các phiên bản .NET mới hơn (như .NET 8), cần kiểm tra để thực hiện theo hướng dẫn hoặc cập nhật mới nhất.
- Sau khi hoàn tất quá trình trên, mọi chức năng liên quan đến xác thực người dùng đã sẵn sàng sử dụng mà không cần phải tự xây dựng thủ công.


### Liên kết:

[[Identity]], [[ApplicationDbContext]], [[IdentityDbContext]], [[OnModelCreating]], [[Scaffolding]], [[Authentication]], [[Authorization]]

