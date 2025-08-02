## Xử lý lỗi khi Scaffolding Identity và Quản lý ApplicationDbContext

### Vấn đề phát sinh khi Scaffolding Identity

- Khi tiến hành *scaffold* tính năng Identity, thường xuyên xuất hiện lỗi, đặc biệt là lỗi liên quan đến việc xử lý `DbContext`.
- Sau khi scaffold, nếu chạy ứng dụng có thể xuất hiện exception lạ liên quan đến *unit of work*.


### Nguyên nhân

- Khi scaffold, hệ thống tự động tạo thêm một file `ApplicationDbContext` mới trong thư mục *Areas/Identity* (không cần thiết), thay vì sử dụng `ApplicationDbContext` đã cấu hình sẵn của dự án.
- Cả hai file `ApplicationDbContext` (cả cũ và mới) đều kế thừa từ `IdentityDbContext` và đều có phương thức `base.OnModelCreating(modelBuilder)`.


### Giải pháp xử lý

- Chỉ giữ lại một file `ApplicationDbContext` chuẩn đã cấu hình trong dự án (thường ở dự án Data layer hoặc vị trí gốc).
- Xóa file `ApplicationDbContext` mới được scaffold trong thư mục *Areas/Identity*.
- Việc định nghĩa `IdentityDbContext` bên trong `ApplicationDbContext` là đủ, không cần thêm gì khác, trừ trường hợp bạn muốn tuỳ biến loại user mặc định (cái này là tuỳ chọn, không bắt buộc).
- Sau khi xóa file dư thừa, build lại dự án để kiểm tra, hệ thống sẽ hoạt động bình thường.


### Hướng dẫn thao tác

- Tìm kiếm từ khóa `ApplicationDbContext` trong thư mục *Areas/Identity*.
- Xóa file không cần thiết (thường là file scaffold mới sinh ra).
- Build lại project (`Ctrl+Shift+B` trong Visual Studio).
- Chạy lại ứng dụng để xác nhận mọi thứ đã ổn định và tính năng Identity hoạt động đúng.


### Ghi chú thêm

- Đây là vấn đề phổ biến mỗi khi scaffold Identity trong các dự án .NET, nên cần lưu ý để tránh lỗi lặp lại.
- Không bắt buộc phải định nghĩa user mặc định khi kế thừa `IdentityDbContext`, tính năng này chỉ dùng khi muốn mở rộng, tuỳ biến.


### Liên kết:

[[Identity]], [[ApplicationDbContext]], [[IdentityDbContext]], [[OnModelCreating]], [[Scaffolding]], [[Authentication]], [[Areas]], [[DbContext]]

