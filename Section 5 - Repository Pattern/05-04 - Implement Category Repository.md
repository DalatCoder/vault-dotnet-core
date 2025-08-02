## Triển khai CategoryRepository Class

### Tạo CategoryRepository Implementation

Tạo class `CategoryRepository.cs` để implement interface `ICategoryRepository`:

```csharp
public class CategoryRepository : Repository<Category>, ICategoryRepository
{
    private readonly ApplicationDbContext _db;

    public CategoryRepository(ApplicationDbContext db) : base(db)
    {
        _db = db;
    }

    public void Save()
    {
        _db.SaveChanges();
    }

    public void Update(Category obj)
    {
        _db.Categories.Update(obj);
    }
}
```


### Khái niệm kế thừa kép

#### Kế thừa từ Generic Repository

- `CategoryRepository` kế thừa từ `Repository<Category>`
- Điều này cung cấp tất cả chức năng cơ bản: `Add()`, `GetAll()`, `GetFirstOrDefault()`, `Remove()`, `RemoveRange()`
- Không cần viết lại các phương thức đã có


#### Implement Interface

- Đồng thời implement `ICategoryRepository` interface
- Chỉ cần viết thêm 2 phương thức: `Update()` và `Save()`


### Constructor và Dependency Injection

#### Vấn đề Constructor

```csharp
// Base class Repository<T> yêu cầu ApplicationDbContext
public Repository(ApplicationDbContext db)
{
    _db = db;
    _dbSet = _db.Set<T>();
}
```


#### Giải pháp

```csharp
public CategoryRepository(ApplicationDbContext db) : base(db)
{
    _db = db;
}
```

- Constructor nhận `ApplicationDbContext` thông qua dependency injection
- Sử dụng `: base(db)` để truyền DbContext cho class cha
- Lưu DbContext để sử dụng trong các phương thức riêng


### Triển khai phương thức cụ thể

#### Save()

```csharp
public void Save()
{
    _db.SaveChanges();
}
```

- Gọi `SaveChanges()` để lưu tất cả thay đổi vào database
- Áp dụng cho tất cả thao tác Add, Update, Remove đã thực hiện


#### Update()

```csharp
public void Update(Category obj)
{
    _db.Categories.Update(obj);
}
```

- Sử dụng `_db.Categories.Update()` của Entity Framework Core
- Đánh dấu entity là Modified state
- Thay đổi sẽ được lưu khi gọi `Save()`


### Ưu điểm của cách tiếp cận

#### Tái sử dụng code

- Kế thừa toàn bộ chức năng cơ bản từ Generic Repository
- Chỉ cần implement logic riêng cho Category


#### Tách biệt trách nhiệm

- Generic Repository: Thao tác CRUD cơ bản
- CategoryRepository: Logic cụ thể cho Category + Save/Update


#### Dependency Injection Ready

- Constructor pattern phù hợp với DI container
- Dễ dàng test và mock


### Bước tiếp theo

CategoryRepository đã được triển khai hoàn chỉnh. Bước tiếp theo sẽ là thay thế việc sử dụng `ApplicationDbContext` trực tiếp trong Controller bằng CategoryRepository này.

**Liên kết:** [[ICategoryRepository]], [[Repository Pattern]], [[Generic Repository]], [[Dependency Injection]], [[Entity Framework Core]], [[Constructor Inheritance]], [[SaveChanges]], [[Update Method]]

