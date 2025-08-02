## Thêm Dropdown Category cho Product Form

### Vấn đề hiện tại

Khi tạo hoặc chỉnh sửa Product, người dùng không thể chọn Category cho sản phẩm vì chưa có dropdown để lựa chọn. Cần thêm chức năng này để hoàn thiện form Product.

### Yêu cầu thực hiện

**Bước 1: Thêm dropdown Category**

- Cần có danh sách tất cả Categories để hiển thị trong dropdown
- Phải lấy dữ liệu này từ Controller trước khi gửi lên View

**Bước 2: Lấy danh sách Categories trong Controller**

- Truy xuất tất cả Categories từ database
- Chuyển đổi sang định dạng phù hợp cho dropdown (SelectListItem)


### Cách thực hiện trong Controller

**Khai báo IEnumerable<SelectListItem>:**

```csharp
IEnumerable<SelectListItem> categoryList = _unitOfWork.Category.GetAll()
    .Select(u => new SelectListItem
    {
        Text = u.Name,
        Value = u.Id.ToString()
    });
```

**Giải thích từng bước:**

- `IEnumerable<SelectListItem>`: Kiểu dữ liệu chuẩn cho dropdown trong ASP.NET Core MVC
- Namespace: `Microsoft.AspNetCore.Mvc.Rendering`
- `SelectListItem` có 2 thuộc tính chính:
    - **Text**: Nội dung hiển thị trong dropdown (tên Category)
    - **Value**: Giá trị được gửi khi submit form (ID của Category)


### Sử dụng Projection trong Entity Framework Core

**Khái niệm Projection:**

- Tính năng mạnh mẽ của EF Core cho phép chuyển đổi dữ liệu trực tiếp khi truy vấn
- Sử dụng phương thức `.Select()` để biến đổi từ một object type sang object type khác
- Chỉ lấy những cột cần thiết thay vì lấy toàn bộ dữ liệu

**Cú pháp Projection:**

```csharp
_unitOfWork.Category.GetAll()
    .Select(u => new SelectListItem
    {
        Text = u.Name,           // Lấy tên Category làm text hiển thị
        Value = u.Id.ToString()  // Convert ID thành string làm value
    });
```

**Lợi ích của Projection:**

- Hiệu suất cao: Chỉ lấy dữ liệu cần thiết từ database
- Chuyển đổi trực tiếp: Không cần thêm bước mapping thủ công
- Code gọn gàng: Thực hiện conversion trong một câu lệnh duy nhất


### Thách thức tiếp theo

**Vấn đề truyền dữ liệu lên View:**

- Controller chỉ có thể return một object duy nhất
- Hiện tại đang return `List<Product>`, cần thêm `categoryList`
- Cần tìm cách truyền cả Product data và Category list lên View cùng lúc

**Giải pháp sẽ được trình bày:**

- Sử dụng ViewBag hoặc ViewData
- Tạo ViewModel chứa cả Product và Category list
- Các phương pháp khác để truyền multiple data types


### Namespace quan trọng

```csharp
using Microsoft.AspNetCore.Mvc.Rendering;  // Cho SelectListItem
using System.Collections.Generic;          // Cho IEnumerable
```


### Ghi chú thêm

- **SelectListItem** là class chuẩn của ASP.NET Core để tạo dropdown options
- **Projection** giúp tối ưu hóa truy vấn database bằng cách chỉ lấy dữ liệu cần thiết
- Phương pháp này scalable và hiệu quả cho các dropdown có nhiều dữ liệu
- Cần convert `Id` thành `string` vì `Value` property của SelectListItem yêu cầu kiểu string
- Bước tiếp theo sẽ giải quyết vấn đề truyền multiple objects từ Controller lên View

**Liên kết:** [[Product Controller]], [[Category Model]], [[SelectListItem]], [[Entity Framework Core]], [[Projection]], [[Dropdown]], [[ASP.NET Core MVC]], [[IEnumerable]], [[Controller Action]], [[ViewBag]], [[ViewModel]]

