## Thêm thuộc tính ImageURL vào Product Model

### Khái niệm

Trong website bán sách, mỗi sản phẩm cần có hình ảnh để hiển thị cho khách hàng. Do đó, cần thêm thuộc tính **ImageURL** vào Product model để lưu trữ đường dẫn hình ảnh của sản phẩm.

### Cách thực hiện

**Bước 1: Thêm thuộc tính ImageURL**

Mở Product model trong thư mục Models và thêm thuộc tính mới:

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
    
    public int CategoryId { get; set; }
    
    [ForeignKey("CategoryId")]
    public Category Category { get; set; }
    
    // Thêm thuộc tính ImageURL
    public string ImageURL { get; set; }
}
```


### Xử lý Migration

**Bước 2: Tạo Migration**

```bash
Add-Migration AddImageURLToProduct
```

**Lỗi thường gặp:**

- Migration có thể thất bại do seed data không có giá trị ImageURL
- Cần cập nhật seed data trong ApplicationDbContext để thêm ImageURL = "" cho tất cả products

**Bước 3: Sửa Seed Data**

Trong ApplicationDbContext, cập nhật phương thức HasData:

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
        CategoryId = 1,
        ImageURL = ""  // Thêm ImageURL trống
    },
    new Product
    {
        Id = 2,
        // ... other properties
        CategoryId = 2,
        ImageURL = ""  // Thêm ImageURL trống
    }
    // ... cập nhật tương tự cho tất cả products
);
```

**Bước 4: Chạy Migration thành công**

Sau khi cập nhật seed data:

```bash
Add-Migration AddImageURLToProduct
Update-Database
```


### Kết quả

- Product model đã có đầy đủ tất cả thuộc tính cần thiết
- Database được cập nhật với cột ImageURL mới
- Seed data hoạt động bình thường với ImageURL trống
- Sẵn sàng cho việc implement upload và hiển thị hình ảnh trong các video tiếp theo


### Ghi chú thêm

- **ImageURL** hiện tại chỉ là string property để lưu đường dẫn
- Chức năng upload và quản lý hình ảnh sẽ được implement sau
- Thuộc tính này quan trọng cho user experience trong website bán hàng
- Cần đảm bảo tất cả seed data đều có ImageURL (có thể để trống ban đầu)
- Migration phải được test kỹ để tránh lỗi seed data

**Liên kết:** [[Product Model]], [[Migration]], [[ApplicationDbContext]], [[Seed Data]], [[Entity Framework Core]], [[Image Upload]], [[Database Schema]], [[Product Properties]]

