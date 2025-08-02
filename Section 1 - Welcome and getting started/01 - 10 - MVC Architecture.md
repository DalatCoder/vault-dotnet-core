## Kiến trúc MVC (Model-View-Controller) trong .NET Core

### Cấu trúc thư mục trong Solution Explorer

Khi mở **Solution Explorer**, bạn sẽ thấy 3 thư mục chính:

- **Controllers**: chứa các controller
- **Models**: chứa các model
- **Views**: chứa các view

→ Đây chính là **3 thành phần cốt lõi** của [[MVC Architecture]]

### Định nghĩa MVC Architecture

**MVC** = **Model-View-Controller**

#### 1. Model - Đại diện cho dữ liệu (Data Shape)

- **Vai trò**: định nghĩa cấu trúc và hình dạng của dữ liệu
- **Nội dung**: chứa các class, table trong database
- **Ví dụ** (ứng dụng e-commerce):
    - Products (sản phẩm)
    - Orders (đơn hàng)
    - Order Details (chi tiết đơn hàng)
    - Shopping Cart (giỏ hàng)
- **Tóm tắt**: Model = tất cả các class files chứa dữ liệu


#### 2. View - Giao diện người dùng (User Interface)

- **Vai trò**: hiển thị những gì người dùng nhìn thấy trên màn hình
- **Nội dung**:
    - Form nhập liệu
    - Biểu đồ (charts)
    - Bảng dữ liệu
    - Tất cả HTML elements
- **Tóm tắt**: View = giao diện HTML của ứng dụng web


#### 3. Controller - Trái tim của ứng dụng (Application Heart)

- **Vai trò**: xử lý request từ người dùng
- **Chức năng**: làm cầu nối giữa [[Model]] và [[View]]
- **Nhiệm vụ**:
    - Xử lý logic nghiệp vụ
    - Fetch dữ liệu từ database
    - Xử lý và chuyển đổi dữ liệu
    - Truyền dữ liệu cho View


### Luồng xử lý Request trong MVC

```
User Request → Controller → Model → Controller → View → Controller → User Response
```

**Các bước chi tiết**:

1. **User gửi request** (click button, mở website)
2. **Controller nhận request** và xác định cần xử lý gì
3. **Controller fetch dữ liệu** từ Model hoặc database
4. **Controller xử lý dữ liệu** (chuyển đổi, tính toán nếu cần)
5. **Controller truyền dữ liệu** cho View component
6. **View tạo HTML** với dữ liệu đã nhận (ví dụ: điền dữ liệu vào table)
7. **View trả HTML hoàn chỉnh** về Controller
8. **Controller gửi response** cuối cùng cho User
9. **User thấy website** hoàn chỉnh trên màn hình

### Khái niệm Action Methods

- **Action Methods**: các phương thức trong Controller định nghĩa các endpoints
- Mỗi Action Method xử lý một loại request cụ thể
- Sẽ được giải thích chi tiết trong bài học tiếp theo


### Liên kết với Default Route

Nhắc lại từ [[Program.cs]]:

```csharp
pattern: "{controller=Home}/{action=Index}/{id?}"
```

- **Controller mặc định**: `Home`
- **Action mặc định**: `Index`
- **ID**: tham số tùy chọn

→ Nếu không có route cụ thể, request sẽ đi đến `HomeController.Index()`

### Ghi chú quan trọng

- **Controller là trái tim** của MVC application
- MVC architecture có thể phức tạp với người mới bắt đầu
- **Nguyên tắc cốt lõi**: Controller điều phối tất cả - nhận request, xử lý dữ liệu, tạo response
- **Luồng dữ liệu**: User → Controller → Model → Controller → View → Controller → User

---
**Liên kết**: [[Program.cs]], [[Routing]], [[Action Methods]], [[Controllers]], [[Models]], [[Views]]

