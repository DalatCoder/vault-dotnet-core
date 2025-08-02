## Triển khai Repository Pattern

### Tạo lớp Repository

Tạo class `Repository.cs` trong thư mục Repository để implement interface `IRepository<T>`:

```csharp
public class Repository<T> : IRepository<T> where T : class
{
    private readonly ApplicationDbContext _db;
    private readonly DbSet<T> _dbSet;

    public Repository(ApplicationDbContext db)
    {
        _db = db;
        _dbSet = _db.Set<T>();
    }

    // Implementation methods...
}
```


### Khái niệm quan trọng

#### DbSet Generic

- Thay vì sử dụng `_db.Categories` cụ thể, ta sử dụng `DbSet<T>` generic
- `_dbSet = _db.Set<T>()` giúp truy cập động đến bảng dữ liệu tương ứng
- Khi Repository được khởi tạo với Category, `_dbSet` sẽ tương đương với `_db.Categories`


#### Dependency Injection

- Constructor nhận `ApplicationDbContext` thông qua dependency injection
- Đảm bảo Repository có thể truy cập cơ sở dữ liệu


### Triển khai các phương thức

#### Add()

```csharp
public void Add(T entity)
{
    _dbSet.Add(entity);
}
```

- Sử dụng `_dbSet.Add()` thay vì `_db.Categories.Add()`
- Generic cho phép làm việc với bất kỳ entity nào


#### GetFirstOrDefault()

```csharp
public T GetFirstOrDefault(Expression<Func<T, bool>> filter)
{
    IQueryable<T> query = _dbSet;
    query = query.Where(filter);
    return query.FirstOrDefault();
}
```

- Tạo `IQueryable<T>` từ `_dbSet`
- Áp dụng điều kiện filter bằng `Where()`
- Trả về bản ghi đầu tiên hoặc null


#### GetAll()

```csharp
public IEnumerable<T> GetAll()
{
    IQueryable<T> query = _dbSet;
    return query.ToList();
}
```

- Lấy toàn bộ dữ liệu từ `_dbSet`
- Chuyển đổi thành `List<T>` để trả về


#### Remove()

```csharp
public void Remove(T entity)
{
    _dbSet.Remove(entity);
}
```

- Sử dụng phương thức `Remove()` có sẵn trong Entity Framework Core


#### RemoveRange()

```csharp
public void RemoveRange(IEnumerable<T> entities)
{
    _dbSet.RemoveRange(entities);
}
```

- Xóa nhiều entities cùng lúc
- Nhận tham số là `IEnumerable<T>`


### Ưu điểm của cách triển khai

#### Tính Generic

- Một class Repository có thể làm việc với nhiều entity khác nhau
- Giảm thiểu việc lặp lại code


#### Tách biệt logic

- Repository chỉ tập trung vào truy cập dữ liệu
- Business logic được xử lý ở tầng khác


#### Dễ bảo trì

- Code rõ ràng, dễ hiểu
- Dễ dàng mở rộng thêm tính năng


### Ghi chú quan trọng

- Repository này chỉ thực hiện các thao tác cơ bản với database
- Không gọi `SaveChanges()` - sẽ được xử lý ở tầng cao hơn
- Sử dụng `IQueryable<T>` để tối ưu hóa truy vấn

**Liên kết:** [[IRepository Interface]], [[DbSet Generic]], [[Entity Framework Core]], [[Dependency Injection]], [[IQueryable]], [[Generic Repository Pattern]]

