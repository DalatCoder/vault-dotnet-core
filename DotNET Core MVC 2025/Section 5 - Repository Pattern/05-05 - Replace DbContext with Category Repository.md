## Sử dụng Repository trong Controller

### Thay thế ApplicationDbContext bằng Repository

#### Cập nhật Constructor

```csharp
public class CategoryController : Controller
{
    private readonly ICategoryRepository _categoryRepo;

    public CategoryController(ICategoryRepository categoryRepo)
    {
        _categoryRepo = categoryRepo;
    }
}
```

- Thay vì inject `ApplicationDbContext`, giờ inject `ICategoryRepository`
- Đổi tên biến từ `_db` thành `_categoryRepo` để rõ nghĩa hơn


#### Cập nhật các phương thức

**Index() - Lấy tất cả Category:**

```csharp
public IActionResult Index()
{
    var categoryList = _categoryRepo.GetAll();
    return View(categoryList);
}
```

**Create() - Thêm Category mới:**

```csharp
[HttpPost]
public IActionResult Create(Category obj)
{
    // Validation logic...
    _categoryRepo.Add(obj);
    _categoryRepo.Save();
    return RedirectToAction("Index");
}
```

**Edit() - Lấy Category để chỉnh sửa:**

```csharp
public IActionResult Edit(int? id)
{
    var category = _categoryRepo.GetFirstOrDefault(u => u.Id == id);
    return View(category);
}
```

**Update() - Cập nhật Category:**

```csharp
[HttpPost]
public IActionResult Edit(Category obj)
{
    // Validation logic...
    _categoryRepo.Update(obj);
    _categoryRepo.Save();
    return RedirectToAction("Index");
}
```

**Delete() - Xóa Category:**

```csharp
[HttpPost]
public IActionResult DeletePOST(int? id)
{
    var category = _categoryRepo.GetFirstOrDefault(u => u.Id == id);
    _categoryRepo.Remove(category);
    _categoryRepo.Save();
    return RedirectToAction("Index");
}
```


### Đăng ký Dependency Injection

#### Lỗi thường gặp

```
Unable to resolve service for type 'ICategoryRepository' 
while attempting to activate 'CategoryController'
```

- Lỗi này xuất hiện khi chưa đăng ký service trong DI container
- Controller yêu cầu `ICategoryRepository` nhưng hệ thống không biết implementation nào cần inject


#### Giải pháp - Đăng ký Service

```csharp
// Program.cs hoặc Startup.cs
builder.Services.AddScoped<ICategoryRepository, CategoryRepository>();
```


#### Giải thích Lifetime

- **Scoped**: Phổ biến nhất cho Repository pattern
- Một instance cho mỗi HTTP request
- Đảm bảo dữ liệu nhất quán trong suốt request


### Lợi ích của Repository Pattern

#### Tách biệt trách nhiệm

- Controller chỉ tập trung vào logic điều khiển
- Repository xử lý truy cập dữ liệu
- Dễ bảo trì và test


#### Abstraction Layer

- Controller không cần biết chi tiết về Entity Framework
- Có thể thay đổi cách thức lưu trữ dữ liệu mà không ảnh hưởng Controller


#### Testability

- Dễ dàng mock `ICategoryRepository` khi unit test
- Không phụ thuộc vào database thực


### Kiểm tra hoạt động

Sau khi cập nhật:

- Tất cả chức năng CRUD hoạt động bình thường
- Hiển thị danh sách Category
- Tạo, sửa, xóa Category thành công
- Ứng dụng hoạt động giống như trước khi sử dụng Repository


### Ghi chú quan trọng

#### Quy trình Dependency Injection

1. Tạo interface và implementation
2. Đăng ký service trong DI container
3. Inject vào constructor của class cần sử dụng

#### Lỗi "Unable to resolve service"

- Luôn kiểm tra việc đăng ký service đầu tiên
- Đảm bảo using statement đúng namespace
- Xác nhận lifetime phù hợp

**Liên kết:** [[ICategoryRepository]], [[CategoryRepository]], [[Dependency Injection]], [[Repository Pattern]], [[ApplicationDbContext]], [[Scoped Lifetime]], [[Controller]], [[Service Registration]]

