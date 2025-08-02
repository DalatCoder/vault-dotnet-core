## Thay thế CategoryRepository bằng Unit of Work trong Controller

### Cập nhật Dependency Injection

#### Thay đổi trong Program.cs

```csharp
// Thay vì đăng ký CategoryRepository
// builder.Services.AddScoped<ICategoryRepository, CategoryRepository>();

// Đăng ký UnitOfWork
builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();
```

- Loại bỏ registration của `ICategoryRepository`
- Thêm registration cho `IUnitOfWork`
- UnitOfWork sẽ internally tạo CategoryRepository


### Cập nhật CategoryController

#### Thay đổi Constructor

```csharp
public class CategoryController : Controller
{
    private readonly IUnitOfWork _unitOfWork;

    public CategoryController(IUnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }
}
```


#### Cập nhật các phương thức

**Index() - Lấy tất cả Categories:**

```csharp
public IActionResult Index()
{
    var categoryList = _unitOfWork.Category.GetAll();
    return View(categoryList);
}
```

**Create() - Thêm Category:**

```csharp
[HttpPost]
public IActionResult Create(Category obj)
{
    // Validation logic...
    _unitOfWork.Category.Add(obj);
    _unitOfWork.Save(); // Không cần .Category
    return RedirectToAction("Index");
}
```

**Edit() - Lấy Category để sửa:**

```csharp
public IActionResult Edit(int? id)
{
    var category = _unitOfWork.Category.GetFirstOrDefault(u => u.Id == id);
    return View(category);
}
```

**Update() - Cập nhật Category:**

```csharp
[HttpPost]
public IActionResult Edit(Category obj)
{
    // Validation logic...
    _unitOfWork.Category.Update(obj);
    _unitOfWork.Save(); // Không cần .Category
    return RedirectToAction("Index");
}
```

**Delete() - Xóa Category:**

```csharp
[HttpPost]
public IActionResult DeletePOST(int? id)
{
    var category = _unitOfWork.Category.GetFirstOrDefault(u => u.Id == id);
    _unitOfWork.Category.Remove(category);
    _unitOfWork.Save(); // Không cần .Category
    return RedirectToAction("Index");
}
```


### Sự khác biệt khi sử dụng Unit of Work

#### Cách truy cập repository

- **Trước đây**: `_categoryRepo.GetAll()`
- **Bây giờ**: `_unitOfWork.Category.GetAll()`


#### Phương thức Save

- **Repository methods**: `_unitOfWork.Category.Add()`, `_unitOfWork.Category.Update()`
- **Save method**: `_unitOfWork.Save()` (không cần .Category)


### So sánh ưu và nhược điểm

#### Ưu điểm của Unit of Work

**Centralized Management (Quản lý tập trung):**

- Tất cả repositories trong một interface duy nhất
- Code cleaner và organized hơn
- Dễ maintain khi có nhiều repositories

**Single Save Method:**

- Chỉ có một phương thức `Save()` global
- Tránh trùng lặp code
- Đảm bảo consistency cho transactions


#### Nhược điểm của Unit of Work

**Over-instantiation (Tạo instance thừa):**

- UnitOfWork tạo **tất cả repositories** khi khởi tạo
- CategoryController chỉ cần Category nhưng vẫn tạo Product, Order, ShoppingCart repositories
- Có thể impact performance nếu có nhiều repositories

**Less Granular Control:**

- Không thể chọn inject chỉ repository cần thiết
- Luôn phải load toàn bộ repositories


#### So sánh với Repository trực tiếp

**Repository Pattern thuần:**

- **Ưu điểm**: Chỉ inject repository cần thiết, flexible
- **Nhược điểm**: Nhiều dependencies, code trùng lặp Save methods

**Unit of Work Pattern:**

- **Ưu điểm**: Clean code, centralized, ít dependencies
- **Nhược điểm**: Over-instantiation, ít flexibility


### Kiểm tra hoạt động

Sau khi cập nhật:

- Tất cả chức năng CRUD hoạt động bình thường
- Tạo, sửa, xóa Category thành công
- Ứng dụng hoạt động như trước nhưng với architecture sạch hơn


### Kết luận cá nhân của Instructor

> **Personally speaking, I like the Unit of Work implementation because it is much cleaner.**

- Mặc dù có trade-offs, Unit of Work được ưa chuộng vì:
    - Code organization tốt hơn
    - Easier maintenance
    - Professional architecture pattern


### Ghi chú quan trọng

#### Khi nào nên dùng Unit of Work

- **Dự án lớn**: Nhiều repositories, cần quản lý tập trung
- **Team development**: Consistency và standardization
- **Enterprise applications**: Professional architecture


#### Khi nào nên dùng Repository trực tiếp

- **Dự án nhỏ**: Ít repositories, cần flexibility
- **Microservices**: Mỗi service chỉ cần vài repositories
- **Performance critical**: Tránh over-instantiation

**Liên kết:** [[IUnitOfWork]], [[UnitOfWork]], [[ICategoryRepository]], [[CategoryRepository]], [[Dependency Injection]], [[Program.cs]], [[CategoryController]], [[Repository Pattern]], [[Save Method]], [[Centralized Management]]

