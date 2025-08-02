## Tái cấu trúc dự án ASP.NET Core MVC với Class Library Projects

### Xóa dự án không cần thiết

- Xóa dự án Razor Pages đã tạo trước đó vì chỉ phục vụ mục đích học tập
- Dự án này không cần thiết cho dự án chính Bulky


### Lý do tách biệt dự án

- Trong các ứng dụng thực tế (real-world applications), không nên gộp tất cả code vào một nơi
- Hiện tại tất cả đều nằm trong một project: data, models, constants
- **Tách biệt giúp**: dễ quản lý, bảo trì và phát triển code


### Tạo các Class Library Projects

#### 1. Bulky.DataAccess

- **Mục đích**: Chứa tất cả logic liên quan đến cơ sở dữ liệu (database)
- **Nội dung sẽ chứa**:
    - DB Context
    - Migrations (di chuyển cơ sở dữ liệu)
    - Các repository patterns


#### 2. Bulky.Models

- **Mục đích**: Chứa tất cả các model classes
- Tách biệt các entity models khỏi dự án chính


#### 3. Bulky.Utility

- **Mục đích**: Chứa các tiện ích và hằng số cho ứng dụng
- **Nội dung sẽ chứa**:
    - Email functionality (chức năng email)
    - Application constants (hằng số ứng dụng)
    - Các helper methods khác


### Cấu trúc dự án sau khi tái cấu trúc

```
Solution
├── Bulky.Web (MVC Project chính)
├── Bulky.DataAccess (Class Library)
├── Bulky.Models (Class Library)  
└── Bulky.Utility (Class Library)
```


### Bước tiếp theo

- Xóa các class files mặc định trong các class library projects
- Di chuyển logic từ dự án Bulky.Web sang các class library tương ứng
- Thiết lập dependencies (phụ thuộc) giữa các projects


### Ghi chú thêm

- Đây là pattern phổ biến trong **Clean Architecture** và **N-tier Architecture**
- Giúp tuân thủ nguyên tắc **Separation of Concerns** (tách biệt trách nhiệm)
- Dễ dàng testing và maintain code khi dự án phát triển

**Liên kết:** [[Class Library]], [[Clean Architecture]], [[Separation of Concerns]], [[ASP.NET Core MVC]], [[Project Structure]], [[DataAccess Layer]], [[Models Layer]], [[Utility Layer]]
