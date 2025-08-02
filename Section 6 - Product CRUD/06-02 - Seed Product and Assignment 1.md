## Cấu hình Product Model trong Database và DbContext

### Thêm Product vào ApplicationDbContext

Sau khi tạo Product model, cần thêm nó vào ApplicationDbContext để Entity Framework có thể quản lý:

```csharp
public DbSet<Product> Products { get; set; }
```

- Copy DbSet của Category và đổi tên thành Product
- Tên bảng trong database sẽ là "Products" (số nhiều)
- Cấu hình tương tự như đã làm với Category


### Cấu hình Seed Data cho Product

Để có dữ liệu mẫu khi tạo database, cần thêm seed data trong phương thức OnModelCreating:

```csharp
modelBuilder.Entity<Product>().HasData(
    // Dữ liệu 6 sản phẩm mẫu sẽ được thêm vào
);
```

- Copy cấu hình HasData từ Category
- Sử dụng dữ liệu có sẵn trong file snippets (resources/snippets/section six/seed products)
- Dữ liệu bao gồm đầy đủ 6 sản phẩm với tất cả thuộc tính đã định nghĩa trong Product model


### Bài tập thực hành

Sau khi cấu hình xong, cần thực hiện các bước sau:

**Bước 1: Tạo Product Table**

- Push Product model vào database thông qua Migration
- 6 bản ghi seed data sẽ tự động được tạo trong bảng

**Bước 2: Tạo Product Repository**

- Tạo interface IProductRepository
- Implement ProductRepository class
- Cấu hình các phương thức CRUD cơ bản

**Bước 3: Thêm vào UnitOfWork**

- Tích hợp ProductRepository vào UnitOfWork pattern
- Đảm bảo quản lý transaction nhất quán


### Ghi chú thêm

- Tất cả các bước thực hiện tương tự như đã làm với Category
- Đây là bài tập thực hành để củng cố kiến thức về Repository Pattern và UnitOfWork
- Cần hoàn thành trước khi chuyển sang video tiếp theo
- Dữ liệu seed phải khớp chính xác với thuộc tính trong Product model

**Liên kết:** [[Product Model]], [[ApplicationDbContext]], [[DbSet]], [[Migration]], [[Seed Data]], [[Repository Pattern]], [[UnitOfWork]], [[Entity Framework Core]], [[Category Repository]]

