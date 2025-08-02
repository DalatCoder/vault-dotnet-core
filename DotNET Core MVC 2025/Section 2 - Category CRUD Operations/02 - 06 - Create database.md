## Tạo Database sử dụng Entity Framework Core

### Hoàn tất cấu hình Entity Framework Core

Việc thiết lập [[Entity Framework Core]] đã hoàn tất với những bước cơ bản:

- Thêm vài dòng code trong `Program.cs`
- Tạo class `ApplicationDbContext` kế thừa từ `DbContext`
- Cấu hình đã sẵn sàng để sử dụng


### Tạo Database từ Connection String

#### Kiểm tra Database hiện tại

- Kết nối đến SQL Server (server: `.`)
- Kiểm tra danh sách database - chưa có database tên `Bulky`


#### Sử dụng Package Manager Console

- Truy cập: **Tools** → **NuGet Package Manager** → **Package Manager Console**
- Công cụ này cho phép chạy các lệnh [[Entity Framework Core]]
- Cần package `Microsoft.EntityFrameworkCore.Tools` (đã cài đặt trước đó)


### Lệnh tạo Database

#### Lệnh cơ bản

```
Update-Database
```

- Lệnh này tạo database dựa trên [[Connection String]] đã cấu hình
- Không cần model hay migration để tạo database trống


#### Xử lý lỗi thường gặp

**Lỗi 1: Connection string property chưa được khởi tạo**

```csharp
// Sai trong Program.cs
builder.Configuration.GetConnectionString("DefaultConnection1")

// Đúng - phải khớp với appsettings.json
builder.Configuration.GetConnectionString("DefaultConnection")
```

**Lỗi 2: Keyword không được hỗ trợ (Server)**

```json
// Sai trong appsettings.json
"Server:."

// Đúng - sử dụng dấu = thay vì :
"Server=."
```


### Kết quả sau khi chạy thành công

#### Thông báo trong Console

```
No migrations were applied. Database is already up to date.
```


#### Kiểm tra trong SQL Server Management Studio

- Refresh danh sách database
- Xuất hiện database mới: `Bulky`
- Bên trong có bảng: `dbo.__EFMigrationsHistory`


### Khái niệm Migration (sơ lược)

- **Migration History** là bảng theo dõi các thay đổi cấu trúc database
- **Migration** là quá trình tạo và cập nhật cấu trúc database từ models
- Sẽ được giải thích chi tiết trong các bài tiếp theo


### Bước tiếp theo

- Tạo bảng (table) cho model `Category`
- Bảng sẽ có các cột tương ứng với properties trong model:
    - Id
    - Name
    - DisplayOrder


### Ghi chú quan trọng

- Lệnh `Update-Database` chỉ tạo database trống ban đầu
- Để tạo bảng từ models, cần sử dụng **Add-Migration** trước
- Cần đảm bảo [[Connection String]] chính xác và có thể kết nối được
- Mọi lỗi cú pháp trong connection string sẽ gây ra lỗi khi chạy lệnh

**Liên kết:** [[Entity Framework Core]], [[Connection String]], [[ApplicationDbContext]], [[Migration]], [[Package Manager Console]]

