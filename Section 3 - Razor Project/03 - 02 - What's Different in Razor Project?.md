## So sánh dự án MVC và Razor Pages

### Điểm tương đồng giữa hai loại dự án

#### Các file cấu hình chung:

- **appsettings.json**: Hoàn toàn giống nhau trong cả hai dự án
    - Chứa các cài đặt kết nối cơ sở dữ liệu (connection strings)
    - Lưu trữ các cấu hình ứng dụng
- **Program.cs**: Chức năng tương tự nhau nhưng có một số khác biệt:
    - **MVC**: Sử dụng `builder.Services.AddControllersWithView()`
    - **Razor Pages**: Sử dụng `builder.Services.AddRazorPages()`
- **wwwroot folder**: Hoàn toàn giống nhau - chứa các file tĩnh (static files)
- **Properties và Dependencies**: Giữ nguyên như nhau


### Điểm khác biệt chính

#### Cấu trúc thư mục:

- **MVC**: Có thư mục Controllers, Models, Views
- **Razor Pages**: Chỉ có thư mục **Pages**, không có Controllers


#### Hệ thống định tuyến (Routing):

- **MVC**:
    - Sử dụng `controller/action` pattern
    - Route phức tạp hơn với controller và action method
- **Razor Pages**:
    - Sử dụng `MapRazorPages()`
    - Định tuyến đơn giản: cấu trúc thư mục Pages = cấu trúc URL
    - Ví dụ: `/pages/privacy.cshtml` → URL: `/privacy`


### Cấu trúc Razor Pages

#### File .cshtml:

- Vẫn sử dụng cú pháp Razor với dấu `@`
- Có thêm directive `@page` ở đầu file
- Có thể sử dụng C\# code trực tiếp


#### Page Model (.cs file):

- **Không phải** code-behind file truyền thống
- Kế thừa từ `PageModel` class
- Chứa các handler methods:
    - `OnGet()`: Xử lý GET request
    - `OnPost()`: Xử lý POST request
    - Quy tắc đặt tên: `On` + `[HTTP Method]`


### Thư mục Shared

#### Các file chung giống MVC:

- `_Layout.cshtml`: Template chung
- `_ValidationScriptsPartial.cshtml`: Script validation
- `_ViewImports.cshtml`: Import các namespace
- `_ViewStart.cshtml`: Cài đặt khởi tạo
- `Error.cshtml`: Trang hiển thị lỗi


### Cấu hình Entity Framework Core

#### Bài tập thực hành:

- Tạo thư mục `Models` và copy model `Category`
- Cài đặt các package EF Core giống dự án MVC
- Tạo `ApplicationDbContext` trong thư mục `Data`
- Cấu hình Program.cs để sử dụng EF Core
- Seed dữ liệu Category (3 categories)


#### Connection String:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=...;Database=bulky_razor;..."
  }
}
```


### Ghi chú quan trọng

- **Không thực hiện migration ngay**: Đợi video tiếp theo để học về những điểm cần lưu ý
- Razor Pages phù hợp cho các ứng dụng đơn giản hơn MVC
- Cấu trúc định tuyến trực quan và dễ hiểu hơn
- Page Model tập trung tất cả logic xử lý cho một trang cụ thể

**Liên kết:** [[Razor Pages]], [[MVC Pattern]], [[Entity Framework Core]], [[Page Model]], [[Routing]], [[ApplicationDbContext]], [[Program.cs]], [[Dependency Injection]]

