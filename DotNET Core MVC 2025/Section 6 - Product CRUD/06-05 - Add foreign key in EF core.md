## Tạo Foreign Key Relationship giữa Product và Category

### Khái niệm

Trong hệ thống bán sách, mỗi sản phẩm (Product) cần thuộc về một danh mục (Category) cụ thể như Action, Sci-Fi, hay History. Điều này yêu cầu tạo **quan hệ khóa ngoại (Foreign Key relationship)** giữa bảng Product và Category.

Với Entity Framework Core, thay vì phải tạo constraint trực tiếp trong SQL Server, chúng ta chỉ cần định nghĩa constraint trong model và EF Core sẽ xử lý phần còn lại.

### Cách thực hiện

**Bước 1: Thêm thuộc tính CategoryId**

```csharp
public class Product
{
    [Key]
    public int Id { get; set; }
    
    [Required]
    public string Title { get; set; }
    
    public string Description { get; set; }
    public string ISBN { get; set; }
    public string Author { get; set; }
    
    [Display(Name = "List Price")]
    [Range(1, 1000)]
    public double ListPrice { get; set; }
    
    [Display(Name = "Price")]
    [Range(1, 1000)]
    public double Price { get; set; }
    
    [Display(Name = "Price 50")]
    [Range(1, 1000)]
    public double Price50 { get; set; }
    
    [Display(Name = "Price 100")]
    [Range(1, 1000)]
    public double Price100 { get; set; }
    
    // Thêm CategoryId
    public int CategoryId { get; set; }
    
    // Navigation Property
    [ForeignKey("CategoryId")]
    public Category Category { get; set; }
}
```

**Bước 2: Định nghĩa Navigation Property và Foreign Key**

- **CategoryId**: Thuộc tính lưu trữ ID của Category
- **Category**: Navigation property để truy cập đối tượng Category
- **[ForeignKey("CategoryId")]**: Data annotation chỉ định CategoryId là khóa ngoại


### Cập nhật Seed Data

Sau khi thêm CategoryId, cần cập nhật seed data trong ApplicationDbContext:

```csharp
modelBuilder.Entity<Product>().HasData(
    new Product
    {
        Id = 1,
        Title = "Fortune of Time",
        Author = "Billy Spark",
        Description = "Praesent vitae sodales libero...",
        ISBN = "SWD9999001",
        ListPrice = 99,
        Price = 90,
        Price50 = 85,
        Price100 = 80,
        CategoryId = 1  // Gán Category ID
    },
    new Product
    {
        Id = 2,
        // ... other properties
        CategoryId = 2  // Gán Category ID khác
    },
    // ... các product khác với CategoryId tương ứng
);
```

**Lưu ý quan trọng:**

- CategoryId phải tồn tại trong bảng Categories
- Không được sử dụng ID không tồn tại (như 11) vì sẽ vi phạm foreign key constraint


### Quá trình Migration

**Bước 1: Tạo Migration đầu tiên (có vấn đề)**

```bash
Add-Migration AddForeignKeyForCategoryProductRelation
```

Migration này sẽ set tất cả CategoryId = 0, không phù hợp với seed data.

**Bước 2: Xóa Migration không mong muốn**

```bash
Remove-Migration
```

Hoặc xóa trực tiếp file migration trong thư mục Migrations.

**Bước 3: Tạo Migration mới sau khi sửa seed data**

```bash
Add-Migration AddForeignKeyForCategoryProductRelation
Update-Database
```


### Kiểm tra kết quả

**Trong SQL Server Management Studio:**

- Bảng Products sẽ có cột CategoryId
- Trong phần Keys, sẽ thấy foreign key constraint giữa Product và Categories
- Thử insert giá trị CategoryId không tồn tại (như 11) sẽ gây lỗi foreign key violation

**Cấu trúc database:**

```sql
-- Product table sẽ có:
CategoryId (int, Foreign Key to Categories.Id)

-- Foreign Key Constraint sẽ được tạo tự động:
FK_Products_Categories_CategoryId
```


### Ưu điểm của Entity Framework Core

- **Tự động hóa**: Không cần viết SQL constraint thủ công
- **Type Safety**: Navigation property đảm bảo type safety
- **IntelliSense**: Hỗ trợ autocomplete khi truy cập Category properties
- **Lazy Loading**: Có thể load Category data khi cần thiết


### Ghi chú thêm

- **Data Annotation [ForeignKey]** là cách explicit để định nghĩa foreign key
- Entity Framework Core cũng có thể tự động detect foreign key theo convention (thuộc tính có tên `<NavigationPropertyName>Id`)
- Navigation property cho phép truy cập dễ dàng: `product.Category.Name`
- Foreign key constraint sẽ ngăn chặn việc xóa Category đang được sử dụng bởi Product
- Khi thay đổi model, luôn cần tạo migration mới để cập nhật database schema

**Liên kết:** [[Product Model]], [[Category Model]], [[Foreign Key]], [[Navigation Property]], [[Entity Framework Core]], [[Migration]], [[ApplicationDbContext]], [[Data Annotations]], [[Seed Data]], [[Database Constraints]]

