## Tích hợp Toaster Notification để cải thiện trải nghiệm người dùng

### Vấn đề với thông báo H2 đơn giản

Thay vì hiển thị thông báo thành công/lỗi bằng thẻ **H2 đơn thuần**, chúng ta cần sử dụng **Toaster notification** để tạo trải nghiệm người dùng chuyên nghiệp và hấp dẫn hơn.

#### Ưu điểm của Toaster

- **Giao diện đẹp**: Thông báo hiện đại, bắt mắt
- **Nhiều loại**: Success, Warning, Error với màu sắc phân biệt
- **Tự động ẩn**: Thông báo tự động biến mất sau một khoảng thời gian
- **Non-intrusive**: Không làm gián đoạn luồng công việc của người dùng


### Cài đặt Toaster Library

#### Thêm CSS CDN vào Layout

Trong file `_Layout.cshtml`, thêm CSS CDN của Toaster:

```html
<!-- Trong phần <head> của _Layout.cshtml -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css" />
```


#### Thêm JavaScript dependencies

```html
<!-- Trong _Notification.cshtml -->
<script src="~/lib/jquery/dist/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>
```


#### Lưu ý về thứ tự dependencies

- **jQuery**: Cần có trước vì Toaster phụ thuộc vào jQuery
- **Minified version**: Sử dụng phiên bản `.min.js` cho production
- **Location**: JavaScript được thêm trong partial view thay vì layout


### Cập nhật Partial View _Notification

#### Code mới với Toaster

```html
@* File: Views/Shared/_Notification.cshtml *@

@if (TempData["success"] != null)
{
    <script src="~/lib/jquery/dist/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>
    <script type="text/javascript">
        toastr.success("@TempData["success"]");
    </script>
}

@if (TempData["error"] != null)
{
    <script src="~/lib/jquery/dist/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>
    <script type="text/javascript">
        toastr.error("@TempData["error"]");
    </script>
}
```


#### So sánh với code cũ

```html
<!-- Code cũ - H2 đơn giản -->
@if (TempData["success"] != null)
{
    <h2>@TempData["success"]</h2>
}

<!-- Code mới - Toaster notification -->
<script type="text/javascript">
    toastr.success("@TempData["success"]");
</script>
```


### Di chuyển Partial View lên Layout

#### Vấn đề với việc đặt trong từng view

- **Lặp lại code**: Phải thêm `<partial name="_Notification" />` vào mỗi view
- **Khó bảo trì**: Khi có hàng trăm trang cần notification
- **Dễ quên**: Có thể quên thêm vào view mới


#### Giải pháp đặt trong Layout

```html
<!-- File: Views/Shared/_Layout.cshtml -->
<!DOCTYPE html>
<html>
<head>
    <!-- CSS và meta tags -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css" />
</head>
<body>
    <!-- Navigation và header -->
    
    <!-- Thêm partial view notification TRƯỚC RenderBody -->
    <partial name="_Notification" />
    
    @RenderBody()
    
    <!-- Footer và scripts -->
</body>
</html>
```


#### Vị trí đặt Partial View

- **Trước `@RenderBody()`**: Đảm bảo notification load trước nội dung chính
- **Global availability**: Có sẵn trên tất cả các trang sử dụng layout này
- **No duplication**: Không cần thêm vào từng view riêng lẻ


### Kiểm tra hoạt động

#### Test case thực tế

- **Edit category**: Thay đổi display order thành 99 → Hiển thị toaster success
- **Delete category**: Xóa danh mục → Hiển thị toaster notification
- **Visual improvement**: Thông báo đẹp mắt, chuyên nghiệp hơn H2 tag


#### Kết quả mong đợi

- **Success notification**: Màu xanh với icon tick
- **Error notification**: Màu đỏ với icon cảnh báo
- **Auto-hide**: Tự động biến mất sau vài giây
- **Non-blocking**: Không cần người dùng tương tác để đóng


### Lợi ích của kiến trúc Partial View

#### Maintainability (Khả năng bảo trì)

- **Single point of change**: Chỉ cần sửa ở một nơi (_Notification.cshtml)
- **Easy migration**: Muốn đổi từ Toaster sang thư viện khác? Chỉ sửa partial view
- **Consistent behavior**: Tất cả trang đều có cùng cơ chế notification


#### Scalability (Khả năng mở rộng)

- **No code duplication**: Không cần copy-paste code notification
- **Global availability**: Tự động có sẵn trên mọi trang mới
- **Future-proof**: Dễ dàng thêm tính năng mới (warning, info notifications)


#### Ví dụ so sánh

```html
<!-- ❌ Không sử dụng partial view - phải lặp lại trên mọi trang -->
<!-- Pages/Category/Index.cshtml -->
<script>toastr.success("@TempData["success"]");</script>

<!-- Pages/Products/Index.cshtml -->  
<script>toastr.success("@TempData["success"]");</script>

<!-- Pages/CoverType/Index.cshtml -->
<script>toastr.success("@TempData["success"]");</script>

<!-- ✅ Sử dụng partial view - chỉ một dòng trong layout -->
<!-- Views/Shared/_Layout.cshtml -->
<partial name="_Notification" />
```


### Cấu hình nâng cao cho Toaster

#### Tùy chọn hiển thị

```javascript
// Có thể mở rộng với các options
toastr.options = {
    "closeButton": true,
    "progressBar": true,
    "positionClass": "toast-top-right",
    "timeOut": "3000"
};

toastr.success("Category updated successfully");
```


#### Các loại notification khả dụng

- `toastr.success()` - Thông báo thành công (màu xanh)
- `toastr.error()` - Thông báo lỗi (màu đỏ)
- `toastr.warning()` - Thông báo cảnh báo (màu vàng)
- `toastr.info()` - Thông báo thông tin (màu xanh nhạt)


### Ghi chú quan trọng

#### Best Practices

- **CDN reliability**: Sử dụng CDN ổn định cho production
- **Minified versions**: Luôn dùng phiên bản minified để tối ưu tốc độ
- **Dependency order**: jQuery phải load trước Toaster
- **Global placement**: Đặt partial view trong layout cho khả năng tái sử dụng tối đa


#### Hiệu suất và UX

- **Non-blocking**: Toaster không chặn tương tác người dùng
- **Auto-dismiss**: Tự động ẩn giúp giao diện sạch sẽ
- **Visual feedback**: Cung cấp phản hồi trực quan rõ ràng cho người dùng
- **Professional appearance**: Nâng cao chất lượng ứng dụng

**Liên kết:** [[Partial View]], [[TempData]], [[Toaster Notification]], [[JavaScript Library]], [[jQuery]], [[Layout Page]], [[User Experience]], [[CDN]], [[Global Components]], [[Notification System]]

