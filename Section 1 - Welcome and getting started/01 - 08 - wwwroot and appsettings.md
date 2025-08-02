## Cấu trúc dự án .NET MVC - wwwroot và App Settings

### wwwroot Folder - Thư mục quan trọng

#### Khái niệm

- Là thư mục quan trọng trong ứng dụng .NET Core
- Chứa tất cả **nội dung tĩnh (static content)** của dự án


#### Static Content là gì?

**Nội dung tĩnh** bao gồm tất cả những file không chứa mã HTML:

- CSS files
- JavaScript files
- NuGet packages hoặc thư viện bên thứ ba (third party libraries)
- Hình ảnh (images)
- Files, PDFs, PowerPoint
- Bất kỳ nội dung tĩnh nào khác


### Cấu trúc wwwroot mặc định

#### CSS

- `global.site.css` - file CSS toàn cục đã được sử dụng trong ứng dụng
- Có thể thêm styling tùy chỉnh vào file này


#### JavaScript

- `site.js` - file JavaScript mặc định (hiện tại chỉ có template cơ bản)


#### Thư mục lib

Chứa các thư viện được bao gồm mặc định khi tạo ứng dụng MVC:

- **Bootstrap** - framework CSS
- **jQuery** - thư viện JavaScript
- **jQuery Validations** - thư viện validation


### Quy tắc quan trọng

> **Luôn nhớ**: Tất cả nội dung tĩnh (hình ảnh, files, thư viện) trong tương lai đều phải được thêm vào **wwwroot folder**

### Controllers, Models và Views

- Sẽ được đề cập chi tiết trong các video tiếp theo


### App Settings - Cấu hình ứng dụng

#### Appsettings.json

- **Mục đích chính**: Lưu trữ tất cả **connection strings** và **secret keys** của ứng dụng


#### Các loại thông tin lưu trữ

- **Connection strings** cho database
- **Secret keys** cho email (ví dụ: SendGrid)
- **Azure Blob Storage** connection strings
- **Azure Storage Account** credentials
- Bất kỳ secret key nào khác của ứng dụng


#### Lợi ích tập trung hóa

- Khi cần thay đổi connection, chỉ cần vào `Appsettings.json`
- Không cần tìm kiếm khắp code base
- Tất cả connection và secret keys được quản lý ở một nơi


### Environment-based Configuration

#### Cấu trúc files

- `appsettings.json` - cấu hình chung
- `appsettings.development.json` - cấu hình cho môi trường phát triển
- `appsettings.production.json` - cấu hình cho môi trường production


#### Cách hoạt động

- Dựa trên biến `ASPNETCORE_ENVIRONMENT` trong [[Launch Settings]]
- .NET Core tự động chọn file cấu hình phù hợp:
    - **Development environment** → sử dụng `appsettings.development.json`
    - **Production environment** → sử dụng `appsettings.production.json`


#### Ví dụ tạo file production

```json
// appsettings.production.json
{
  "ConnectionStrings": {
    "DefaultConnection": "production-connection-string"
  }
}
```


### Ghi chú thêm

- Tính năng environment-based configuration rất thông minh và linh hoạt
- Chi tiết cụ thể sẽ được đề cập trong các video sau
- File `Program.cs` sẽ được phân tích trong video tiếp theo (là file quan trọng)


### Nguyên tắc cơ bản cần nhớ

> Tất cả **secrets** và **connection strings** phải được lưu trong file `appsettings.json`

