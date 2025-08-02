## Seeding Data (Gieo dữ liệu) trong Entity Framework Core

### Vấn đề hiện tại

Trang Category List đã được tạo nhưng chưa có dữ liệu để hiển thị vì database không có categories nào. Thay vì thêm dữ liệu thủ công bằng cách "Edit Top 200 rows" trong SQL Server, [[Entity Framework Core]] cung cấp tính năng **seeding** để tạo dữ liệu mẫu tự động.

### Cách thực hiện Data Seeding

#### Override phương thức OnModelCreating

- Trong `ApplicationDbContext`, override phương thức mặc định `OnModelCreating`
- Phương thức này là built-in function của [[DbContext]] để cấu hình model

```csharp
public class ApplicationDbContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {
    }
    
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Seeding code ở đây
    }
}
```


#### Sử dụng ModelBuilder để seed data

- Tham số `ModelBuilder` được cung cấp để cấu hình entities
- Tương tự như `MigrationBuilder` trong migration files, nhưng dùng cho model configuration

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Category>().HasData(
        new Category { Id = 1, Name = "Action", DisplayOrder = 1 },
        new Category { Id = 2, Name = "SciFi", DisplayOrder = 2 },
        new Category { Id = 3, Name = "History", DisplayOrder = 3 }
    );
}
```


### Cú pháp HasData Method

#### Chức năng

- `HasData()` method dùng để thêm **seed data** cho entity type
- Nhận tham số là array của category objects
- Comment trong IntelliSense: *"Adds seed data to this entity type"*


#### Cấu trúc code

```csharp
modelBuilder.Entity<Category>()
    .HasData(
        new Category { Id = 1, Name = "Action", DisplayOrder = 1 },
        new Category { Id = 2, Name = "SciFi", DisplayOrder = 2 },
        new Category { Id = 3, Name = "History", DisplayOrder = 3 }
    );
```


#### Lưu ý quan trọng

- Phải cung cấp giá trị cho **Primary Key** (Id) khi seeding
- Có thể thêm nhiều records bằng cách ngăn cách bằng dấu phẩy
- Dữ liệu này sẽ được insert vào database khi migration được áp dụng


### Quy trình Apply Seed Data

#### Bước 1: Add Migration cho seed data

```bash
Add-Migration SeedCategoryTable
```

- Mọi thay đổi về database đều cần [[Migration]]
- **Quy tắc quan trọng**: "Always, always, always remember that"


#### Bước 2: Kiểm tra Migration file

Migration sẽ chứa lệnh `InsertData`:

```csharp
migrationBuilder.InsertData(
    table: "Categories",
    columns: new[] { "Id", "Name", "DisplayOrder" },
    values: new object[,]
    {
        { 1, "Action", 1 },
        { 2, "SciFi", 2 },
        { 3, "History", 3 }
    });
```


#### Bước 3: Update Database

```bash
Update-Database
```

- Áp dụng migration vào database
- Seed data sẽ được insert vào bảng Categories


### Kết quả

#### Kiểm tra trong SQL Server Management Studio

- Refresh bảng `Categories`
- Sẽ thấy 3 records mới: Action, SciFi, History
- Dữ liệu này sẵn sàng để retrieve và hiển thị trong ứng dụng


#### Bước tiếp theo

- Học cách retrieve (lấy) dữ liệu từ database
- Hiển thị danh sách categories trên trang Category List


### Ghi chú thêm

#### Khi nào sử dụng Data Seeding

- Tạo dữ liệu mẫu cho development/testing
- Khởi tạo dữ liệu cơ bản cần thiết cho ứng dụng
- Thay thế việc insert dữ liệu thủ công


#### Lưu ý về Migration và Seeding

- Seed data được quản lý thông qua [[Migration]] system
- Có thể rollback seed data nếu cần thông qua Down method
- Nên cẩn thận với Primary Key values để tránh conflict

**Liên kết:** [[Entity Framework Core]], [[Migration]], [[ApplicationDbContext]], [[ModelBuilder]], [[HasData]], [[Category Controller]]

