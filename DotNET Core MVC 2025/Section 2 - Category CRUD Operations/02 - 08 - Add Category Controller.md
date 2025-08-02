## Tạo Controller cho các thao tác CRUD trên Category

### Khái niệm CRUD Operations

CRUD là viết tắt của các thao tác cơ bản trên dữ liệu:

- **Create** (Tạo) - tạo category mới
- **Read** (Đọc) - lấy tất cả categories
- **Update** (Cập nhật) - chỉnh sửa category
- **Delete** (Xóa) - xóa category

Để thực hiện các thao tác này, cần tạo một [[Controller]] riêng cho Category vì hiện tại ứng dụng chỉ có Home Controller.

### Tạo Category Controller

#### Các bước tạo Controller

- Trong thư mục `Controllers`, chuột phải → **Add** → **Controller**
- Chọn template: **MVC Controller - Empty** để bắt đầu từ đầu
- Đặt tên: `CategoryController`


#### Quy tắc đặt tên quan trọng

- **Bắt buộc** phải có từ khóa `Controller` ở cuối tên
- Ví dụ: `CategoryController`, `ProductController`
- Đây là cách [[MVC Framework]] nhận diện class là một controller


#### Controller được tạo tự động

```csharp
public class CategoryController : Controller
{
    public IActionResult Index()
    {
        return View();
    }
}
```


### Cách truy cập Controller qua URL

#### Cấu trúc URL trong MVC

```
https://localhost:port/[Controller]/[Action]
```


#### Ví dụ cụ thể

- Controller: `CategoryController`
- Action method: `Index` (phương thức mặc định được tạo)
- URL: `https://localhost:port/Category/Index`


### Xử lý lỗi thiếu View

#### Lỗi thường gặp

Khi truy cập `/Category/Index` lần đầu sẽ gặp lỗi:

```
The view 'Index' was not found
```


#### Nguyên nhân

- [[Action Method]] `Index()` trả về `View()`
- Nhưng chưa có file view tương ứng
- [[MVC Framework]] tìm kiếm view ở các vị trí:
    - `/Views/Category/Index.cshtml`
    - `/Views/Shared/Index.cshtml`


### Tạo View cho Controller

#### Tạo thư mục View

- Trong thư mục `Views`, tạo folder mới
- **Tên folder phải khớp chính xác** với tên Controller: `Category`


#### Tạo file View

- Chuột phải trong folder `Category` → **Add** → **View**
- Chọn **Empty View**
- Đặt tên: `Index.cshtml`


#### Nội dung View cơ bản

```html
<h1>Category List</h1>
```


### Kiểm tra kết quả

#### Chạy ứng dụng

- Truy cập URL: `/Category/Index`
- Kết quả: Hiển thị trang với tiêu đề "Category List"
- Đây sẽ là trang hiển thị tất cả categories từ database


### Cấu trúc thư mục hoàn chỉnh

```
Controllers/
├── HomeController.cs
└── CategoryController.cs

Views/
├── Home/
│   ├── Index.cshtml
│   └── Privacy.cshtml
├── Category/
│   └── Index.cshtml
└── Shared/
    └── _Layout.cshtml
```


### Ghi chú thêm

- Mỗi [[Controller]] cần có folder views riêng trong thư mục `Views`
- Tên folder views phải khớp chính xác với tên controller (không bao gồm từ "Controller")
- [[Action Method]] mặc định `Index` thường được dùng để hiển thị danh sách
- Trang này sẽ được phát triển để hiển thị tất cả categories từ [[Entity Framework Core]]

**Liên kết:** [[Controller]], [[Action Method]], [[MVC Framework]], [[Entity Framework Core]], [[CRUD Operations]]

