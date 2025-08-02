## Truy xuất và hiển thị dữ liệu từ cơ sở dữ liệu trong .NET Core MVC

### Khái niệm cơ bản

- Để hiển thị dữ liệu trong view (giao diện), trước tiên phải truy xuất dữ liệu đó trong controller (bộ điều khiển)
- Dữ liệu được truyền từ controller sang view sẽ được sử dụng để hiển thị trên giao diện người dùng
- Khi làm việc với truy xuất, thêm hoặc cập nhật dữ liệu, chúng ta sẽ sử dụng [[Application DB Context]]


### Sự khác biệt giữa .NET cũ và .NET Core

**Cách làm truyền thống (.NET Framework):**

- Tạo đối tượng Application DB Context thủ công
- Phải mở và đóng kết nối cơ sở dữ liệu manually

**Cách làm trong .NET Core:**

- Sử dụng [[Dependency Injection]] (tiêm phụ thuộc)
- Application DB Context được đăng ký trong services container
- Framework tự động tạo và cung cấp đối tượng khi cần


### Cách triển khai Dependency Injection

```csharp
// Constructor injection
public CategoryController(ApplicationDbContext db)
{
    _db = db;
}

private readonly ApplicationDbContext _db;
```

**Các bước thực hiện:**

- Sử dụng constructor để nhận implementation của Application DB Context
- Tạo private readonly field để lưu trữ đối tượng này
- Gán đối tượng nhận được từ constructor vào field cục bộ
- Sử dụng field này trong các action method khác


### Truy xuất dữ liệu với Entity Framework Core

```csharp
public IActionResult Index()
{
    List<Category> objCategoryList = _db.Categories.ToList();
    return View();
}
```

**Ưu điểm:**

- Không cần viết câu lệnh SQL thủ công
- Chỉ cần 3 từ: `_db.Categories.ToList()`
- Entity Framework Core tự động chuyển đổi thành câu lệnh `SELECT * FROM Categories`
- Truy xuất dữ liệu và gán vào đối tượng một cách tự động


### Quy trình hoạt động

- Framework truy cập vào database
- Thực thi câu lệnh SQL tương ứng
- Truy xuất tất cả bản ghi từ bảng categories
- Chuyển đổi thành danh sách đối tượng Category
- Gán kết quả vào biến `objCategoryList`


### Bước tiếp theo

- Dữ liệu đã được truy xuất thành công trong controller
- Cần truyền đối tượng này sang [[View]]
- Trong view, cần fetch và hiển thị tất cả các categories
- Sử dụng debugging để kiểm tra dữ liệu đã được truy xuất đúng (ví dụ: action, scifi, history)


### Ghi chú thêm

- Sử dụng `var` hoặc khai báo kiểu rõ ràng `List<Category>` đều được
- Việc khai báo kiểu rõ ràng giúp code dễ đọc và hiểu hơn
- [[Entity Framework Core]] làm đơn giản hóa việc làm việc với cơ sở dữ liệu đáng kể so với các phương pháp truyền thống

