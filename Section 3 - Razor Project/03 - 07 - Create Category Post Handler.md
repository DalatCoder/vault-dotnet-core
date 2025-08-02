## Tạo POST Handler trong Razor Pages

### Thêm chức năng POST để xử lý form submit

#### Cấu trúc POST Handler:

```csharp
public IActionResult OnPost()
{
    _db.Categories.Add(Category);
    _db.SaveChanges();
    return RedirectToPage("Index");
}
```

**Đặc điểm quan trọng:**

- Return type: `IActionResult` (không phải `void` như OnGet)
- Tên method: `OnPost()` - bắt buộc có prefix "On" + "Post"
- Cho phép redirect sau khi xử lý thành công


#### Redirect trong Razor Pages:

- **MVC**: `RedirectToAction()`
- **Razor Pages**: `RedirectToPage()`
- Ví dụ: `RedirectToPage("Index")` - redirect đến trang Index trong cùng thư mục


### Vấn đề Property Binding

#### Hiện tượng ban đầu:

- Khi submit form, object Category nhận được là `null`
- Properties như `Category.Name` không được populate
- Cần debug để phát hiện vấn đề


#### Nguyên nhân:

- Trong Razor Pages, properties không tự động bind như MVC
- Cần khai báo explicit binding để nhận dữ liệu từ form


### Giải pháp 1: BindProperty Attribute

#### Thêm attribute cho property:

```csharp
[BindProperty]
public Category Category { get; set; }
```


#### Ưu điểm:

- Property tự động bind khi POST
- Không cần parameter trong OnPost method
- Truy cập trực tiếp: `Category.Name`, `Category.DisplayOrder`


#### Cách hoạt động:

- `[BindProperty]` đảm bảo property được bind từ form data
- Khi POST, Category object được populate đầy đủ
- Có thể truy cập tất cả properties của Category


### Giải pháp 2: BindProperties (Page Model Level)

#### Áp dụng cho toàn bộ Page Model:

```csharp
[BindProperties]
public class CreateModel : PageModel
{
    public Category Category { get; set; }
    // Các properties khác cũng sẽ được bind tự động
}
```


#### Ưu điểm:

- Bind tất cả properties trong Page Model
- Không cần khai báo từng property riêng lẻ
- Phù hợp khi có nhiều properties cần bind


### So sánh hai phương pháp

| Phương pháp | Ưu điểm | Nhược điểm |
| :-- | :-- | :-- |
| `[BindProperty]` | Kiểm soát chính xác property nào được bind | Phải khai báo từng property |
| `[BindProperties]` | Tự động bind tất cả properties | Ít kiểm soát, có thể bind không mong muốn |

### Kiểm tra và debug

#### Sử dụng breakpoint:

- Đặt breakpoint trong `OnPost()` method
- Kiểm tra Category object có được populate không
- Verify các properties: `Category.Name`, `Category.DisplayOrder`


#### Test workflow:

- Truy cập `/Categories/Create`
- Nhập dữ liệu vào form
- Submit form
- Kiểm tra dữ liệu trong database
- Redirect về Index page


### Quy trình hoàn chỉnh

#### OnGet Handler:

- Load trang Create với form trống
- Không cần logic đặc biệt


#### OnPost Handler:

```csharp
public IActionResult OnPost()
{
    _db.Categories.Add(Category);
    _db.SaveChanges();
    return RedirectToPage("Index");
}
```


#### Model Binding:

- Sử dụng `[BindProperty]` hoặc `[BindProperties]`
- Đảm bảo dữ liệu form được bind vào Page Model


### Bài tập thực hành

#### Nhiệm vụ được giao:

- Tạo chức năng **Edit** functionality
- Tạo chức năng **Delete** functionality
- Áp dụng kiến thức về GET và POST handlers
- Sử dụng property binding phù hợp


#### Kiến thức cần áp dụng:

- OnGet handler để load dữ liệu hiện có
- OnPost handler để update/delete
- Property binding với `[BindProperty]`
- Redirect sau khi hoàn thành thao tác


### Ghi chú thêm

- **Binding là bắt buộc**: Không như MVC, Razor Pages cần explicit binding
- **Return type quan trọng**: OnPost phải return `IActionResult` để redirect
- **Naming convention**: OnGet, OnPost - prefix "On" là bắt buộc
- **RedirectToPage**: Phương thức redirect chuyên dụng cho Razor Pages
- **Debug-friendly**: Có thể dễ dàng debug và kiểm tra object binding

**Liên kết:** [[Razor Pages]], [[POST Handler]], [[BindProperty]], [[BindProperties]], [[Page Model]], [[OnPost]], [[RedirectToPage]], [[Property Binding]], [[IActionResult]], [[Entity Framework Core]], [[Categories]], [[CRUD Operations]]

