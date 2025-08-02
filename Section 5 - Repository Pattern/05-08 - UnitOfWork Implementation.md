## Unit of Work Pattern trong .NET Core

### Vấn đề với Repository Pattern hiện tại

#### Hạn chế của cách tiếp cận hiện tại

- **Injection nhiều repositories**: Controller cần inject 10+ repositories khác nhau (Product, Order, Shopping Cart, v.v.)
- **Trùng lặp Save method**: Mỗi repository đều có phương thức `Save()` giống nhau
- **Save không liên quan đến model**: Phương thức `Save()` là chức năng global, không thuộc về repository cụ thể


#### Ưu và nhược điểm

- **Ưu điểm**: Chỉ inject repository cần thiết, linh hoạt
- **Nhược điểm**: Code trùng lặp, không tối ưu về mặt thiết kế


### Giải pháp Unit of Work Pattern

#### Khái niệm

**Unit of Work** (đơn vị công việc) là pattern tập trung quản lý tất cả repositories và phương thức `Save()` global.

#### Tạo IUnitOfWork Interface

```csharp
public interface IUnitOfWork
{
    ICategoryRepository Category { get; }
    void Save();
}
```

- Chứa tất cả repositories dưới dạng properties
- Có phương thức `Save()` global duy nhất
- Chỉ có getter cho các repository properties


#### Triển khai UnitOfWork Class

```csharp
public class UnitOfWork : IUnitOfWork
{
    private readonly ApplicationDbContext _db;
    
    public ICategoryRepository Category { get; private set; }

    public UnitOfWork(ApplicationDbContext db)
    {
        _db = db;
        Category = new CategoryRepository(db);
    }

    public void Save()
    {
        _db.SaveChanges();
    }
}
```


### Chi tiết triển khai

#### Constructor và Dependency Injection

- UnitOfWork nhận `ApplicationDbContext` qua DI
- Khởi tạo tất cả repositories trong constructor
- Truyền DbContext cho từng repository


#### Repository Properties

- **Getter**: Public, cho phép truy cập từ bên ngoài
- **Setter**: Private, chỉ được set trong constructor
- Tránh việc tạo instance repository nhiều lần


#### Centralized Save Method

- Phương thức `Save()` duy nhất trong UnitOfWork
- Gọi `_db.SaveChanges()` để lưu tất cả thay đổi
- Loại bỏ `Save()` khỏi individual repositories


### Cập nhật ICategoryRepository

#### Loại bỏ Save method

```csharp
public interface ICategoryRepository : IRepository<Category>
{
    void Update(Category obj);
    // Xóa: void Save(); - không cần nữa
}
```


#### Lý do loại bỏ

- `Save()` là global functionality
- Không thuộc về logic riêng của Category
- Tránh trùng lặp code


### Lợi ích của Unit of Work

#### Tập trung quản lý

- Tất cả repositories ở một nơi
- Một phương thức `Save()` duy nhất
- Dễ quản lý transaction


#### Giảm Dependency Injection

- Controller chỉ cần inject `IUnitOfWork`
- Thay vì inject nhiều repositories riêng lẻ
- Code cleaner và dễ maintain


#### Consistency

- Đảm bảo tất cả repositories sử dụng cùng DbContext
- Transaction nhất quán giữa các operations


### Ghi chú quan trọng

#### Khi nào nên sử dụng

- **Không bắt buộc**: Nếu chỉ có Category repository thì không cần Unit of Work
- **Nên dùng**: Khi có nhiều repositories và cần quản lý tập trung
- **Cá nhân preference**: Instructor thích approach này vì cleaner


#### Trade-offs

- **Pros**: Clean code, centralized management
- **Cons**: Có một số drawbacks sẽ được đề cập sau


#### Visual Studio Note

- **Compact view**: Tính năng mới trong Visual Studio
- Hữu ích khi tạo class files
- Có thể "Show all templates" để xem đầy đủ options


### Bước tiếp theo

UnitOfWork đã được implement xong. Bước tiếp theo sẽ là cập nhật CategoryController để sử dụng `IUnitOfWork` thay vì `ICategoryRepository` trực tiếp.

**Liên kết:** [[Repository Pattern]], [[ICategoryRepository]], [[CategoryRepository]], [[Dependency Injection]], [[ApplicationDbContext]], [[IUnitOfWork]], [[SaveChanges]], [[Transaction Management]], [[Visual Studio]]

