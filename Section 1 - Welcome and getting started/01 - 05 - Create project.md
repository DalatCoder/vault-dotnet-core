## Tạo Dự Án ASP.NET Core MVC trong Visual Studio 2022

### Khởi tạo dự án mới

- Mở **Visual Studio 2022**
- Chọn **"Create a new project"** ở phía bên trái
- Tìm kiếm từ khóa **"MVC"** trong thanh tìm kiếm
- Chọn template **"ASP.NET Core Web App"** với mô tả **Model View Controller**


### Lưu ý quan trọng khi chọn template

- **Chọn đúng template**: Đảm bảo chọn template có ghi chú "Model View and Controller"
- **Tránh nhầm lẫn**: Không chọn template "Web app" sử dụng Razor Pages (không có MVC)


### Cấu hình thông tin dự án

- **Tên dự án (Project name)**: `BulkyWeb`
- **Vị trí lưu trữ (Location)**: Chọn thư mục mong muốn
- **Tên solution (Solution name)**: `Bulky`
    - Lý do: Sẽ chứa nhiều project con trong solution này


### Cấu hình framework và tùy chọn

**Framework**:

- Chọn **.NET 8** làm framework chính

**Authentication (Xác thực)**:

- Để **"None"** ở thời điểm hiện tại
- Lý do: Sẽ thêm authentication sau, giữ đơn giản ban đầu

**Các tùy chọn khác**:

- **Configure for HTTPS**: Để mặc định (được tích)
- **Do not use top-level statements**: Không tích chọn
    - Lý do: Tùy chọn này chỉ thêm using statements ở đầu file, không quan trọng lắm


### Tạo dự án và quản lý mã nguồn

**Tạo dự án**:

- Nhấn nút **"Create"** để hoàn tất việc tạo dự án
- Visual Studio sẽ tự động tạo cấu trúc thư mục và file mặc định

**Thêm vào source control**:

- Tên repository: `bulky_MVC`
- Loại repository: **Private** (sẽ chuyển thành public sau khi khóa học ra mắt)
- Nhấn **"Create and Push"** để tạo và đẩy code lên Git repository


### Kết quả

- Dự án ASP.NET Core MVC đã được tạo thành công
- Code đã được commit và push lên Git repository
- Sẵn sàng để khám phá cấu trúc file và thư mục mặc định của dự án MVC


### Ghi chú thêm

- Solution được đặt tên `Bulky` vì sẽ chứa nhiều project con
- Authentication sẽ được thêm vào ở các bài học sau
- Framework .NET 8 cung cấp các tính năng mới nhất cho ứng dụng web

