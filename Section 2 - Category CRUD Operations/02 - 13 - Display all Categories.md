## Hiển thị Danh Sách Categories từ Controller sang View

### Khái niệm cơ bản

Hot reload hiện đã hoạt động trong dự án. Nhiệm vụ tiếp theo là lấy danh sách category từ controller và hiển thị chúng trong view.

**Nhắc lại kiến trúc MVC:**

- **Controller**: Chứa các action methods được gọi
- **View**: Tương ứng với controller, nằm trong thư mục views có cùng tên
- **Model**: Dữ liệu được truyền giữa controller và view


### Cách truyền dữ liệu từ Controller sang View

Để truyền danh sách categories từ controller sang view:

```csharp
// Trong controller action
return View(categoryList); // Truyền object vào trong dấu ngoặc đơn
```


### Định nghĩa Model trong View

Tại đầu view, sử dụng cú pháp `@model` (viết thường) để định nghĩa kiểu dữ liệu:

```csharp
@model List<Category>
```

**Lưu ý quan trọng:**

- Khi định nghĩa: `@model` (viết thường)
- Khi truy cập: `Model` (viết hoa chữ cái đầu - Pascal case)


### Sử dụng C\# Code trong View (Razor Syntax)

Trong MVC application, có thể viết C\# code trực tiếp trong view bằng cách sử dụng ký hiệu `@`:

```html
<!-- HTML cơ bản cho bảng -->
<table>
    <thead>
        <tr>
            <th>Tên Category</th>
            <th>Thứ tự hiển thị</th>
        </tr>
    </thead>
    <tbody>
        @foreach(var obj in Model.OrderBy(u => u.DisplayOrder))
        {
            <tr>
                <td>@obj.Name</td>
                <td>@obj.DisplayOrder</td>
            </tr>
        }
    </tbody>
</table>
```


### Các thành phần quan trọng trong code

- **Foreach loop**: Lặp qua từng object trong Model
- **Truy cập thuộc tính**: Sử dụng `@obj.PropertyName` để hiển thị giá trị
- **Sắp xếp dữ liệu**: Có thể sử dụng `.OrderBy()` để sắp xếp theo thuộc tính cụ thể
- **C\# code trong view**: Cho phép thực hiện các phép tính phức tạp mà không thể làm được trong .NET Framework cũ


### Ví dụ thực tế

Khi có 3 categories trong database, bảng sẽ hiển thị 3 dòng tương ứng với:

- Tên category (`@obj.Name`)
- Thứ tự hiển thị (`@obj.DisplayOrder`)
- Được sắp xếp theo thứ tự tăng dần của DisplayOrder


### Ưu điểm của Razor View Engine

- Tích hợp C\# code trực tiếp trong HTML
- Hỗ trợ các câu lệnh điều kiện (if), vòng lặp (foreach, for)
- Cho phép thực hiện các phép tính và xử lý logic phức tạp
- Cải tiến đáng kể so với .NET Framework truyền thống

**Liên kết:** [[MVC Architecture]], [[Controller Actions]], [[Razor View Engine]], [[Model Binding]], [[Hot Reload]], [[Entity Framework]], [[Category Model]]

