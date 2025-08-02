## Repository Pattern trong .NET Core

### Khái niệm

**Repository Pattern** (mẫu thiết kế kho lưu trữ) là một mẫu thiết kế giúp tách biệt logic truy cập dữ liệu khỏi business logic. Pattern này đặc biệt hữu ích khi làm việc với nhiều model khác nhau như Category, Product, Order Header, Order Detail.

### Cách thực hiện

#### 1. Tạo cấu trúc thư mục

- Trong **Data project**, tạo folder `Repository`
- Folder này sẽ chứa tất cả các repository interface và implementation


#### 2. Tạo Generic Repository Interface

Tạo file `IRepository.cs` với interface generic:

```csharp
public interface IRepository<T> where T : class
{
    // T có thể là Category hoặc bất kỳ generic model nào khác
    // để thực hiện các thao tác CRUD hoặc tương tác với DB context
    
    // Lấy tất cả bản ghi
    IEnumerable<T> GetAll();
    
    // Lấy một bản ghi dựa trên điều kiện LINQ
    T GetFirstOrDefault(Expression<Func<T, bool>> filter);
    
    // Thêm một entity
    void Add(T entity);
    
    // Xóa một entity
    void Remove(T entity);
    
    // Xóa nhiều entities cùng lúc
    void RemoveRange(IEnumerable<T> entities);
}
```


### Giải thích chi tiết các phương thức

#### GetAll()

- **Mục đích**: Lấy tất cả bản ghi để hiển thị danh sách
- **Kiểu trả về**: `IEnumerable<T>`


#### GetFirstOrDefault()

- **Mục đích**: Lấy một bản ghi cụ thể (ví dụ: cho trang Edit)
- **Tham số**: `Expression<Func<T, bool>> filter` - điều kiện LINQ
- **Lý do sử dụng Expression**: Cho phép truyền điều kiện linh hoạt thay vì chỉ giới hạn ở ID như phương thức `Find()`


#### Add(), Remove(), RemoveRange()

- **Add**: Thêm một entity mới
- **Remove**: Xóa một entity
- **RemoveRange**: Xóa nhiều entities trong một lần gọi


### Ghi chú quan trọng

#### Tại sao không có Update và SaveChanges?

- **Update**: Logic cập nhật thường khác nhau giữa các model (Category vs Product)
- Đôi khi chỉ cần cập nhật một số thuộc tính nhất định
- **SaveChanges**: Sẽ được implement ở tầng repository cụ thể (như CategoryRepository)


#### Generic Repository Pattern

- Sử dụng `<T> where T : class` để đảm bảo T là một class
- Cho phép tái sử dụng code cho nhiều model khác nhau
- Giảm thiểu việc lặp lại code


### Bước tiếp theo

Interface đã được định nghĩa xong, bước tiếp theo sẽ là implement interface này trong một class cụ thể.

**Liên kết:** [[Repository Pattern]], [[Generic Interface]], [[Entity Framework Core]], [[CRUD Operations]], [[LINQ Expression]], [[Dependency Injection]]

