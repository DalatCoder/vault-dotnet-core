## Layout và Views trong MVC

### Khái niệm Layout (Master Page)

- **[[Layout]]** là trang chủ (master page) của toàn bộ ứng dụng MVC
- File `_Layout.cshtml` nằm trong thư mục `Views/Shared/`
- Chứa cấu trúc HTML cơ bản: doctype, head, body, header, footer
- Tương tự như Master Page trong các ứng dụng .NET cũ


### Cách hoạt động của RenderBody

- `@RenderBody()` là helper tích hợp sẵn trong MVC
- Hiển thị nội dung của view được trả về từ [[Controller]]
- Nội dung view sẽ được render tại vị trí của `@RenderBody()` trong layout
- Ví dụ: Khi controller trả về `Index` view, nội dung của `Index.cshtml` sẽ xuất hiện tại vị trí `@RenderBody()`


### _ViewStart.cshtml - Định nghĩa Layout mặc định

- File `_ViewStart.cshtml` xác định layout nào sẽ được sử dụng làm master page
- Nội dung cơ bản:

```csharp
@{
    Layout = "_Layout";
}
```

- Nếu thay đổi tên layout trong file này, ứng dụng sẽ tìm file layout tương ứng
- Nếu không tìm thấy file layout, ứng dụng sẽ báo lỗi


### _ViewImports.cshtml - Import toàn cục

- Chứa các câu lệnh `using` và import toàn cục cho tất cả views
- Tránh phải viết lại các import trong từng view riêng lẻ
- Các import trong file này chỉ áp dụng cho **views**, không áp dụng cho [[Controller]] hay [[Model]]

Ví dụ nội dung:

```csharp
@using BulkyWeb
@using BulkyWeb.Models
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```


### Partial Views

- **[[Partial View]]** là các view không thể hiển thị độc lập
- Được sử dụng như một phần của view chính
- Thường có tên bắt đầu bằng dấu gạch dưới `_` (ví dụ: `_ValidationScriptsPartial.cshtml`)
- Quy ước đặt tên: Dấu `_` giúp nhận biết đây là component được sử dụng xuyên suốt ứng dụng


### Validation Scripts Partial

- File `_ValidationScriptsPartial.cshtml` chứa JavaScript cho [[Client-side Validation]]
- Không được include trên tất cả trang để tối ưu hiệu suất
- Chỉ được sử dụng trên các trang cần validation
- Sẽ được học chi tiết trong các video sau


### Tag Helpers

- Các thẻ như `asp-controller`, `asp-action` trong anchor tags
- Là [[Tag Helpers]] của ASP.NET Core MVC
- Sẽ được giải thích chi tiết trong các bài học tiếp theo


### Ghi chú thêm

- Tất cả JavaScript và CSS toàn cục nên được thêm vào `_Layout.cshtml`
- File `Error.cshtml` dùng để hiển thị thông báo lỗi khi có exception xảy ra
- Cấu trúc views bao gồm: Views chính, Shared views, và Partial views

