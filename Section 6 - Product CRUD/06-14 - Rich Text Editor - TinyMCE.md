## Tích hợp TinyMCE Rich Text Editor cho Product Description

### Khái niệm

Thay vì sử dụng text box thông thường cho trường **Description**, cần nâng cấp thành **Rich Text Editor** để người dùng có thể định dạng nội dung phong phú hơn. **TinyMCE** là một JavaScript editor mạnh mẽ và miễn phí phù hợp cho yêu cầu này.

### Ưu điểm của TinyMCE

- **Miễn phí**: Core version miễn phí vĩnh viễn
- **Fancy Editor**: Giao diện đẹp với nhiều tính năng formatting
- **Dễ tích hợp**: Chỉ cần thêm script và cấu hình đơn giản
- **Tùy chỉnh linh hoạt**: Có thể enable/disable các plugins theo nhu cầu


### Cách thực hiện

**Bước 1: Đăng ký TinyMCE Account**

- Truy cập website TinyMCE official
- Đăng ký tài khoản miễn phí (có thể dùng Google account)
- Truy cập Cloud Dashboard để lấy API key và script

**Bước 2: Thêm TinyMCE Script vào Layout**

Trong file `_Layout.cshtml`, thêm script vào cuối trang trước thẻ đóng `</body>`:

```html
<!-- Thêm TinyMCE script từ CDN -->
<script src="https://cdn.tiny.cloud/1/YOUR_API_KEY/tinymce/6/tinymce.min.js" referrerpolicy="origin"></script>

<!-- Existing scripts -->
<script src="~/lib/jquery/dist/jquery.min.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
<script src="~/js/site.js" asp-append-version="true"></script>
```

**Lưu ý**: API key đã được include sẵn trong script URL, không cần cấu hình thêm.

**Bước 3: Cập nhật Description Field trong Upsert.cshtml**

Thay đổi từ `input` thành `textarea` và thêm cấu hình TinyMCE:

```html
<!-- Trước đây: Input text thông thường -->
<div class="mb-3">
    <label asp-for="Product.Description" class="form-label"></label>
    <input asp-for="Product.Description" class="form-control" />
    <span asp-validation-for="Product.Description" class="text-danger"></span>
</div>

<!-- Sau khi cập nhật: Textarea với TinyMCE -->
<div class="mb-3">
    <label asp-for="Product.Description" class="text-muted">Description</label>
    <textarea asp-for="Product.Description"></textarea>
</div>
```

**Bước 4: Thêm JavaScript Configuration**

Trong section Scripts của `Upsert.cshtml`:

```html
@section Scripts {
    <script>
        tinymce.init({
            selector: 'textarea',
            plugins: 'anchor autolink charmap codesample emoticons link lists media searchreplace table visualblocks wordcount',
            toolbar: 'undo redo | blocks fontfamily fontsize | bold italic underline strikethrough | link table | addcomment showcomments | spellcheckdialog a11ycheck typography | align lineheight | checklist numlist bullist indent outdent | emoticons charmap | removeformat',
            tinycomments_mode: 'embedded',
            tinycomments_author: 'Author name',
            mergetags_list: [
                { value: 'First.Name', title: 'First Name' },
                { value: 'Email', title: 'Email' },
            ]
        });
    </script>
}
```


### Tùy chỉnh TinyMCE

**Loại bỏ plugins không cần thiết:**

```javascript
tinymce.init({
    selector: 'textarea',
    plugins: 'lists link charmap emoticons wordcount',  // Giảm plugins
    toolbar: 'undo redo | bold italic underline | link | bullist numlist | removeformat',  // Giảm toolbar
    height: 300,
    menubar: false  // Ẩn menu bar
});
```

**Các plugins đã loại bỏ:**

- `image`: Không cần thiết vì có file upload riêng
- `media`: Không sử dụng cho text description
- `searchreplace`: Quá phức tạp cho description field
- `table`: Không phù hợp với product description


### Cải thiện giao diện

**Styling cho label:**

```html
<label asp-for="Product.Description" class="text-muted">Description</label>
```

- Sử dụng `text-muted` class cho màu xám nhẹ
- Loại bỏ validation span vì không cần thiết
- Loại bỏ `form-floating` để giao diện gọn gàng hơn


### Kết quả

Sau khi implement:

- **Rich Text Editor**: Description field hiển thị như editor chuyên nghiệp
- **Formatting Options**: Bold, italic, underline, lists, links
- **User Experience**: Dễ sử dụng với toolbar trực quan
- **Responsive**: Hoạt động tốt trên các thiết bị


### Chuẩn bị cho bước tiếp theo

- **File Upload**: Sẵn sàng implement chức năng upload và lưu trữ hình ảnh
- **Image URL**: Cần cập nhật ImageURL field khi user chọn file
- **Storage**: Cần xác định nơi lưu trữ file (local hoặc cloud)


### Lưu ý quan trọng

- **API Key**: Mỗi project cần API key riêng từ TinyMCE
- **Selector**: TinyMCE sử dụng CSS selector để target textarea
- **Explicit Closing Tag**: Textarea bắt buộc phải có `</textarea>`
- **Section Scripts**: JavaScript phải đặt trong section để load sau TinyMCE script
- **Performance**: Chỉ load TinyMCE ở những trang thực sự cần thiết


### Ghi chú thêm

- TinyMCE có nhiều themes và skins để tùy chỉnh giao diện
- Có thể integrate với cloud storage cho image upload trong editor
- Version 6 là phiên bản mới nhất với nhiều cải tiến
- Có thể validate HTML content ở server-side để đảm bảo security
- Editor content được submit như HTML string qua form POST

**Liên kết:** [[ProductViewModel]], [[Product Controller]], [[Upsert]], [[Rich Text Editor]], [[JavaScript]], [[TinyMCE]], [[HTML Forms]], [[File Upload]], [[ImageURL]], [[Product Model]], [[ASP.NET Core MVC]], [[Bootstrap Forms]], [[Client-side Scripting]]

