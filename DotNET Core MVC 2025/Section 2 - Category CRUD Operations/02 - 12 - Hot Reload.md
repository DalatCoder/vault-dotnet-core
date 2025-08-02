## Truyền dữ liệu từ Controller sang View và tạo bảng hiển thị

### Vấn đề cần giải quyết

- Đã có danh sách categories trong `objCategoryList` ở controller
- Cần truyền dữ liệu này sang [[View]] để hiển thị
- Thay vì hiển thị text đơn giản "Category List", cần tạo bảng (table) để hiển thị dữ liệu có cấu trúc


### Tạo bảng HTML với Bootstrap

```html
<table class="table table-bordered table-striped">
    <tr>
        <th>Category Name</th>
        <th>Display Order</th>
    </tr>
</table>
```

**Cấu trúc HTML Table:**

- `<table>`: Phần tử chính chứa toàn bộ bảng
- `<tr>`: Table row - hàng trong bảng
- `<th>`: Table header - tiêu đề cột
- Sử dụng các class Bootstrap: `table`, `table-bordered`, `table-striped` để tạo kiểu dáng

**Các cột hiển thị:**

- **Category Name**: Tên danh mục
- **Display Order**: Thứ tự hiển thị


### Tính năng Hot Reload trong .NET 7

**Khái niệm:**

- [[Hot Reload]] cho phép tự động làm mới giao diện khi có thay đổi
- Không cần khởi động lại ứng dụng khi chỉnh sửa view

**Cách hoạt động:**

- Áp dụng cho các thay đổi trong [[View]] (HTML, CSS, JavaScript)
- **Không áp dụng** cho thay đổi trong [[Controller]] - vẫn cần rebuild project
- Tự động phản ánh thay đổi ngay lập tức trên trình duyệt

**Cách kích hoạt:**

- Tìm biểu tượng Hot Reload trong IDE
- Click vào mũi tên bên cạnh biểu tượng
- Bật tùy chọn "Hot Reload on File Save"
- Khởi động lại ứng dụng để áp dụng cài đặt


### Ưu điểm của Hot Reload

- Tiết kiệm thời gian phát triển đáng kể
- Không cần restart application liên tục
- Phản hồi tức thời khi chỉnh sửa giao diện
- Tích hợp sẵn trong .NET 7, không cần cài đặt thêm


### Quy trình làm việc hiệu quả

**Khi chỉnh sửa View:**

- Thay đổi HTML/CSS/JavaScript
- Lưu file (Ctrl+S)
- Tự động làm mới trình duyệt

**Khi chỉnh sửa Controller:**

- Thay đổi logic C\#
- Cần rebuild project
- Khởi động lại ứng dụng


### Bước tiếp theo

- Bảng header đã được tạo thành công
- Cần hiển thị dữ liệu chi tiết từ `objCategoryList`
- Sử dụng vòng lặp để render các hàng dữ liệu
- Truyền dữ liệu từ controller sang view thông qua [[Model]]


### Ghi chú thêm

- Hot Reload là tính năng mạnh mẽ giúp tăng năng suất lập trình
- Bootstrap classes giúp tạo giao diện đẹp và responsive nhanh chóng
- Cấu trúc HTML table chuẩn đảm bảo tính accessibility và SEO tốt

