## Thêm Connection String vào dự án .NET Core MVC

### Yêu cầu tiên quyết

- Cài đặt **SQL Server**
- Cài đặt **SQL Server Management Studio**
- Đảm bảo có thể kết nối thành công với SQL Server


### Kiểm tra kết nối SQL Server

Trong SQL Server Management Studio, có thể kết nối với các server khác nhau:

- `localhost` - Server cục bộ với tên localhost
- `localdb\MSSQLLocalDB` - Local database instance
- `.` - Server cục bộ với ký hiệu dấu chấm


### Nơi định nghĩa Connection String

- **Không nên** hardcode connection string trong `program.cs`
- **Nên sử dụng** file `appsettings.json` để lưu trữ connection strings và secrets
- `appsettings.json` là file JSON cơ bản với cấu trúc key-value


### Cấu trúc Connection String trong appsettings.json

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=.;Database=Bulky;Trusted_Connection=true;TrustServerCertificate=true;"
  }
}
```

### Kết nối đến MySQL
```json
{  
  "Logging": {  
    "LogLevel": {  
      "Default": "Information",  
      "Microsoft.AspNetCore": "Warning"  
    }  
  },  
  "AllowedHosts": "*",  
  "ConnectionStrings": {  
    "DefaultConnection": "Server=localhost;Port=3306;Database=Bulky;Uid=root;Pwd=mypassword123;"  
  }  
}
```

### Các thành phần trong Connection String

#### Tên khối (Block name)

- Sử dụng chính xác tên `"ConnectionStrings"` (không có lỗi chính tả)
- Đây là helper method được cung cấp sẵn trong app settings


#### Tên kết nối (Connection name)

- Có thể đặt tên tùy ý, ví dụ: `"DefaultConnection"`
- Đây là key để tham chiếu đến connection string cụ thể


#### Thuộc tính kết nối

- **Server**: Tên server (ví dụ: `.`, `localhost`, `localdb\MSSQLLocalDB`)
- **Database**: Tên cơ sở dữ liệu (ví dụ: `Bulky`)
- **Trusted_Connection=true**: Sử dụng xác thực Windows
- **TrustServerCertificate=true**: Tin tưởng chứng chỉ server


### Ghi chú quan trọng

- Các thuộc tính `Trusted_Connection` và `TrustServerCertificate` là **bắt buộc** với một số cài đặt SQL Server
- Chú ý chính tả: `Trusted_Connection` có dấu gạch dưới, `TrustServerCertificate` không có
- Nếu không thêm các thuộc tính này, kết nối có thể bị lỗi do yêu cầu trust certificate


### Bước tiếp theo

- Sử dụng connection string để kết nối database và tạo bảng thực tế
- Cấu hình [[Entity Framework Core]] để sử dụng connection string này

**Liên kết:** [[Entity Framework Core]], [[SQL Server]], [[Database Configuration]]

