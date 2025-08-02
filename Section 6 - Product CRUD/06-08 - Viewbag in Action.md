## Sử dụng ViewBag để truyền dữ liệu Category Dropdown

### Khái niệm ViewBag

**ViewBag** là một cách thức truyền dữ liệu từ Controller sang View trong ASP.NET Core MVC, với các đặc điểm sau:

- **Hướng truyền**: Chỉ từ Controller → View (không ngược lại)
- **Mục đích**: Lý tưởng cho dữ liệu tạm thời không thuộc về Model
- **Ví dụ**: Dropdown list, thông báo, dữ liệu hỗ trợ cho giao diện


### Đặc điểm kỹ thuật của ViewBag

- **Dynamic Property**: Tận dụng tính năng dynamic của C\# 4.0
- **Linh hoạt**: Có thể gán bất kỳ số lượng properties và values nào
- **Thời gian sống**: Chỉ tồn tại trong HTTP request hiện tại
- **Giới hạn**: Giá trị sẽ null nếu có redirection
- **Bản chất**: Là wrapper xung quanh ViewData


### Cách thực hiện với Category Dropdown

**Bước 1: Di chuyển code lấy CategoryList**

Từ action Index, di chuyển code vào action Create:

```csharp
public IActionResult Create()
{
    IEnumerable<SelectListItem> categoryList = _unitOfWork.Category.GetAll()
        .Select(u => new SelectListItem
        {
            Text = u.Name,
            Value = u.Id.ToString()
        });
    
    // Gán vào ViewBag
    ViewBag.CategoryList = categoryList;
    
    return View();
}
```

**Bước 2: Sử dụng ViewBag trong View**

ViewBag hoạt động như key-value pairs:

- **Key**: `CategoryList` (tên tự định nghĩa)
- **Value**: `categoryList` (dữ liệu IEnumerable<SelectListItem>)

**Bước 3: Tạo dropdown trong Create.cshtml**

Thêm sau trường Price100:

```html
<div class="mb-3">
    <label asp-for="CategoryId" class="form-label"></label>
    <select asp-for="CategoryId" asp-items="@ViewBag.CategoryList" class="form-select">
        <option disabled selected>Select Category</option>
    </select>
    <span asp-validation-for="CategoryId" class="text-danger"></span>
</div>
```


### Chi tiết implementation

**Sử dụng select tag helper:**

- `asp-for="CategoryId"`: Bind với thuộc tính CategoryId của Product
- `asp-items="@ViewBag.CategoryList"`: Lấy dữ liệu dropdown từ ViewBag
- **Lưu ý**: Phải có explicit closing tag `</select>`, không dùng self-closing

**Default option:**

- `disabled selected`: Option mặc định không thể chọn
- Hiển thị "Select Category" để hướng dẫn người dùng

**Bootstrap styling:**

- `class="form-select"`: Class Bootstrap cho dropdown


### Lưu ý quan trọng về ViewBag

**Vấn đề IntelliSense:**

- Do tính chất dynamic, không có IntelliSense support
- Phải typing chính xác tên key: `ViewBag.CategoryList`
- Lỗi chính tả sẽ gây ra runtime error

**Best practices:**

- Kiểm tra spelling cẩn thận
- Sử dụng consistent naming convention
- Đảm bảo key name khớp giữa Controller và View


### Kết quả

Sau khi implement:

- Truy cập `/Product/Create`
- Scroll xuống phần Category selection
- Dropdown hiển thị: "Select Category", "Action", "SciFi", "History"
- User có thể chọn Category cho Product


### So sánh với các phương pháp khác

ViewBag là một trong nhiều cách truyền dữ liệu:

- **ViewBag**: Dynamic, dễ sử dụng nhưng thiếu type safety
- **ViewData**: Tương tự ViewBag nhưng dùng indexer syntax
- **ViewModel**: Type-safe nhưng phức tạp hơn
- **TempData**: Cho dữ liệu persist qua redirect


### Ghi chú thêm

- ViewBag phù hợp cho dữ liệu đơn giản, không cần validation
- Đối với dữ liệu phức tạp, nên cân nhắc sử dụng ViewModel
- Cần implement tương tự cho Edit action để có dropdown khi chỉnh sửa Product
- Có thể mở rộng để hiển thị selected category trong Edit mode

**Liên kết:** [[ViewBag]], [[ViewData]], [[Product Controller]], [[Category Model]], [[SelectListItem]], [[Tag Helpers]], [[ASP.NET Core MVC]], [[Dynamic Properties]], [[HTTP Request]], [[Bootstrap Forms]], [[Dropdown]], [[Controller to View]]

