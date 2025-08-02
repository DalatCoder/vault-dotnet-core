## Hoàn thành Product Repository và Migration

### Tạo Migration và Cập nhật Database

**Bước 1: Khắc phục lỗi Migration**

- Mở Package Manager Console trong Visual Studio
- Đảm bảo **Default Project** được chọn là `Data Access` thay vì `Web Project`
- Kiểm tra lại cấu hình `HasData` trong ApplicationDbContext phải là `Product` (không phải category)

**Bước 2: Chạy Migration Commands**

```bash
Add-Migration AddProductsToDB
Update-Database
```

**Kết quả:**

- Bảng Products được tạo thành công trong database
- 6 bản ghi seed data được thêm tự động thông qua migration
- Có thể kiểm tra trong SQL Server Management Studio để xác nhận dữ liệu đã được tạo


### Tạo Product Repository Interface

Tạo file `IProductRepository.cs` dựa trên Category Repository:

```csharp
public interface IProductRepository : IRepository<Product>
{
    void Update(Product obj);
}
```

- Copy từ `ICategoryRepository` và đổi tên thành `IProductRepository`
- Kế thừa từ `IRepository<Product>` thay vì `IRepository<Category>`
- Phương thức `Update` nhận tham số kiểu `Product`


### Implement Product Repository Class

Tạo file `ProductRepository.cs`:

```csharp
public class ProductRepository : Repository<Product>, IProductRepository
{
    private ApplicationDbContext _db;
    
    public ProductRepository(ApplicationDbContext db) : base(db)
    {
        _db = db;
    }
    
    public void Update(Product obj)
    {
        _db.Products.Update(obj);
    }
}
```

**Các thay đổi chính:**

- Class name: `ProductRepository`
- Constructor name: `ProductRepository`
- Kế thừa từ `Repository<Product>`
- Implement `IProductRepository`
- Phương thức Update sử dụng `_db.Products.Update(obj)`


### Tích hợp vào UnitOfWork

**Cập nhật IUnitOfWork Interface:**

```csharp
public interface IUnitOfWork
{
    ICategoryRepository Category { get; }
    IProductRepository Product { get; }
    void Save();
}
```

**Cập nhật UnitOfWork Implementation:**

```csharp
public class UnitOfWork : IUnitOfWork
{
    private ApplicationDbContext _db;
    
    public ICategoryRepository Category { get; private set; }
    public IProductRepository Product { get; private set; }
    
    public UnitOfWork(ApplicationDbContext db)
    {
        _db = db;
        Category = new CategoryRepository(_db);
        Product = new ProductRepository(_db);
    }
    
    public void Save()
    {
        _db.SaveChanges();
    }
}
```


### Ghi chú thêm

- Tất cả các bước thực hiện tương tự như với Category Repository
- Đảm bảo Default Project trong Package Manager Console được chọn đúng là Data Access
- Migration sẽ tự động seed dữ liệu vào bảng Products
- Cấu trúc Repository Pattern và UnitOfWork giúp quản lý data access một cách nhất quán
- Bước tiếp theo sẽ là tạo Controller và View để thực hiện CRUD operations cho Product

**Liên kết:** [[Product Model]], [[Repository Pattern]], [[UnitOfWork]], [[Migration]], [[Entity Framework Core]], [[ApplicationDbContext]], [[Package Manager Console]], [[Category Repository]], [[Database Seeding]]

