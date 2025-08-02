## Sử dụng ViewData và So sánh ViewBag, ViewData, TempData

### Khái niệm ViewData

**ViewData** là phương pháp thứ hai để truyền dữ liệu từ Controller sang View, có các đặc điểm:

- **Hướng truyền**: Chỉ từ Controller → View (giống ViewBag)
- **Mục đích**: Lý tưởng cho dữ liệu tạm thời không thuộc về Model
- **Kiểu dữ liệu**: Derived từ ViewDataDictionary (dictionary type)
- **Yêu cầu**: Phải type cast trước khi sử dụng
- **Thời gian sống**: Chỉ tồn tại trong HTTP request hiện tại


### Sự khác biệt chính so với ViewBag

| Đặc điểm | ViewBag | ViewData |
| :-- | :-- | :-- |
| **Syntax** | Dynamic properties | Dictionary với [] |
| **Type casting** | Không cần | Bắt buộc phải cast |
| **IntelliSense** | Không có | Không có |
| **Convenience** | Dễ sử dụng hơn | Phức tạp hơn một chút |

### Cách thực hiện với ViewData

**Bước 1: Sử dụng ViewData trong Controller**

```csharp
public IActionResult Create()
{
    IEnumerable<SelectListItem> categoryList = _unitOfWork.Category.GetAll()
        .Select(u => new SelectListItem
        {
            Text = u.Name,
            Value = u.Id.ToString()
        });
    
    // Sử dụng ViewData với dictionary syntax
    ViewData["CategoryList"] = categoryList;
    
    return View();
}
```

**Đặc điểm syntax:**

- Không sử dụng dot notation như ViewBag
- Phải dùng square brackets `[]` như dictionary
- Key được định nghĩa trong dấu ngoặc vuông

**Bước 2: Truy cập ViewData trong View**

```html
<div class="mb-3">
    <label asp-for="CategoryId" class="form-label"></label>
    <select asp-for="CategoryId" asp-items="@(ViewData["CategoryList"] as IEnumerable<SelectListItem>)" class="form-select">
        <option disabled selected>Select Category</option>
    </select>
    <span asp-validation-for="CategoryId" class="text-danger"></span>
</div>
```

**Chi tiết type casting:**

- `ViewData["CategoryList"]`: Lấy dữ liệu từ ViewData
- `as IEnumerable<SelectListItem>`: Explicit casting về đúng kiểu dữ liệu
- Bắt buộc phải cast vì ViewData lưu trữ dưới dạng object


### Lưu ý quan trọng về xung đột

**Vấn đề trùng key:**

- ViewBag internally sử dụng ViewDataDictionary
- Key của ViewData và property của ViewBag **không được trùng nhau**
- Cả hai sẽ lưu vào cùng một entity, gây xung đột dữ liệu

```csharp
// TRÁNH làm như này
ViewBag.CategoryList = categoryList;
ViewData["CategoryList"] = categoryList;  // Sẽ gây xung đột
```


### Tổng quan về TempData

**TempData** là phương pháp thứ ba để truyền dữ liệu:

- **Mục đích**: Lưu trữ dữ liệu giữa **hai consecutive requests**
- **Cơ chế**: Sử dụng Session để lưu trữ internally
- **Bản chất**: Short-lived session, chỉ tồn tại cho một request
- **Type casting**: Bắt buộc phải cast trước khi sử dụng
- **Null check**: Cần kiểm tra null để tránh runtime errors
- **Ứng dụng**: Thông báo lỗi, validation messages, one-time messages
- **Hành vi**: Tự động biến mất sau khi refresh page


### So sánh tổng thể ViewBag, ViewData, TempData

| Tiêu chí | ViewBag | ViewData | TempData |
| :-- | :-- | :-- | :-- |
| **Syntax** | Dynamic properties | Dictionary `[]` | Dictionary `[]` |
| **Type casting** | Không cần | Bắt buộc | Bắt buộc |
| **Thời gian sống** | Current request | Current request | Consecutive requests |
| **Redirection** | Null after redirect | Null after redirect | Persist qua redirect |
| **Storage mechanism** | ViewDataDictionary | ViewDataDictionary | Session |
| **Use case** | Simple data transfer | Simple data transfer | Messages, alerts |
| **Performance** | Lightweight | Lightweight | Heavier (Session) |

### Kết quả thực tế

Sau khi thay đổi từ ViewBag sang ViewData:

- Chức năng dropdown Category hoạt động bình thường
- Code phức tạp hơn một chút do yêu cầu type casting
- Hiệu suất tương đương ViewBag
- Cần cẩn thận với spelling của key name


### Ghi chú thêm

- **ViewData** phù hợp khi cần kiểm soát chặt chẽ kiểu dữ liệu
- **ViewBag** thuận tiện hơn cho rapid development
- **TempData** là lựa chọn duy nhất cho data persist qua redirect
- Nên chọn một phương pháp và sử dụng consistent trong toàn dự án
- Type casting có thể gây runtime error nếu không cẩn thận
- Always check for null values khi sử dụng ViewData và TempData

**Liên kết:** [[ViewBag]], [[ViewData]], [[TempData]], [[ViewDataDictionary]], [[Product Controller]], [[Category Model]], [[SelectListItem]], [[Type Casting]], [[Dictionary]], [[Session]], [[HTTP Request]], [[ASP.NET Core MVC]], [[Controller to View]], [[Bootstrap Forms]]

