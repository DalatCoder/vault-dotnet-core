## Thiết lập kết nối giữa Database và Entity Framework Core

### Tạo ApplicationDbContext Class

#### Tạo cấu trúc thư mục và file

- Tạo thư mục mới: `Data` trong project chính
- Tạo class mới: `ApplicationDbContext.cs` trong thư mục Data
- Tên class có thể tùy chỉnh, nhưng `ApplicationDbContext` là cách đặt tên phổ biến


#### Cấu hình cơ bản cho Entity Framework Core

```csharp
using Microsoft.EntityFrameworkCore;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {
    }
}
```


### Khái niệm DbContext

- **DbContext** là class gốc của [[Entity Framework Core]]
- Mọi class muốn sử dụng Entity Framework Core phải kế thừa từ `DbContext`
- DbContext cho phép truy cập và thao tác với cơ sở dữ liệu


### Cấu hình Constructor

#### Tại sao cần DbContextOptions?

- Constructor nhận tham số `DbContextOptions<ApplicationDbContext>`
- Tham số này chứa thông tin cấu hình (bao gồm [[Connection String]])
- Sử dụng `base(options)` để truyền cấu hình lên class cha `DbContext`


#### Cú pháp tạo constructor nhanh

- Sử dụng snippet `ctor` và nhấn Tab 2 lần để tạo constructor tự động


### Đăng ký DbContext trong Program.cs

#### Vị trí đăng ký

- Tất cả services được đăng ký trong `Program.cs`
- Thêm vào block `builder.Services` trước dòng `builder.Build()`


#### Cấu hình cơ bản

```csharp
using Bulky.Data;  
using Microsoft.EntityFrameworkCore;  
  
var builder = WebApplication.CreateBuilder(args);  
  
// Add services to the container.  
builder.Services.AddControllersWithViews();  
builder.Services.AddDbContext<ApplicationDbContext>(options =>  
    options.UseMySql(  
        builder.Configuration.GetConnectionString("DefaultConnection"),  
        ServerVersion.AutoDetect(builder.Configuration.GetConnectionString("DefaultConnection"))  
    )  
);
```


### Giải thích từng thành phần

#### AddDbContext

- `builder.Services.AddDbContext()` - báo cho ứng dụng biết sử dụng [[Entity Framework Core]]
- Chỉ định class implementation: `ApplicationDbContext`


#### UseSqlServer

- `options.UseSqlServer()` - cấu hình sử dụng [[SQL Server]] làm database provider
- Phương thức này nằm trong package `Microsoft.EntityFrameworkCore.SqlServer`


#### GetConnectionString

- `builder.Configuration.GetConnectionString()` - phương thức helper tích hợp sẵn
- Tự động truy xuất section `ConnectionStrings` từ `appsettings.json`
- Truyền tên key để lấy connection string cụ thể (ví dụ: `"DefaultConnection"`)


### Cấu trúc liên kết với appsettings.json

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=.;Database=Bulky;Trusted_Connection=true;TrustServerCertificate=true;"
  }
}
```

- Section name: `ConnectionStrings` (cố định)
- Key name: `DefaultConnection` (có thể tùy chỉnh)
- Value: chuỗi kết nối database


### Ghi chú quan trọng

- Đây là **cấu hình cơ bản bắt buộc** cho [[Entity Framework Core]]
- Các bước này là syntax cố định, cần thực hiện đúng thứ tự
- Nếu key name trong `GetConnectionString()` không khớp với `appsettings.json`, sẽ xảy ra lỗi
- Tất cả packages [[Microsoft Entity Framework]] phải cùng version để tránh xung đột


### Lưu ý về Debug

- Nên kiểm tra chính tả key name giữa `Program.cs` và `appsettings.json`
- Ví dụ lỗi thường gặp: `"DefaultConnection"` vs `"DefaultConnection1"`

**Liên kết:** [[Entity Framework Core]], [[SQL Server]], [[Connection String]], [[DbContext]], [[Program.cs Configuration]]

