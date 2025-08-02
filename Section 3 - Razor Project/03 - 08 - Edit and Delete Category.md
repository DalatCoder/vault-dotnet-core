## Tạo chức năng Edit và Delete trong Razor Pages

### Tạo trang Edit

#### Tạo Razor Page mới:

- Trong thư mục `Categories`, thêm **Empty Razor Page** tên `Edit.cshtml`
- Tương tự, tạo **Empty Razor Page** tên `Delete.cshtml`


#### Cấu hình Edit Page Model:

```csharp
[BindProperty]
public Category Category { get; set; }

private readonly ApplicationDbContext _db;

public EditModel(ApplicationDbContext db)
{
    _db = db;
}
```

**Lưu ý:**

- Sử dụng `[BindProperty]` để bind dữ liệu khi POST
- Constructor name phải là `EditModel` (khớp với class name)


### Xử lý OnGet Handler cho Edit

#### Nhận và xử lý ID parameter:

```csharp
public void OnGet(int? id)
{
    if (id != null && id != 0)
    {
        Category = _db.Categories.Find(id);
    }
}
```

**Đặc điểm:**

- Parameter `id` có thể nullable (`int?`)
- Kiểm tra ID hợp lệ trước khi load dữ liệu
- Sử dụng `Find()` để lấy Category theo ID


### Xử lý OnPost Handler cho Edit

#### Copy logic từ MVC Controller:

```csharp
public IActionResult OnPost()
{
    if (ModelState.IsValid)
    {
        _db.Categories.Update(Category);
        _db.SaveChanges();
        return RedirectToPage("Index");
    }
    return Page();
}
```

**Logic xử lý:**

- Kiểm tra `ModelState.IsValid`
- Sử dụng `Update()` thay vì `Add()`
- Redirect về Index nếu thành công
- Return `Page()` nếu validation lỗi


### Cấu hình Edit UI

#### Copy và chỉnh sửa từ MVC View:

- Copy nội dung từ `Views/Category/Edit.cshtml`
- Paste vào `Edit.cshtml` trong Razor Pages
- Xóa directive `@model`


#### Cập nhật cách truy cập dữ liệu:

- **Trước**: `asp-for="Name"`
- **Sau**: `asp-for="Category.Name"`
- **Trước**: `asp-for="DisplayOrder"`
- **Sau**: `asp-for="Category.DisplayOrder"`


#### Cập nhật routing:

```html
<a asp-page="/Categories/Index" class="btn btn-secondary">Back to List</a>
```


### Tạo trang Delete

#### Cấu hình Delete Page Model:

```csharp
public class DeleteModel : PageModel
{
    [BindProperty]
    public Category Category { get; set; }
    
    private readonly ApplicationDbContext _db;
    
    public DeleteModel(ApplicationDbContext db)
    {
        _db = db;
    }
}
```

**Quan trọng:** Class name phải là `DeleteModel` để binding hoạt động đúng

### Xử lý OnGet Handler cho Delete

#### Load dữ liệu để hiển thị:

```csharp
public void OnGet(int? id)
{
    if (id != null && id != 0)
    {
        Category = _db.Categories.Find(id);
    }
}
```

**Mục đích:** Hiển thị thông tin Category cần xóa để user xác nhận

### Xử lý OnPost Handler cho Delete

#### Copy logic từ MVC Controller:

```csharp
public IActionResult OnPost(int? id)
{
    Category? obj = _db.Categories.Find(id);
    if (obj == null)
    {
        return NotFound();
    }
    
    _db.Categories.Remove(obj);
    _db.SaveChanges();
    return RedirectToPage("Index");
}
```

**Logic xử lý:**

- Lấy ID từ `Category.Id`
- Tìm và kiểm tra object tồn tại
- Sử dụng `Remove()` để xóa
- Redirect về Index sau khi thành công


### Cấu hình Delete UI

#### Copy và chỉnh sửa từ MVC View:

- Copy nội dung từ `Views/Category/Delete.cshtml`
- Cập nhật tất cả properties: `Model.Name` → `Model.Category.Name`
- Cập nhật routing: `asp-page="/Categories/Index"`


#### Đặc điểm UI Delete:

- Chỉ hiển thị thông tin (read-only)
- Không có form input fields
- Có nút Delete để confirm xóa


### Kiểm tra chức năng hoàn chỉnh

#### Test Edit functionality:

- Truy cập Category list
- Click "Edit" trên một category
- Thay đổi thông tin (ví dụ: "test" → "test 33")
- Submit form → redirect về Index với dữ liệu đã cập nhật


#### Test Delete functionality:

- Click "Delete" trên một category
- Xem thông tin category cần xóa
- Confirm delete → category bị xóa khỏi database


### Validation và Error Handling

#### Validation tự động:

- Edit page có validation scripts
- Required fields được validate
- Model state được kiểm tra trước khi save


#### Error handling cơ bản:

- Kiểm tra ID null/zero
- Return `NotFound()` nếu category không tồn tại
- Return `Page()` nếu ModelState invalid


### Điểm quan trọng cần nhớ

#### Naming Convention:

- **Class name**: `EditModel`, `DeleteModel` - phải khớp chính xác
- **Constructor name**: Phải khớp với class name
- **File binding**: Razor page tự động bind với Page Model cùng tên


#### Property Binding:

- Sử dụng `[BindProperty]` cho properties cần bind
- Không cần parameter trong OnPost method
- Truy cập trực tiếp qua `Category` property


#### Routing:

- Sử dụng `asp-page` thay vì `asp-controller`/`asp-action`
- Đường dẫn phải có dấu `/` đầu: `/Categories/Index`
- `asp-route-id` vẫn hoạt động để truyền parameters


### Bài tập tiếp theo

#### Nhiệm vụ được giao:

- Hoàn thiện **toaster notification** (thông báo popup)
- Implement **partial view** để tái sử dụng code
- Áp dụng cho tất cả operations: Create, Edit, Delete


#### Kỹ năng cần áp dụng:

- TempData để truyền thông báo giữa các page
- Partial views trong Razor Pages
- Notification styling với Bootstrap

**Liên kết:** [[Razor Pages]], [[Edit Functionality]], [[Delete Functionality]], [[BindProperty]], [[OnGet Handler]], [[OnPost Handler]], [[ModelState]], [[Page Model]], [[ApplicationDbContext]], [[Entity Framework Core]], [[Categories]], [[CRUD Operations]], [[Partial Views]], [[TempData]]

