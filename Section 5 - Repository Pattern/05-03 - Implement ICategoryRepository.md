## Tạo Category Repository Interface

### Khái niệm

Sau khi có **Generic Repository** và **IRepository interface**, bước tiếp theo là tạo interface cụ thể cho từng entity. Với Category, chúng ta cần tạo `ICategoryRepository` để mở rộng chức năng cơ bản.

### Cách thực hiện

#### 1. Tạo ICategoryRepository Interface

```csharp
public interface ICategoryRepository : IRepository<Category>
{
    void Update(Category obj);
    void Save();
}
```


#### 2. Kế thừa từ IRepository

- `ICategoryRepository` implement `IRepository<Category>`
- Lúc này chúng ta đã biết rõ **Class type** là `Category`
- Interface sẽ có tất cả phương thức cơ bản từ `IRepository<T>` với `T = Category`


### Tại sao cần interface riêng?

#### Chức năng cơ bản được kế thừa

- `GetAll()` → lấy tất cả Category
- `GetFirstOrDefault()` → lấy một Category theo điều kiện
- `Add()` → thêm Category mới
- `Remove()` → xóa Category
- `RemoveRange()` → xóa nhiều Category


#### Chức năng bổ sung cần thiết

- **Update()**: Cập nhật thông tin Category
- **Save()**: Lưu thay đổi vào database (gọi SaveChanges())


### Lý do tách Update và Save

#### Update logic khác nhau

- Logic cập nhật Category có thể khác với Product
- Đôi khi chỉ cần update một số thuộc tính nhất định
- Có thể có validation riêng cho từng entity


#### Save trong repository cụ thể

- Generic Repository không gọi SaveChanges()
- Category Repository sẽ quản lý việc lưu thay đổi
- Tách biệt trách nhiệm rõ ràng


### Cấu trúc hoàn chỉnh

```csharp
// Base interface - Generic
public interface IRepository<T> where T : class
{
    IEnumerable<T> GetAll();
    T GetFirstOrDefault(Expression<Func<T, bool>> filter);
    void Add(T entity);
    void Remove(T entity);
    void RemoveRange(IEnumerable<T> entities);
}

// Specific interface - Category
public interface ICategoryRepository : IRepository<Category>
{
    void Update(Category obj);
    void Save();
}
```


### Bước tiếp theo

Interface `ICategoryRepository` đã được định nghĩa xong. Bước tiếp theo sẽ là tạo class `CategoryRepository` để implement interface này.

**Liên kết:** [[IRepository Interface]], [[Repository Pattern]], [[Generic Repository]], [[Category Model]], [[Interface Inheritance]], [[Update Method]], [[SaveChanges]]

