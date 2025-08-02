## Các Loại Service Lifetime trong Dependency Injection của .NET Core

### Tổng quan về Service Lifetimes

Có **3 loại service lifetime** chính trong Dependency Injection:

- **Transient**: Tạo object mới **mỗi lần được yêu cầu**
- **Scoped**: Tạo object mới **một lần cho mỗi HTTP request**
- **Singleton**: Tạo object mới **một lần cho toàn bộ application lifetime**


### Chi tiết từng Service Lifetime

#### Transient Lifetime

- **Đơn giản và an toàn nhất** trong các service lifetime
- **Mỗi lần service được request** → Tạo implementation mới
- **Không bao giờ tái sử dụng** object đã tồn tại
- **Ví dụ**: Nếu gọi service 10 lần trong 1 request → Tạo 10 object khác nhau


#### Scoped Lifetime

- **Phụ thuộc vào HTTP request**
- **Một object cho một HTTP request** → Tái sử dụng trong cùng request
- **Ví dụ**: Gọi service 10 lần trong 1 page load → Chỉ tạo 1 object, sử dụng 10 lần
- **Request mới** → Tạo implementation mới
- **Được khuyến nghị nhất** cho web applications


#### Singleton Lifetime

- **Một implementation cho toàn bộ application lifetime**
- **Tạo một lần** khi application start → Sử dụng cho tất cả requests
- **Chỉ tạo lại** khi restart application


### Ví dụ thực tế với Code

#### Tạo Interfaces

```csharp
// ITransientGuidService.cs
public interface ITransientGuidService
{
    string GetGuid();
}

// IScopedGuidService.cs  
public interface IScopedGuidService
{
    string GetGuid();
}

// ISingletonGuidService.cs
public interface ISingletonGuidService
{
    string GetGuid();
}
```


#### Tạo Implementations

```csharp
public class TransientGuidService : ITransientGuidService
{
    private readonly Guid _id;
    
    public TransientGuidService()
    {
        _id = Guid.NewGuid();
    }
    
    public string GetGuid()
    {
        return _id.ToString();
    }
}

// Tương tự cho ScopedGuidService và SingletonGuidService
```


#### Đăng ký Services trong Program.cs

```csharp
builder.Services.AddSingleton<ISingletonGuidService, SingletonGuidService>();
builder.Services.AddScoped<IScopedGuidService, ScopedGuidService>();
builder.Services.AddTransient<ITransientGuidService, TransientGuidService>();
```


#### Sử dụng trong HomeController

```csharp
public class HomeController : Controller
{
    private readonly ITransientGuidService _transient1, _transient2;
    private readonly IScopedGuidService _scoped1, _scoped2;
    private readonly ISingletonGuidService _singleton1, _singleton2;
    
    public HomeController(
        ITransientGuidService transient1,
        ITransientGuidService transient2,
        IScopedGuidService scoped1,
        IScopedGuidService scoped2,
        ISingletonGuidService singleton1,
        ISingletonGuidService singleton2)
    {
        _transient1 = transient1;
        _transient2 = transient2;
        _scoped1 = scoped1;
        _scoped2 = scoped2;
        _singleton1 = singleton1;
        _singleton2 = singleton2;
    }
    
    public IActionResult Index()
    {
        var message = new StringBuilder();
        message.AppendLine($"Transient 1: {_transient1.GetGuid()}");
        message.AppendLine($"Transient 2: {_transient2.GetGuid()}");
        message.AppendLine($"Scoped 1: {_scoped1.GetGuid()}");
        message.AppendLine($"Scoped 2: {_scoped2.GetGuid()}");
        message.AppendLine($"Singleton 1: {_singleton1.GetGuid()}");
        message.AppendLine($"Singleton 2: {_singleton2.GetGuid()}");
        
        return Ok(message.ToString());
    }
}
```


### Kết quả khi chạy ứng dụng

#### Lần đầu chạy:

- **Transient 1** và **Transient 2**: Có GUID **khác nhau**
- **Scoped 1** và **Scoped 2**: Có GUID **giống nhau** (cùng request)
- **Singleton 1** và **Singleton 2**: Có GUID **giống nhau** (cùng application)


#### Khi refresh browser:

- **Transient**: GUID **thay đổi** cho cả 2 instance
- **Scoped**: GUID **thay đổi** cho cả 2 instance (request mới)
- **Singleton**: GUID **không thay đổi** (vẫn giữ nguyên từ lần đầu)


### So sánh Service Lifetimes

| Service Lifetime | Tần suất tạo object | Phạm vi sử dụng | Khuyến nghị |
| :-- | :-- | :-- | :-- |
| **Transient** | Mỗi lần request | Mỗi lần gọi service | An toàn, đơn giản |
| **Scoped** | Mỗi HTTP request | Trong cùng request | **Khuyến nghị** cho web apps |
| **Singleton** | Một lần duy nhất | Toàn bộ application | Cần cẩn thận với thread-safety |

### Ghi chú thêm

- **Dependency Injection Container** tự động quản lý lifecycle của các services
- **Scoped lifetime** phù hợp nhất cho web applications vì cân bằng performance và resource usage
- **Singleton** cần chú ý về **thread-safety** khi có nhiều request đồng thời
- **Transient** an toàn nhất nhưng có thể tốn resources nếu object nặng

**Liên kết:** [[Dependency Injection]], [[.NET Core]], [[Service Lifetime]], [[Program.cs]], [[Constructor Injection]], [[HTTP Request]], [[Application Lifetime]], [[Transient]], [[Scoped]], [[Singleton]], [[GUID]]

