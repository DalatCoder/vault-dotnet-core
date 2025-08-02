## Hiển thị danh sách Category từ Database trong Razor Pages

### So sánh cách tiếp cận giữa MVC Controller và Page Model

#### Trong MVC Controller:

- Sử dụng Dependency Injection để lấy `ApplicationDbContext`
- Truy xuất dữ liệu và trả về thông qua `View()`
- Logic xử lý nằm trong Action Method


#### Trong Razor Pages:

- Sử dụng **Page Model** (.cs file) thay vì Controller
- Logic xử lý nằm trong **OnGet handler**
- Không cần `return View()` - dữ liệu tự động bind


### Cấu hình Page Model

#### Tạo constructor với Dependency Injection:

```csharp
private readonly ApplicationDbContext _db;

public IndexModel(ApplicationDbContext db)
{
    _db = db;
}
```


#### Tạo property để lưu danh sách:

```csharp
public List<Category> CategoryList { get; set; }
```


#### Implement OnGet handler:

```csharp
public void OnGet()
{
    CategoryList = _db.Categories.ToList();
}
```

**Lưu ý quan trọng:**

- Return type là `void` (không cần `return View()`)
- Dữ liệu tự động bind vào model
- OnGet handler tương đương với GET action method trong MVC


### Chỉnh sửa Razor Page (.cshtml)

#### Copy code từ MVC View:

- Sao chép nội dung từ `Views/Category/Index.cshtml`
- Xóa directive `@model` (đã có sẵn trong Razor Page)
- **Chú ý**: Đóng file MVC để tránh nhầm lẫn khi edit


#### Thay đổi cách truy cập dữ liệu:

- **MVC**: Sử dụng `Model` trực tiếp
- **Razor Pages**: Sử dụng `Model.CategoryList`

```html
@foreach (var obj in Model.CategoryList)
{
    <!-- Hiển thị category -->
}
```


### Cập nhật Tag Helpers

#### Thay đổi từ MVC sang Razor Pages:

- **MVC**: `asp-controller` và `asp-action`
- **Razor Pages**: `asp-page`


#### Ví dụ cụ thể:

```html
<!-- Nút Create -->
<a asp-page="Category/Create" class="btn btn-primary">Create New Category</a>

<!-- Nút Edit -->
<a asp-page="Category/Edit" asp-route-id="@obj.Id" class="btn btn-primary">Edit</a>

<!-- Nút Delete -->
<a asp-page="Category/Delete" asp-route-id="@obj.Id" class="btn btn-danger">Delete</a>
```

**Ghi chú:**

- `asp-route-id` vẫn hoạt động như bình thường
- Phải cung cấp đường dẫn đầy đủ (ví dụ: `Category/Edit`)
- Không phân biệt chữ hoa/thường trong route


### Cấu hình Navigation Menu

#### Thêm link vào _Layout.cshtml:

```html
<li class="nav-item">
    <a class="nav-link" asp-page="/Categories/Index">Category</a>
</li>
```

- Sử dụng `asp-page` thay vì `asp-controller`/`asp-action`
- Đường dẫn: `/Categories/Index`


### Quy tắc đặt tên Handler Methods

#### Naming Convention bắt buộc:

- **Đúng**: `OnGet()` - bắt đầu bằng "On"
- **Sai**: `Get()` - thiếu prefix "On"


#### Kiểm tra hoạt động:

- Nếu đặt tên sai, page sẽ không load được
- Handler method phải match chính xác với naming convention
- `OnGet` cho GET requests, `OnPost` cho POST requests


### Kết quả và kiểm tra

#### Tính năng hoạt động:

- Hiển thị danh sách categories từ database
- Navigation menu có link đến Category page
- Các nút Create, Edit, Delete hiển thị đúng
- Sử dụng Bootstrap classes mặc định (chưa có Bootswatch theme)


#### Optional enhancements:

- Có thể thêm Bootstrap Icons
- Áp dụng Bootswatch theme nếu muốn


### Ghi chú thêm

- **Page Model** là trung tâm xử lý logic trong Razor Pages
- Dependency Injection hoạt động tương tự như MVC
- Tag helpers được điều chỉnh phù hợp với Razor Pages routing
- Cấu trúc đơn giản hơn MVC cho các trang cơ bản
- Không cần explicit `return View()` statement

**Liên kết:** [[Razor Pages]], [[Page Model]], [[OnGet Handler]], [[Dependency Injection]], [[ApplicationDbContext]], [[Tag Helpers]], [[asp-page]], [[Bootstrap]], [[Entity Framework Core]], [[Categories]], [[Navigation Menu]]

