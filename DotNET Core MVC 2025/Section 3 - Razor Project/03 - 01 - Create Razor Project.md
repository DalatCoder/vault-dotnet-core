## Giới thiệu Razor Pages trong .NET Core

### Khái niệm cơ bản

- Dự án MVC hiện tại đã hoàn thành các thao tác CRUD (Create, Read, Update, Delete) trên danh mục (categories)
- **Razor Pages** là một cách tiếp cận khác để viết code thay vì sử dụng mô hình MVC (Model-View-Controller)
- Cách tiếp cận này tương tự với phương pháp lập trình .NET cũ, nơi có trang backend và frontend riêng biệt


### Tạo dự án Razor Pages mới

#### Các bước thực hiện:

- Trong solution hiện tại, thêm một project mới
- Chọn loại project: **ASP.NET Core Web App** (không phải loại MVC)
- Đặt tên project: `bulky web razor_temp`
- Cấu hình:
    - Authentication: None
    - Các cài đặt khác giữ nguyên mặc định


#### Cấu trúc dự án:

- Project mới sẽ được thêm vào solution với tên `bulkyweb razor`
- Có thể đặt làm startup project để chạy thay vì project MVC hiện tại


### So sánh MVC và Razor Pages

#### Giao diện người dùng:

- Trang mặc định trông giống hệt như project MVC
- Giao diện hiển thị không có sự khác biệt rõ rệt


#### Cấu trúc URL:

- **MVC**: `home/privacy` (controller/action method)
    - `home` là controller
    - `privacy` là action method
- **Razor Pages**: chỉ có `privacy`
    - Không có controllers trong Razor Pages
    - URL trực tiếp trỏ đến trang


### Điểm khác biệt chính

- **MVC**: Sử dụng mô hình Model-View-Controller
- **Razor Pages**: Không có controllers, tổ chức theo từng trang riêng biệt
- Cách tiếp cận Razor Pages phù hợp cho những ứng dụng đơn giản hơn, ít phức tạp về cấu trúc

**Liên kết:** [[MVC Pattern]], [[Razor Pages]], [[ASP.NET Core]], [[CRUD Operations]], [[URL Routing]], [[Project Structure]]

