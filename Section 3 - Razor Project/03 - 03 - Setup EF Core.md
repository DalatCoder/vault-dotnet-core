## Cấu hình Entity Framework Core trong dự án Razor Pages

### Cài đặt NuGet Packages

#### Phương pháp 1: NuGet Package Manager

- Click chuột phải vào Solution → chọn **Manage NuGet packages for solution**
- Chọn các package đã cài trong dự án MVC (bulky web)
- Tích chọn thêm dự án Razor Pages để cài đặt cùng version


#### Phương pháp 2: Chỉnh sửa Project File (khuyến nghị)

- Click chuột phải dự án MVC → **Edit project file**
- Copy toàn bộ các NuGet packages (bao gồm version)
- Mở project file của dự án Razor Pages
- Xóa package hiện có và paste các package đã copy
- Hệ thống sẽ tự động cài đặt tất cả packages

**Ưu điểm phương pháp 2:**

- Project file là vị trí cuối cúng lưu trữ thông tin NuGet packages
- Tự động hóa quá trình cài đặt
- Đảm bảo đồng bộ version giữa các dự án


### Tạo ApplicationDbContext

#### Các bước thực hiện:

- Tạo thư mục `Data` trong dự án Razor Pages
- Tạo class `ApplicationDbContext`
- Copy nội dung từ dự án MVC (nhưng **nên tự gõ lại** để luyện tập)


#### Nội dung cần có:

- Kế thừa từ `DbContext`
- Khai báo `DbSet<Category> Categories`
- Override phương thức `OnModelCreating()` để seed dữ liệu
- Sử dụng `HasData()` để thêm dữ liệu mẫu

**Lưu ý quan trọng:** Sử dụng namespace của dự án hiện tại (`bulky web razor`), không copy namespace từ dự án MVC

### Cấu hình Program.cs

#### Thêm Entity Framework service:

```csharp
builder.Services.AddDbContext<ApplicationDbContext>(options => 
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```


#### Thêm using statement:

- Import namespace cho `ApplicationDbContext`
- Import `Microsoft.EntityFrameworkCore`


### Tạo Migration và Database

#### Sử dụng Package Manager Console:

- **Tools** → **Package Manager Console**
- **Quan trọng:** Chọn đúng **Default project** là dự án Razor Pages
- Chạy lệnh: `Add-Migration AddCategoryToDB`
- Kiểm tra file migration được tạo (phải có tạo bảng Category và insert dữ liệu)
- Chạy lệnh: `Update-Database`


#### Lưu ý về Default Project:

- Khi có nhiều dự án trong solution, phải chọn đúng dự án đích
- Migration sẽ được tạo trong dự án được chọn làm default
- Nếu chọn sai, migration sẽ được tạo ở dự án không mong muốn


### Kiểm tra kết quả

#### Trong SQL Server:

- Database mới: `bulky_razor`
- Bảng `Categories` với 3 dòng dữ liệu mẫu
- Cấu trúc giống hệt dự án MVC


#### Ưu điểm của Entity Framework Core:

- Thiết lập nhanh chóng trong ứng dụng .NET
- Code-first approach thuận tiện
- Migration tự động quản lý thay đổi database schema
- Khả năng seed dữ liệu tự động


### Ghi chú thêm

- Việc tự gõ lại code thay vì copy-paste giúp hiểu sâu hơn về cách cấu hình
- Package Manager Console là công cụ mạnh mẽ để quản lý EF Core
- Connection string cần thay đổi tên database để tránh conflict
- Entity Framework Core giúp setup database rất nhanh trong .NET applications

**Liên kết:** [[Entity Framework Core]], [[NuGet Packages]], [[ApplicationDbContext]], [[Migration]], [[Package Manager Console]], [[DbContext]], [[HasData]], [[OnModelCreating]], [[Program.cs]], [[Connection String]]

