## Sử dụng Partial View để tái sử dụng logic hiển thị thông báo

### Vấn đề với việc lặp lại code

#### Bối cảnh thực tế

- **Nhiều trang CRUD**: Website có thể có Category, Cover Type, Products và nhiều entity khác
- **Logic giống nhau**: Tất cả đều cần hiển thị thông báo thành công/lỗi từ TempData
- **Code duplication**: Copy-paste cùng một đoạn code vào nhiều view khác nhau
- **Khó bảo trì**: Khi cần thay đổi, phải sửa ở nhiều nơi


#### Ví dụ code bị lặp lại

```html
<!-- Code này sẽ xuất hiện ở nhiều view khác nhau -->
@if (TempData["success"] != null)
{
    <h2>@TempData["success"]</h2>
}
```


### Giải pháp với Partial View

#### Khái niệm Partial View

- **Định nghĩa**: View nhỏ, có thể tái sử dụng trong nhiều view khác
- **Mục đích**: Tách logic chung thành component độc lập
- **Ví dụ đã thấy**: `_ValidationScriptsPartial.cshtml`
- **Convention**: Tên file thường bắt đầu với dấu gạch dưới `_`


### Tạo Partial View cho Notification

#### Các bước thực hiện

1. **Tạo file**: Right-click vào **Shared folder** → Add → View → Empty View
2. **Đặt tên**: `_Notification.cshtml` (bắt đầu với `_` để nhận biết là partial view)
3. **Thêm logic**: Di chuyển code TempData từ view chính vào partial view

#### Code của _Notification.cshtml

```html
@* File: Views/Shared/_Notification.cshtml *@

@if (TempData["success"] != null)
{
    <h2>@TempData["success"]</h2>
}

@if (TempData["error"] != null)
{
    <h2>@TempData["error"]</h2>
}
```


#### Cấu trúc thư mục

```
Views/
├── Shared/
│   ├── _Layout.cshtml
│   ├── _ValidationScriptsPartial.cshtml
│   └── _Notification.cshtml          ← Partial view mới tạo
├── Category/
│   ├── Index.cshtml
│   ├── Create.cshtml
│   └── Edit.cshtml
```


### Sử dụng Partial View trong View chính

#### Cú pháp consume partial view

```html
<!-- Trong Index.cshtml -->
<partial name="_Notification" />

<!-- Phần còn lại của view -->
<div class="container">
    <!-- Nội dung chính của trang -->
</div>
```


#### Lưu ý quan trọng

- **Tên chính xác**: Phải viết đúng tên file, bao gồm cả dấu gạch dưới
- **Không cần extension**: Không cần thêm `.cshtml`
- **Case-sensitive**: Chú ý viết hoa/thường cho đúng
- **Vị trí**: Thường đặt ở đầu view để hiển thị thông báo trước nội dung chính


### Kiểm tra hoạt động

#### Test case

- **Chỉnh sửa category**: Thay đổi thông tin → Hiển thị "Category updated successfully"
- **Kết quả**: Thông báo vẫn hiển thị bình thường như trước
- **Confirmation**: Partial view hoạt động chính xác


### Lợi ích của Partial View

#### Tái sử dụng (Reusability)

- **Multiple pages**: Có thể sử dụng trong Category, Products, Cover Type, etc.
- **One line code**: Chỉ cần thêm `<partial name="_Notification" />` vào mỗi view
- **Consistent UI**: Đảm bảo giao diện thống nhất trên tất cả các trang


#### Dễ bảo trì (Maintainability)

- **Single source of truth**: Logic notification chỉ có ở một nơi
- **Easy updates**: Thay đổi trong partial view sẽ áp dụng cho tất cả view sử dụng nó
- **No duplication**: Không cần copy-paste code


#### Ví dụ thực tế

```html
<!-- Views/Category/Index.cshtml -->
<partial name="_Notification" />
<!-- Nội dung category -->

<!-- Views/Products/Index.cshtml -->
<partial name="_Notification" />
<!-- Nội dung products -->

<!-- Views/CoverType/Index.cshtml -->
<partial name="_Notification" />
<!-- Nội dung cover type -->
```


### Mở rộng thêm

#### Hỗ trợ nhiều loại thông báo

```html
@* Có thể mở rộng để hỗ trợ thêm các loại thông báo khác *@
@if (TempData["success"] != null)
{
    <div class="alert alert-success">@TempData["success"]</div>
}

@if (TempData["error"] != null)
{
    <div class="alert alert-danger">@TempData["error"]</div>
}

@if (TempData["warning"] != null)
{
    <div class="alert alert-warning">@TempData["warning"]</div>
}
```


#### Best practices cho Partial Views

- **Shared folder**: Đặt partial view có thể tái sử dụng trong `/Views/Shared/`
- **Naming convention**: Bắt đầu tên file với dấu gạch dưới `_`
- **Single responsibility**: Mỗi partial view nên có một mục đích cụ thể
- **No business logic**: Chỉ chứa presentation logic, không có business logic


### Ghi chú quan trọng

#### Convention và Best Practices

- **Underscore prefix**: `_Notification.cshtml` dễ nhận biết là partial view
- **Shared location**: Đặt trong `/Views/Shared/` để sử dụng globally
- **Spell check**: Viết đúng tên file khi consume, sai chính tả sẽ lỗi
- **Reusable components**: Áp dụng cho mọi component có thể tái sử dụng


#### Khi nào sử dụng Partial View

- **Repeated UI elements**: Các thành phần giao diện lặp lại
- **Complex components**: Component phức tạp cần tách riêng
- **Shared functionality**: Chức năng dùng chung nhiều nơi
- **Maintenance concerns**: Khi muốn dễ bảo trì và cập nhật

**Liên kết:** [[Partial View]], [[TempData]], [[Shared Folder]], [[View Reusability]], [[Code Duplication]], [[Maintainability]], [[UI Components]], [[ASP.NET Core MVC]], [[Razor Syntax]], [[Template Organization]]

