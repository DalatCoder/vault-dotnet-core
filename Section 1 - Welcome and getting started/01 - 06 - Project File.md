## Chạy Ứng Dụng và Khám Phá Project File

### Chạy ứng dụng lần đầu

- Nhấn nút **HTTPS** trong Visual Studio để build và chạy project
- Ứng dụng sẽ tự động mở trong trình duyệt với giao diện mặc định


### Giao diện mặc định của ứng dụng MVC

**Các thành phần có sẵn**:

- **Header (đầu trang)**: Chứa thanh điều hướng với 2 liên kết chính
    - `Home`: Trang chủ hiển thị thông điệp chào mừng
    - `Privacy`: Trang riêng tư với nội dung khác
- **Body (nội dung chính)**: Khu vực hiển thị nội dung trang
- **Footer (chân trang)**: Phần cuối trang web
- **Navigation (điều hướng)**: Hệ thống menu đã được cấu hình sẵn


### Phân biệt Solution và Project

**Solution (giải pháp)**:

- Tên: `Bulky`
- Có thể chứa nhiều project con
- Là container tổng thể cho toàn bộ ứng dụng

**Project (dự án)**:

- Tên: `BulkyWeb`
- Thuộc về solution `Bulky`
- Hiện tại chỉ có 1 project duy nhất


### Khám phá Project File

**Cách truy cập**:

- Chuột phải vào tên project (`BulkyWeb`)
- Chọn **"Edit Project File"**

**Cấu trúc Project File**:

```xml
<PropertyGroup>
  <TargetFramework>net8.0</TargetFramework>
  <Nullable>enable</Nullable>
  <ImplicitUsings>enable</ImplicitUsings>
</PropertyGroup>
```


### Các thuộc tính quan trọng trong Project File

**Target Framework (framework mục tiêu)**:

- `<TargetFramework>net8.0</TargetFramework>`
- Xác định ứng dụng sử dụng .NET 8 framework
- Đây là thuộc tính quan trọng nhất

**Nullable (nullable reference types)**:

- `<Nullable>enable</Nullable>`
- Tính năng được giới thiệu từ .NET 6
- Chi tiết sẽ được giải thích trong các video sau

**Implicit Usings (using statements ngầm định)**:

- `<ImplicitUsings>enable</ImplicitUsings>`
- Tự động bao gồm các using statements mặc định từ thư viện .NET
- **Lợi ích**: Không cần viết thủ công các import statements cơ bản
- **Nếu disable**: Phải explicitly thêm các import statements


### So sánh với phiên bản cũ

- **Phiên bản cũ của .NET Core**: Project file phức tạp hơn nhiều
- **Phiên bản mới**: Được đơn giản hóa đáng kể
- Dễ đọc và dễ quản lý hơn


### Ghi chú về việc mở rộng project

**Khi thêm NuGet packages**:

- Project file sẽ được cập nhật tự động
- Các dependencies sẽ được liệt kê trong file này
- Sẽ được trình bày chi tiết trong các video tiếp theo

**Khi thêm NPM packages**:

- Cũng cần cập nhật trong project file
- Quản lý dependencies front-end


### Tầm quan trọng của Project File

- Chứa tất cả **project properties (thuộc tính dự án)** được .NET thiết lập
- Là nơi quản lý **dependencies** và **packages**
- Xác định **framework version** và **project settings**
- Sẽ trở nên phức tạp hơn khi thêm nhiều tính năng vào project


### Kế hoạch tiếp theo

- Khám phá chi tiết các files và folders trong project
- Tìm hiểu về Nullable reference types
- Học cách thêm và quản lý NuGet packages
- Xem project file thay đổi như thế nào khi mở rộng tính năng

