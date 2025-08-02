## Tạo trang Create Category mới

### Quy trình tạo trang mới trong MVC

Khi làm việc với việc tạo trang mới, **không trực tiếp tạo view** mà phải tuân theo quy trình:

1. Tạo action method (phương thức hành động) trong controller
2. Action method sẽ được gọi và trả về view tương ứng
3. Tạo view với tên trùng khớp với action method

### Tạo Action Method trong Category Controller

```csharp
public IActionResult Create()
{
    return View();
}
```

**Đặc điểm quan trọng:**

- Kiểu trả về: `IActionResult`
- Tên method: `Create` (sẽ khớp với tên view)
- Chỉ đơn giản return View() để hiển thị form tạo mới


### Tạo View tương ứng

**Cách tạo view:**

- **Thủ công**: Navigate đến thư mục Views/Category và tạo file mới
- **Shortcut**: Right-click trên action method → "Add View" → chọn "Empty Razor View"

**Lưu ý quan trọng:**

- Tên view phải **trùng khớp chính xác** với tên action method
- Trong trường hợp này: `Create.cshtml`


### Định nghĩa Model cho Create View

```csharp
@model Category
```

**Giải thích:**

- Model sẽ là `Category` object
- Không cần truyền `new Category()` từ controller
- Nếu không truyền gì, system sẽ tự tạo Category object với default values
- Đây chính là điều chúng ta muốn cho form tạo mới


### Thiết lập Navigation từ Index sang Create

```html
<a asp-controller="Category" asp-action="Create" class="btn btn-primary">
    <i class="bi bi-plus-circle"></i> Create New Category
</a>
```

**Best Practices về Navigation:**

- Luôn **explicit** (rõ ràng) khi định nghĩa controller và action
- Viết `asp-controller` trước `asp-action` để dễ đọc và debug
- Ngay cả khi ở cùng controller, vẫn nên khai báo đầy đủ


### Tạo Form với Bootstrap Styling

#### Cấu trúc Form cơ bản

```html
<form method="post">
    <div class="border p-3 mt-4">
        <div class="row pb-2">
            <h2 class="text-primary">Create Category</h2>
            <hr />
        </div>
        
        <!-- Form fields sẽ được thêm vào đây -->
    </div>
</form>
```


#### Input Fields cho Category

```html
<!-- Category Name -->
<div class="mb-3">
    <label>Category Name</label>
    <input type="text" class="form-control" />
</div>

<!-- Display Order -->
<div class="mb-3">
    <label>Display Order</label>
    <input type="text" class="form-control" />
</div>
```


#### Buttons Layout với Responsive Design

```html
<div class="row">
    <div class="col-6 col-md-3">
        <button type="submit" class="btn btn-primary form-control">
            Create
        </button>
    </div>
    <div class="col-6 col-md-3">
        <a asp-controller="Category" asp-action="Index" 
           class="btn btn-outline-secondary border form-control">
            Back to List
        </a>
    </div>
</div>
```


### Responsive Design Implementation

**Bootstrap Grid System được sử dụng:**

- `col-6`: Chiếm 6 cột trên màn hình nhỏ (mobile)
- `col-md-3`: Chiếm 3 cột trên màn hình medium trở lên (desktop)
- `form-control`: Đảm bảo full width cho buttons

**Padding và Spacing:**

```html
<div class="mb-3 p-0">  <!-- margin-bottom 3, padding 0 -->
<div class="p-1">       <!-- padding 1 cho buttons -->
```


### Kiểm tra tính năng

**Cách test:**

1. Chạy application (cần restart khi thay đổi controller)
2. Truy cập `/Category`
3. Click "Create New Category"
4. Kiểm tra form hiển thị đúng
5. Test responsive bằng F12 Developer Tools

### Kết quả đạt được

- Trang Create Category với form đầy đủ chức năng
- Design responsive hoạt động tốt trên mọi thiết bị
- Navigation mượt mà giữa danh sách và trang tạo mới
- UI chuyên nghiệp với Bootstrap styling
- Buttons được bố trí hợp lý và dễ sử dụng


### Bước tiếp theo

Form hiện tại chỉ có giao diện. Video tiếp theo sẽ xử lý:

- Model binding cho form data
- POST action method để xử lý việc tạo category
- Validation và error handling

**Liên kết:** [[MVC Action Methods]], [[Razor Views]], [[Bootstrap Forms]], [[Category Model]], [[Form Handling]], [[Responsive Design]], [[ASP.NET Core Navigation]]

