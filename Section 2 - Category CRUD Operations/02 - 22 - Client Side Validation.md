## Xác thực phía máy khách (Client-side Validation) trong .NET Core MVC

### Khái niệm

- **Xác thực phía máy chủ (Server-side validation)**: Khi nhấn nút Create, yêu cầu được gửi đến controller, xử lý tại máy chủ và trả về thông báo lỗi. Trang web sẽ reload và có spinner hiển thị quá trình gửi yêu cầu lên server.
- **Xác thực phía máy khách (Client-side validation)**: Thực hiện xác thực ngay trên trình duyệt bằng JavaScript mà không cần gửi yêu cầu lên server, giúp tăng trải nghiệm người dùng.


### Cách thực hiện Client-side Validation

#### Sử dụng Validation Scripts Partial

- .NET Core đã cung cấp sẵn các công cụ xác thực phía máy khách
- File `_ValidationScriptsPartial.cshtml` chứa jQuery validation scripts cần thiết
- Partial view này nằm trong thư mục Shared


#### Thêm Partial View vào View chính

```razor
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

**Lưu ý quan trọng:**

- Tên file phải khớp chính xác với tên trong thư mục Shared
- Vì đây là partial view liên quan đến scripts nên được thêm vào section Scripts
- Nếu không phải script, có thể thêm trực tiếp vào phần nội dung chính của view


### Cách hoạt động

#### Client-side Validation

- Xác thực được thực hiện ngay trên trình duyệt
- Không có spinner loading
- Không gửi yêu cầu đến server
- Hiển thị lỗi ngay lập tức khi người dùng nhập liệu


#### Server-side Validation (cho Custom Validation)

- Các validation tùy chỉnh vẫn cần xử lý trên server
- Ví dụ: "Category name và display order không được giống nhau"
- Client-side validation không biết về các rule tùy chỉnh này
- Khi có custom validation, yêu cầu vẫn được gửi đến server để kiểm tra


### Ví dụ thực tế

- **Validation chuẩn**: Required field, MaxLength - được xử lý client-side
- **Custom validation**: So sánh giá trị giữa các field - cần xử lý server-side
- Khi nhập liệu, validation hiển thị ngay lập tức mà không cần submit form


### Ưu điểm

- Cải thiện trải nghiệm người dùng (UX)
- Giảm tải cho server
- Phản hồi nhanh chóng
- Không cần reload trang cho các validation cơ bản

**Liên kết:** [[Validation]], [[Partial View]], [[jQuery Validation]], [[Server-side Validation]], [[Client-side Validation]], [[Tag Helper]], [[Razor Syntax]], [[MVC Pattern]]

