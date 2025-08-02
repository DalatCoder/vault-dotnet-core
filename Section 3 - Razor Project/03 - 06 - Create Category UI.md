## Tạo trang Create Category trong Razor Pages

### Vấn đề với routing

- Khi click "Create New Category", trang không thể route đến đúng địa chỉ
- **Khác biệt với MVC**:
    - MVC sẽ hiển thị lỗi "Page not found"
    - Razor Pages không redirect nếu không tìm thấy trang


### Tạo trang Create mới

#### Các bước thực hiện:

- Trong thư mục `Categories`, thêm **Empty Razor Page** mới
- Đặt tên: `Create.cshtml`
- Hệ thống tự động tạo file `Create.cshtml.cs` (Page Model)


#### Copy UI từ dự án MVC:

- Sao chép nội dung từ `Views/Category/Create.cshtml`
- Paste vào `Create.cshtml` trong dự án Razor Pages
- Đóng file MVC để tránh nhầm lẫn


### Cấu hình Page Model

#### Thêm ApplicationDbContext:

```csharp
private readonly ApplicationDbContext _db;

public CreateModel(ApplicationDbContext db)
{
    _db = db;
}
```


#### Tạo property cho Category:

```csharp
public Category Category { get; set; }
```

**Lưu ý:**

- Đổi constructor name thành `CreateModel`
- Thêm using statements cần thiết
- Khác với Index page (dùng List), Create chỉ cần một Category object


### Chỉnh sửa Razor Page

#### Thay đổi cách truy cập dữ liệu:

- **Trước**: `@Model.Name` (trong MVC)
- **Sau**: `@Model.Category.Name` (trong Razor Pages)


#### Cập nhật routing:

```html
<!-- Nút Back to List -->
<a asp-page="/Categories/Index" class="btn btn-secondary">Back to List</a>
```

**Quan trọng**: Phải có dấu `/` ở đầu đường dẫn (`/Categories/Index`)

### Xử lý lỗi routing

#### Vấn đề phổ biến:

- Thiếu dấu `/` đầu đường dẫn → routing không hoạt động
- Ví dụ sai: `asp-page="categories/index"`
- Ví dụ đúng: `asp-page="/Categories/Index"`


#### Kiểm tra và sửa lỗi:

- Cập nhật tất cả `asp-page` attributes
- Đảm bảo có dấu `/` ở đầu mọi đường dẫn
- Test lại chức năng "Back to List"


### Tính năng validation

#### Validation tự động hoạt động:

- File `Create.cshtml` có `_ValidationScriptsPartial`
- Razor Pages hỗ trợ validation mặc định
- Client-side validation hoạt động như MVC


#### Kiểm tra validation:

- Để trống form và nhấn Create
- Hiển thị thông báo validation error
- Tương tự như trong dự án MVC


### Hạn chế hiện tại

#### Chức năng chưa hoàn thiện:

- Chỉ có **OnGet handler** - load trang trống
- Chưa có **OnPost handler** để xử lý form submit
- Nhấn "Create" button không có gì xảy ra


#### Các trang chưa tạo:

- Edit page → chưa có, link sẽ lỗi
- Delete page → chưa có, link sẽ lỗi
- Chỉ có Create page hoạt động


### Workflow hiện tại

#### Khi load trang Create:

- OnGet handler được gọi (hiện tại rỗng)
- Hiển thị empty form với text boxes
- Validation scripts được load


#### Khi submit form:

- Hiện tại không có xử lý
- Cần OnPost handler để lưu dữ liệu vào database
- Sẽ được thực hiện trong video tiếp theo


### Ghi chú thêm

- **UI giống hệt MVC**: Copy trực tiếp từ Views
- **Page Model khác Controller**: Cần điều chỉnh cách truy cập properties
- **Routing khác biệt**: Phải có dấu `/` đầu đường dẫn
- **Validation tương tự**: Razor Pages hỗ trợ validation như MVC
- **Cần POST handler**: Để hoàn thiện chức năng Create

**Liên kết:** [[Razor Pages]], [[Page Model]], [[OnGet Handler]], [[asp-page]], [[ApplicationDbContext]], [[Validation]], [[Routing]], [[Empty Razor Page]], [[Categories]], [[Create Category]], [[POST Handler]]

