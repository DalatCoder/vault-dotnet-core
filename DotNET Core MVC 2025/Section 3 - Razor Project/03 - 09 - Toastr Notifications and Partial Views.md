## Thêm Toaster Notification vào dự án Razor Pages

### Tạo Partial View cho Notification

#### Các bước thực hiện:

- Copy nội dung từ partial view trong dự án MVC
- Trong dự án Razor Pages, truy cập thư mục `Shared`
- Thêm **Razor Page** mới với tên `_Notification.cshtml`


#### Xử lý file được tạo:

- Hệ thống tự động tạo cả `.cshtml` và `.cshtml.cs` file
- **Xóa file `.cs`** vì partial view không cần Page Model
- Xóa toàn bộ code template mặc định
- Paste nội dung đã copy từ dự án MVC

**Lưu ý:** Partial view không cần Page Model, chỉ cần file `.cshtml`

### Cấu hình Layout Template

#### Thêm partial view vào _Layout.cshtml:

```html
@await Html.PartialAsync("_Notification")
```

- Đặt trước `@RenderBody()` trong layout
- Đảm bảo notification hiển thị trên mọi trang


#### Thêm Toaster CSS/JS references:

- Copy đường dẫn Toaster URL từ dự án MVC
- Paste vào `_Layout.cshtml` trong dự án Razor Pages
- Đóng dự án MVC để tránh nhầm lẫn


### Cấu hình TempData trong POST Handlers

#### Thêm thông báo cho từng operation:

**Create Page:**

```csharp
public IActionResult OnPost()
{
    _db.Categories.Add(Category);
    _db.SaveChanges();
    TempData["success"] = "Category created successfully";
    return RedirectToPage("Index");
}
```

**Edit Page:**

```csharp
public IActionResult OnPost()
{
    _db.Categories.Update(Category);
    _db.SaveChanges();
    TempData["success"] = "Category updated successfully";
    return RedirectToPage("Index");
}
```

**Delete Page:**

```csharp
public IActionResult OnPost()
{
    _db.Categories.Remove(obj);
    _db.SaveChanges();
    TempData["success"] = "Category deleted successfully";
    return RedirectToPage("Index");
}
```


### Kiểm tra chức năng hoạt động

#### Test các operations:

- **Edit**: Chỉnh sửa category "History" → hiển thị notification thành công
- **Delete**: Xóa category "Test Two" → hiển thị notification thành công
- **Create**: Tạo category mới → hiển thị notification thành công


#### Kết quả:

- Toaster notification hiển thị đúng cho mọi thao tác CRUD
- Giao diện và hoạt động giống hệt dự án MVC
- Tích hợp mượt mà với Razor Pages architecture


### Tổng kết và ý nghĩa

#### Kiến thức đã học:

- **CRUD operations** hoàn chỉnh trong cả **MVC** và **Razor Pages**
- So sánh hai kiến trúc: Controllers vs Page Models
- Cách thức routing khác nhau giữa hai approach
- Property binding và handler methods trong Razor Pages


#### Mục đích học Razor Pages:

- **Chuẩn bị cho .NET Identity**: Identity class library được xây dựng trên Razor Pages
- Khi customize Identity pages, sẽ làm việc với:
    - **Page Model** thay vì Controller
    - **OnGet** và **OnPost handlers** thay vì Action methods
    - Cấu trúc tương tự như đã thực hành


#### Ứng dụng trong tương lai:

- Hiểu được cấu trúc Identity pages để customization
- Có thể chỉnh sửa login, register, profile pages
- Áp dụng kiến thức về GET/POST handlers trong Identity context


### Ưu điểm của Razor Pages

#### So với MVC:

- **Đơn giản hóa routing**: Cấu trúc thư mục = URL structure
- **Tập trung logic**: Mỗi page có Page Model riêng
- **Dễ bảo trì**: Logic và UI của một trang ở cùng nơi
- **Phù hợp Identity**: Microsoft sử dụng cho Authentication system


#### Khi nào sử dụng:

- **Razor Pages**: Ứng dụng đơn giản, form-based applications
- **MVC**: Ứng dụng phức tạp, cần separation of concerns cao
- **Identity**: Luôn sử dụng Razor Pages (do Microsoft design)


### Ghi chú thêm

- **TempData** hoạt động giống nhau trong cả MVC và Razor Pages
- **Partial Views** có thể chia sẻ giữa hai kiến trúc
- **Entity Framework Core** integration hoàn toàn tương tự
- Toaster notification library (như Toastr.js) hoạt động độc lập với backend architecture
- Kiến thức này là nền tảng quan trọng cho các section về Identity authentication

**Liên kết:** [[Razor Pages]], [[Toaster Notification]], [[Partial Views]], [[TempData]], [[POST Handlers]], [[CRUD Operations]], [[MVC vs Razor Pages]], [[.NET Identity]], [[Page Model]], [[OnGet]], [[OnPost]], [[Entity Framework Core]], [[Layout Template]], [[Authentication System]]

