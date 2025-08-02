## Routing trong MVC Application

### Khái niệm Routing

- **[[Routing]]**: định nghĩa khi người dùng nhập URL, request sẽ được gửi đến đâu
- Đã cấu hình **default route** trong [[Program.cs]], nhưng MVC có pattern cụ thể


### Cấu trúc URL Pattern trong MVC

#### Phần bỏ qua khi xử lý routing:

- **Domain name**: `localhost`, `google.com`, `.netmastery.com`
- **Port number**: `:3000`, `:5000`


#### Pattern chính sau domain:

```
/{controller}/{action}/{id?}
```

**Hai từ khóa quan trọng cần nhớ**:

1. **Controller**: điều khiển logic
2. **Action**: phương thức xử lý trong controller

### Ví dụ phân tích URL

#### URL: `localhost:5000/category/index/3`

**Phân tích**:

- **Domain**: `localhost:5000` (bỏ qua)
- **Controller**: `category` (phần đầu tiên sau domain)
- **Action**: `index` (phần thứ hai)
- **ID**: `3` (tham số tùy chọn)


### Bài tập phân tích URL Patterns

| URL | Controller | Action | ID |
| :-- | :-- | :-- | :-- |
| `/category/index` | `category` | `index` | `null` |
| `/category` | `category` | `index` (mặc định) | `null` |
| `/category/edit/3` | `category` | `edit` | `3` |
| `/product/details/3` | `product` | `details` | `3` |

### Quy tắc mặc định

#### Khi không có Action:

- Nếu chỉ có `/category` → Action tự động là `index`
- **Nguyên tắc**: Action mặc định luôn là `index`


#### Khi không có gì sau domain:

- URL: `localhost:5000/`
- **Controller mặc định**: `Home`
- **Action mặc định**: `Index`
- **Cấu hình trong Program.cs**:

```csharp
pattern: "{controller=Home}/{action=Index}/{id?}"
```


### Tầm quan trọng của việc hiểu Routing

- **Kỹ năng quan trọng**: nhìn vào URL MVC, có thể xác định:
    - Controller name
    - Action name
    - Tham số ID (nếu có)
- **Ứng dụng thực tế**: debug, phát triển và bảo trì ứng dụng MVC


### Default Route Configuration

```csharp
// Trong Program.cs
app.MapControllerRoute(
    name: "default", 
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

**Ý nghĩa**:

- Nếu không định nghĩa gì sau domain → đi đến `HomeController.Index()`
- Có thể thay đổi default route này nếu cần
- **Dấu `?`**: tham số `id` là tùy chọn (có thể null)


### Ghi chú quan trọng

- **Pattern cố định**: `/{controller}/{action}/{id?}`
- **Thứ tự quan trọng**: Controller → Action → ID
- **Action mặc định**: luôn là `Index` khi không chỉ định
- **ID**: luôn là tham số tùy chọn
- Có thể customize routing pattern trong các bài học sau

---
**Liên kết**: [[Program.cs]], [[MVC Architecture]], [[Controllers]], [[Action Methods]], [[URL Pattern]]

