## Controllers và Action Methods trong MVC

### Cấu trúc thư mục Controllers

#### Quy tắc đặt tên Controller

- **Bắt buộc**: tên phải kết thúc bằng từ khóa `Controller`
- **Ví dụ**: `HomeController` (không phải chỉ `Home`)
- **Vị trí**: phải đặt trong thư mục `Controllers`
- **Lưu ý**: nếu đặt sai vị trí, sẽ không hoạt động


#### Trong Solution Explorer

```
📁 Controllers/
   └── HomeController.cs
📁 Models/
   └── ErrorViewModel.cs  
📁 Views/
   └── 📁 Home/
       ├── Index.cshtml
       └── Privacy.cshtml
```


### Quy tắc ánh xạ Views

#### Cấu trúc thư mục Views

- **Nguyên tắc**: Views phải đặt trong thư mục con có **tên giống Controller**
- **Tên thư mục**: `Home` (không phải `HomeController`)
- **Ví dụ**:
    - Controller: `HomeController`
    - Thư mục views: `Views/Home/`


#### Quan hệ Controller-View

- Mỗi [[Action Method]] trong Controller tương ứng với một View file
- **HomeController** → Views được đặt trong `Views/Home/`
- **Action `Index`** → View file `Index.cshtml`
- **Action `Privacy`** → View file `Privacy.cshtml`


### Action Methods trong Controller

#### Cấu trúc Action Method

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        return View(); // Trả về view có tên giống action
    }
    
    public IActionResult Privacy() 
    {
        return View(); // Trả về Privacy.cshtml
    }
}
```


#### Cách hoạt động của `return View()`

- **Không tham số**: tìm view có tên giống với action method
- **Tự động ánh xạ**:
    - Action `Index()` → tìm file `Index.cshtml`
    - Action `Privacy()` → tìm file `Privacy.cshtml`
- **Vị trí tìm**: trong thư mục `Views/{ControllerName}/`


### Luồng xử lý Request-Response

#### Ví dụ URL: `/Home/Privacy`

1. **Routing** xác định: Controller = `Home`, Action = `Privacy`
2. **Controller** thực thi: `HomeController.Privacy()`
3. **Action Method** chạy: `return View()`
4. **System** tìm view: `Views/Home/Privacy.cshtml`
5. **Response** trả về: nội dung HTML của Privacy view

#### Default Route behavior

- **URL trống**: `localhost:5000/`
- **Ánh xạ**: `HomeController.Index()` (từ [[Program.cs]])
- **Kết quả**: hiển thị `Views/Home/Index.cshtml`


### Debugging Controllers

#### Sử dụng Breakpoints

```csharp
public IActionResult Index()
{
    return View(); // ← Đặt breakpoint ở đây
}

public IActionResult Privacy()  
{
    return View(); // ← Đặt breakpoint ở đây
}
```

**Cách sử dụng**:

- Click vào **circle dot** bên trái dòng code
- Khi chạy ứng dụng, debugger sẽ dừng tại breakpoint
- **Chứng minh**: request đã đến đúng action method


### Tùy chỉnh View trả về

#### Override default view

```csharp
public IActionResult Index()
{
    return View("Privacy"); // Trả về Privacy.cshtml thay vì Index.cshtml
}
```


#### Thay đổi Default Route

```csharp
// Trong Program.cs
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Privacy}/{id?}"); // Đổi từ Index thành Privacy
```


### Quan hệ với Models

#### Model không luôn cần thiết

- **Trường hợp cần Model**: hiển thị dữ liệu dynamic từ database
- **Trường hợp không cần**: hiển thị static view (như trang About, Contact)
- **Ví dụ**: `ErrorViewModel` - chỉ chứa 2 properties đơn giản


### URL Mapping Examples

| URL | Controller | Action | View File |
| :-- | :-- | :-- | :-- |
| `/` | `Home` | `Index` | `Views/Home/Index.cshtml` |
| `/Home/Index` | `Home` | `Index` | `Views/Home/Index.cshtml` |
| `/Home/Privacy` | `Home` | `Privacy` | `Views/Home/Privacy.cshtml` |

### Ghi chú quan trọng

- **MVC architecture** phức tạp cho người mới bắt đầu
- **Mục tiêu hiện tại**: hiểu 50-70% là tiến bộ tốt
- **Thực hành**: khi implement sẽ hiểu rõ hơn tại sao pattern này mạnh mẽ
- **Naming convention** rất quan trọng - phải tuân thủ chính xác
- **Debugging** giúp hiểu flow của request/response

---
**Liên kết**: [[Program.cs]], [[MVC Architecture]], [[Routing]], [[Action Methods]], [[Views]], [[Models]]

