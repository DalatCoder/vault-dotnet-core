## Tạo Table từ Model sử dụng Migration trong Entity Framework Core

### Sức mạnh của Entity Framework Core

Entity Framework Core cho phép tạo bảng cơ sở dữ liệu trực tiếp từ code mà không cần viết câu lệnh SQL thủ công. Đây là một trong những tính năng mạnh mẽ nhất của [[Entity Framework Core]].

### Định nghĩa Primary Key

#### Lỗi khi không có Primary Key

- Nếu đặt tên property là `Category21Id` (không theo quy tắc) và không có `[Key]`
- [[Entity Framework Core]] sẽ báo lỗi: *"The entity type 'Category' requires a primary key to be defined"*


#### Quy tắc tự động nhận diện Primary Key

- Property tên `Id` → tự động là primary key
- Property theo format `[ModelName]Id` (ví dụ: `CategoryId`) → tự động là primary key
- Hoặc sử dụng `[Key]` data annotation để định nghĩa rõ ràng


### Tạo DbSet trong ApplicationDbContext

#### Cú pháp cơ bản

```csharp
public class ApplicationDbContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {
    }
}
```


#### Giải thích

- `DbSet<Category>` - định nghĩa entity type sẽ được map với bảng database
- `Categories` - tên property sẽ trở thành tên bảng trong database
- Chỉ cần **một dòng code** này để tạo toàn bộ bảng với các cột tương ứng


### Quy trình Migration (Di chuyển)

#### Bước 1: Add Migration

```bash
Add-Migration AddCategoryTableToDB
```

- Sử dụng **Package Manager Console**: Tools → NuGet Package Manager → Package Manager Console
- `Add-Migration` + tên có ý nghĩa cho migration
- Lệnh này tạo file migration code trong thư mục `Migrations`


#### Bước 2: Update Database

```bash
Update-Database
```

- Áp dụng migration vào database thực tế
- [[Entity Framework Core]] sẽ chuyển đổi migration code thành SQL commands
- Tạo bảng với đúng cấu trúc đã định nghĩa


### Cấu trúc Migration File

#### File tự động được tạo

- Thư mục `Migrations` được tạo tự động
- File name format: `[Timestamp]_[MigrationName].cs`
- Ví dụ: `20250726_AddCategoryTableToDB.cs`


#### Nội dung Migration

```csharp
public partial class AddCategoryTableToDB : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.CreateTable(
            name: "Categories",
            columns: table => new
            {
                Id = table.Column<int>(nullable: false)
                    .Annotation("SqlServer:Identity", "1, 1"),
                Name = table.Column<string>(nullable: false),
                DisplayOrder = table.Column<int>(nullable: false)
            },
            constraints: table =>
            {
                table.PrimaryKey("PK_Categories", x => x.Id);
            });
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.DropTable(name: "Categories");
    }
}
```


#### Chức năng Up và Down

- **Up method**: Thực hiện thay đổi (tạo bảng, thêm cột...)
- **Down method**: Rollback thay đổi (xóa bảng, xóa cột...)


### Cơ chế theo dõi Migration

#### Bảng __EFMigrationsHistory

- [[Entity Framework Core]] tự động tạo bảng `__EFMigrationsHistory`
- Lưu trữ thông tin các migration đã được áp dụng
- Khi chạy `Update-Database`, EF Core kiểm tra bảng này để biết migration nào chưa được áp dụng


#### Truy vấn kiểm tra

```sql
SELECT TOP 1000 * FROM __EFMigrationsHistory
```

- Ban đầu bảng trống (chưa có migration nào)
- Sau khi `Update-Database`, sẽ có record của migration `AddCategoryTableToDB`


### Kết quả sau khi Migration

#### Trong Database

- Bảng `Categories` được tạo với 3 cột: `Id`, `Name`, `DisplayOrder`
- Cột `Id` được thiết lập làm Primary Key với Identity (auto-increment)
- Tên bảng khớp với tên DbSet property (`Categories`)


#### Tracking trong Migration History

- Record mới trong `__EFMigrationsHistory` với tên migration
- [[Entity Framework Core]] biết migration này đã được áp dụng


### Tóm tắt quy trình hoàn chỉnh

#### Các bước cần thiết

1. **Tạo Model** - định nghĩa class với các properties
2. **Thêm DbSet** - trong `ApplicationDbContext`
3. **Add Migration** - tạo migration file từ changes
4. **Update Database** - áp dụng migration vào database

#### Lợi ích của Migration

- Không cần viết SQL commands thủ công
- Tự động chuyển đổi từ C\# code sang SQL
- Có thể rollback thay đổi nếu cần
- Theo dõi lịch sử thay đổi database
- Làm việc với code yêu thích thay vì trực tiếp với database


### Ghi chú quan trọng

- Migration là tính năng cốt lõi của [[Entity Framework Core]]
- Mọi thay đổi về cấu trúc database nên thông qua migration
- Migration files nên được commit vào source control
- Có thể sử dụng `Remove-Migration` để undo migration chưa apply

**Liên kết:** [[Entity Framework Core]], [[ApplicationDbContext]], [[DbSet]], [[Package Manager Console]], [[Primary Key]], [[Data Annotations]]

