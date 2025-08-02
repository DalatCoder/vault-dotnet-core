## Khám Phá Cấu Trúc Project: Dependencies và Launch Settings

### Connected Services

- Mục **Connected Services** hiện tại trống
- Sẽ được sử dụng khi kết nối với các dịch vụ bên ngoài
- Có thể bỏ qua ở thời điểm này


### Dependencies (Phụ thuộc)

**Khái niệm**:

- Chứa tất cả các **packages** và **project references** mà ứng dụng phụ thuộc vào
- Hiện tại chưa có dependency nào

**Các loại dependencies thường gặp**:

- **NuGet packages**: Để truy cập database, tích hợp thanh toán, v.v.
- **Project references**: Khi có nhiều project trong solution
- Sẽ tự động cập nhật khi thêm packages hoặc project references

**Ví dụ sử dụng**:

- Truy cập database → Thêm Entity Framework packages
- Tích hợp thanh toán → Thêm payment gateway packages
- Multi-project solution → Thêm project references


### Properties Folder và Launch Settings

### Launch Settings.json

**Mục đích**:

- Định nghĩa cài đặt khi **chạy** hoặc **debug** ứng dụng
- Kiểm soát cách ứng dụng khởi động trong môi trường phát triển

**Cấu trúc chính**:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:port",
      "sslPort": "https_port_number"
    }
  },
  "profiles": {
    "http": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "applicationUrl": "http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "https": {
      "commandName": "Project", 
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "applicationUrl": "https://localhost:7169;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```


### Profiles (Cấu hình chạy)

**Các profile mặc định**:

- **HTTP profile**: Chạy ứng dụng qua HTTP
- **HTTPS profile**: Chạy ứng dụng qua HTTPS (đang sử dụng)
- **IIS Express profile**: Chạy qua IIS Express

**Cách chuyển đổi profiles**:

- Sử dụng dropdown menu bên cạnh nút chạy
- Chọn profile mong muốn trước khi nhấn run


### Environment Variables (Biến môi trường)

**Khái niệm**:

- Giống như **global variables** có thể sử dụng trong toàn bộ ứng dụng
- Giúp phân biệt giữa các môi trường khác nhau

**Ví dụ sử dụng `ASPNETCORE_ENVIRONMENT`**:

**Development environment**:

- Sử dụng development database
- Dùng test payment keys (không xử lý thẻ tín dụng thật)
- Hiển thị error details đầy đủ

**Production environment**:

- Sử dụng production database
- Dùng production payment keys (xử lý thanh toán thật)
- Ẩn error details nhạy cảm


### Thay Đổi Port Ứng Dụng

**Cách thực hiện**:

1. Mở `launchSettings.json`
2. Tìm đến profile đang sử dụng (ví dụ: `https`)
3. Sửa `applicationUrl` từ `7169` thành `7001`
4. Lưu file và chạy lại ứng dụng

**Ví dụ**:

```json
"applicationUrl": "https://localhost:7001;http://localhost:5000"
```

**Kết quả**: Ứng dụng sẽ chạy trên port 7001 thay vì 7169

### Hoàn tác thay đổi với Git

**Cách thực hiện**:

- Chuột phải vào file đã sửa đổi
- Chọn **"Undo Git changes"**
- File sẽ trở về trạng thái ban đầu


### Ghi chú quan trọng

**Khi nào cần sửa launch settings**:

- Thay đổi port mặc định
- Cấu hình environment variables
- Thêm profile mới cho môi trường đặc biệt
- Tùy chỉnh cách ứng dụng khởi động

**Thông thường**:

- Không cần sửa đổi nhiều
- Cấu hình mặc định đã đủ cho hầu hết trường hợp
- Chỉ điều chỉnh khi có yêu cầu cụ thể


### Kết luận

- **Launch Settings.json** là file cấu hình quan trọng cho quá trình development
- **Environment Variables** giúp phân biệt các môi trường khác nhau
- **Profiles** cho phép chạy ứng dụng với các cài đặt khác nhau
- Có thể dễ dàng thay đổi port và các cài đặt khác theo nhu cầu

