## Tạo Razor Pages cho chức năng Category

### Khái niệm cơ bản

- Mục tiêu: Tái tạo các chức năng tương tự như dự án MVC nhưng sử dụng **Razor Pages** thay vì Controllers
- Trong MVC có Controllers, nhưng trong Razor Pages chỉ có **Pages**
- Cần tổ chức lại cấu trúc để phù hợp với Razor Pages


### Tạo cấu trúc thư mục và page

#### Tạo thư mục Categories:

- Trong thư mục `Pages`, tạo thư mục mới tên `Categories`
- **Lưu ý đặt tên**: Nên đặt tên thư mục là `Categories` (số nhiều) thay vì `Category` để tránh xung đột với Model `Category`
- Cách này giúp tránh phải viết đầy đủ using statements nhiều lần


#### Tạo Razor Page mới:

- Thêm **Empty Razor Page** trong thư mục Categories
- Đặt tên: `Index.cshtml`
- Hệ thống tự động tạo 2 files:
    - `Index.cshtml`: Razor page (tương đương View trong MVC)
    - `Index.cshtml.cs`: Page Model (tương đương Controller trong MVC)


### Thuật ngữ quan trọng

#### So sánh MVC vs Razor Pages:

- **MVC**: View + Controller
- **Razor Pages**: Razor Page + Page Model


#### Cấu trúc Razor Page:

- Có directive `@page` ở đầu file (định nghĩa đây là razor page)
- Có `@model` được định nghĩa sẵn, trỏ đến Page Model file (.cs)
- Không cần định nghĩa model như trong MVC


### Nội dung trang đơn giản

```html
<h1>Category List</h1>
```

- Bỏ các directive không cần thiết
- Chỉ cần nội dung HTML cơ bản


### Hệ thống Routing trong Razor Pages

#### Nguyên tắc routing:

- **Rất đơn giản**: Đường dẫn file trong thư mục Pages = Route URL
- Ví dụ: `/Pages/Categories/Index.cshtml` → URL: `/categories/index`


#### Tính năng đặc biệt:

- `index` luôn là **tùy chọn** trong URL
- `/categories/index` và `/categories` đều dẫn đến cùng một trang
- Không có routing phức tạp như MVC (controller/action/parameters)


### Chạy và kiểm tra

#### Cấu hình project:

- Đặt dự án Razor Pages làm **startup project**
- Chạy ứng dụng


#### Truy cập trang:

- URL: `/categories/index` hoặc `/categories`
- Hiển thị: "Category List"


### Ưu điểm của Razor Pages Routing

- **Trực quan**: Cấu trúc thư mục = cấu trúc URL
- **Đơn giản**: Không cần cấu hình route phức tạp
- **Dễ hiểu**: Không có khái niệm controller/action method
- **Straightforward**: Routing thẳng theo đường dẫn file


### Ghi chú thêm

- Trong suốt phần này sẽ sử dụng dự án Razor Pages làm startup project
- Terminology cần nhớ:
    - **MVC**: View (giao diện)
    - **Razor Pages**: Razor Page (giao diện) + Page Model (xử lý logic)
- Cấu trúc này phù hợp cho các ứng dụng đơn giản hơn MVC

**Liên kết:** [[Razor Pages]], [[Page Model]], [[MVC Pattern]], [[Routing]], [[Empty Razor Page]], [[Categories]], [[Index Page]], [[Startup Project]], [[Pages Folder]], [[URL Routing]]

