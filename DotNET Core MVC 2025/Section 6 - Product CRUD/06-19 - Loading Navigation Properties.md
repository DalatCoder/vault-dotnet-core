## Sử dụng Include để hiển thị Navigation Properties trong Entity Framework Core

### Vấn đề hiện tại

Trong danh sách Product, hiện tại chỉ hiển thị **CategoryId** (số) thay vì tên Category thực tế. Mặc dù có foreign key relationship giữa Product và Category, nhưng navigation property `Category` chưa được populate khi truy xuất dữ liệu.

### Khái niệm Include trong EF Core

**Include** là tính năng của Entity Framework Core cho phép:

- Tự động populate **navigation properties** dựa trên foreign key relationship
- Load related data trong một query duy nhất thay vì lazy loading
- Tối ưu hiệu suất bằng cách giảm số lượng database queries


### Cách sử dụng Include cơ bản

**Syntax trực tiếp với ApplicationDbContext:**

```csharp
// Trong Controller hoặc Repository
var products = _db.Products
    .Include(u => u.Category)  // Include navigation property
    .ToList();
```

**Đặc điểm quan trọng:**

- `Include()` chỉ định related entities được include trong query
- Navigation property `Category` sẽ được populate tự động
- Có thể chain multiple Include statements cho nhiều relationships


### Implement Include trong Repository Pattern

**Bước 1: Cập nhật IRepository Interface**

```csharp
public interface IRepository<T> where T : class
{
    IEnumerable<T> GetAll(string? includeProperties = null);
    T Get(Expression<Func<T, bool>> filter, string? includeProperties = null);
    void Add(T entity);
    void Remove(T entity);
    void RemoveRange(IEnumerable<T> entity);
}
```

**Bước 2: Implement Include Logic trong Repository**

```csharp
public class Repository<T> : IRepository<T> where T : class
{
    private readonly ApplicationDbContext _db;
    internal DbSet<T> dbSet;
    
    public Repository(ApplicationDbContext db)
    {
        _db = db;
        this.dbSet = _db.Set<T>();
    }
    
    public IEnumerable<T> GetAll(string? includeProperties = null)
    {
        IQueryable<T> query = dbSet;
        
        // Xử lý Include Properties
        if (!string.IsNullOrEmpty(includeProperties))
        {
            foreach (var includeProp in includeProperties
                .Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries))
            {
                query = query.Include(includeProp);
            }
        }
        
        return query.ToList();
    }
    
    public T Get(Expression<Func<T, bool>> filter, string? includeProperties = null)
    {
        IQueryable<T> query = dbSet;
        query = query.Where(filter);
        
        // Áp dụng Include Properties tương tự
        if (!string.IsNullOrEmpty(includeProperties))
        {
            foreach (var includeProp in includeProperties
                .Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries))
            {
                query = query.Include(includeProp);
            }
        }
        
        return query.FirstOrDefault();
    }
}
```


### Xử lý Multiple Include Properties

**Comma-separated string format:**

```csharp
// Truyền nhiều navigation properties cùng lúc
var products = _unitOfWork.Product.GetAll(includeProperties: "Category,Author,Publisher");
```

**Logic xử lý:**

- **Split by comma**: Tách chuỗi thành array các property names
- **StringSplitOptions.RemoveEmptyEntries**: Loại bỏ entries trống
- **Foreach loop**: Áp dụng Include cho từng property
- **Dynamic approach**: Repository có thể handle bất kỳ số lượng includes nào


### Sử dụng trong ProductController

**Cập nhật Index Action:**

```csharp
public IActionResult Index()
{
    // Include Category navigation property
    List<Product> objProductList = _unitOfWork.Product
        .GetAll(includeProperties: "Category").ToList();
    
    return View(objProductList);
}
```

**Lưu ý Case Sensitivity:**

```csharp
// ✅ ĐÚNG - Khớp với tên property trong model
.GetAll(includeProperties: "Category")

// ❌ SAI - Sẽ gây lỗi runtime
.GetAll(includeProperties: "category")  // lowercase
```


### Cập nhật View để hiển thị Category Name

**Trong Index.cshtml:**

```html
<table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th>Title</th>
            <th>ISBN</th>
            <th>List Price</th>
            <th>Author</th>
            <th>Category</th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        @foreach (var obj in Model)
        {
            <tr>
                <td>@obj.Title</td>
                <td>@obj.ISBN</td>
                <td>@obj.ListPrice</td>
                <td>@obj.Author</td>
                <td>@obj.Category.Name</td>  <!-- Hiển thị tên Category -->
                <td>
                    <!-- Action buttons -->
                </td>
            </tr>
        }
    </tbody>
</table>
```


### Debug và Troubleshooting

**Kiểm tra Navigation Property được populate:**

```csharp
// Trong Controller với debugging
public IActionResult Index()
{
    List<Product> objProductList = _unitOfWork.Product
        .GetAll(includeProperties: "Category").ToList();
    
    // Debug: Kiểm tra Category có được populate không
    foreach (var product in objProductList)
    {
        if (product.Category != null)
        {
            Console.WriteLine($"Product: {product.Title}, Category: {product.Category.Name}");
        }
    }
    
    return View(objProductList);
}
```

**Lỗi thường gặp:**

- **"Unable to find navigation property"**: Do sai tên property (case sensitivity)
- **Null reference exception**: Do không include navigation property
- **Multiple queries**: Do thiếu Include, EF Core thực hiện lazy loading


### Ưu điểm của Include Pattern

- **Performance**: Giảm số lượng database queries (eager loading thay vì lazy loading)
- **Flexibility**: Repository có thể handle multiple navigation properties
- **Reusability**: Logic Include có thể tái sử dụng cho các entities khác
- **Type Safety**: Compile-time checking với lambda expressions


### Ghi chú thêm

- **Case Sensitivity**: Tên property trong Include string phải khớp chính xác với model
- **Performance**: Chỉ include những navigation properties thực sự cần thiết
- **Multiple Levels**: Có thể include nested relationships với dot notation (`"Category.Parent"`)
- **Null Checking**: Luôn kiểm tra null trước khi truy cập navigation properties
- **Lazy Loading**: Include là eager loading, khác với lazy loading default của EF Core

**Liên kết:** [[Entity Framework Core]], [[Navigation Properties]], [[Repository Pattern]], [[Foreign Key]], [[Product Model]], [[Category Model]], [[Include]], [[Eager Loading]], [[ProductController]], [[IRepository]], [[ApplicationDbContext]], [[Generic Repository]], [[UnitOfWork]]

