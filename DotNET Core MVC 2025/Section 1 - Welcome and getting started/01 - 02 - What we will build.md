## Demo ứng dụng thương mại điện tử (E-commerce) cuối khóa

### Tính năng dành cho khách hàng (Customer)

#### Trang chủ và sản phẩm

- **Hiển thị sản phẩm** với hình ảnh và giá cả
- **Số lượng giỏ hàng** hiển thị trên header
- **Chức năng chi tiết sản phẩm** với pricing theo số lượng


#### Hệ thống giá theo số lượng

- **Giá giảm dần** theo số lượng đặt hàng
- Ví dụ: đặt 51 sản phẩm → giá 85/sản phẩm
- **Tính toán tự động** khi thay đổi số lượng


#### Xác thực và đăng ký

- **Bắt buộc đăng nhập** khi thêm vào giỏ hàng
- **Đăng ký tài khoản mới** với validation tích hợp
- **Đăng nhập bằng Facebook** - tích hợp OAuth
- **Thông báo toaster** xác nhận thành công


#### Giỏ hàng (Shopping Cart)

- **Hiển thị tổng tiền** (grand total)
- **Xóa sản phẩm** khỏi giỏ hàng
- **Thay đổi số lượng** trực tiếp trong giỏ
- **Tính toán tự động** khi cập nhật


#### Quy trình thanh toán

1. **Trang tóm tắt** (Summary) - hiển thị thông tin user
2. **Nhập thông tin giao hàng** (shipping details)
3. **Chuyển hướng đến Stripe** để thanh toán
4. **Thẻ test:** 4242 4242 4242 4242 (test mode)
5. **Xác nhận đặt hàng** với Order ID

#### Quản lý đơn hàng của khách

- **Xem đơn hàng** đã đặt
- **Trạng thái đơn hàng** (order status)
- **Thông tin vận chuyển** (carrier \& tracking) khi đã ship
- **Trạng thái "Approved"** sau khi thanh toán


### Tính năng dành cho quản trị viên (Admin)

#### Content Management

- **Menu đặc biệt** chỉ admin mới thấy
- **Quản lý danh mục** (Categories): tạo, sửa, xóa
- **Quản lý sản phẩm** (Products): CRUD đầy đủ


#### Quản lý sản phẩm nâng cao

- **DataTable với tìm kiếm** và lọc dữ liệu
- **Sắp xếp** (sorting) linh hoạt
- **Rich Text Editor** cho mô tả sản phẩm
- **Upload hình ảnh** và thay thế
- **Thiết lập giá theo tier:**
    - Giá niêm yết (list price)
    - Giá 1-50 sản phẩm
    - Giá >50 sản phẩm
    - Giá >100 sản phẩm
- **Dropdown chọn danh mục**
- **Sweet Alert** xác nhận khi xóa


#### Quản lý người dùng

- **Tạo tài khoản** cho user
- **Phân quyền 4 loại user:**
    - Admin
    - Employee (nhân viên)
    - Company (công ty)
    - Customer (khách hàng)


#### Xử lý đơn hàng

- **Xem tất cả đơn hàng** trong hệ thống
- **Theo dõi trạng thái thanh toán:**
    - Payment Intent ID (đã thanh toán)
    - Payment Status: Approved
- **Quy trình xử lý đơn hàng:**

1. Approved → Start Processing
2. Processing → Ship Order
3. Nhập carrier \& tracking info
4. Shipped (không thể cancel)
- **Cập nhật thông tin đơn hàng**


### Tính năng đặc biệt cho tài khoản công ty (Company)

#### Quy trình đăng ký

1. **Admin tạo Company** trong tab Companies
2. **Tạo user account** và chọn company
3. **Dropdown chọn công ty** khi đăng ký

#### Thanh toán trễ (Delayed Payment)

- **Đặt hàng không cần thanh toán** ngay
- **Thanh toán sau 30 ngày** kể từ khi ship
- **Trạng thái:** "Approved for Delayed Payment"
- **Nút "Pay Now"** xuất hiện sau khi ship


#### Quy trình thanh toán công ty

1. Admin ship đơn hàng
2. **Nút Pay Now** được kích hoạt
3. Company user có thể **tự thanh toán**
4. Hoặc **admin thu tiền** thay mặt
5. **Chuyển hướng Stripe** để hoàn tất

### Luồng công việc tổng thể

#### Khách hàng thường (Customer)

```
Duyệt sản phẩm → Thêm vào giỏ → Đăng nhập → 
Thanh toán ngay → Xác nhận đơn hàng
```


#### Công ty (Company)

```
Duyệt sản phẩm → Thêm vào giỏ → Đăng nhập → 
Đặt hàng không thanh toán → Thanh toán sau khi ship
```


#### Admin

```
Quản lý sản phẩm/danh mục → Tạo user/company → 
Xử lý đơn hàng → Thu tiền (nếu cần)
```


### Ghi chú về khóa học

#### Độ phức tạp

- **Video dài** cho thấy **nhiều tính năng** sẽ được đề cập
- **Khóa học dài** nhưng **không cần lo sợ**
- **Bao phủ nhiều chủ đề** trong .NET Core


#### Kết quả mong đợi

- **Tự hào** về ứng dụng đã xây dựng
- **Nắm vững** nhiều khía cạnh của .NET Core MVC
- **Ứng dụng thực tế** với độ phức tạp cao

*Liên kết: [[E-commerce Development]], [[User Authentication]], [[Payment Integration]], [[Admin Panel]], [[Role-based Authorization]], [[Stripe Integration]]*

