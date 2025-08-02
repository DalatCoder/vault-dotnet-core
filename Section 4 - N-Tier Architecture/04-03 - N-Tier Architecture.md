## Di chuyển Logic từ Dự án Monolithic sang Multi-Project Architecture

### Di chuyển các thành phần chính

#### 1. Di chuyển Data folder sang DataAccess Project

- **Mục đích**: DataAccess project chịu trách nhiệm cho tất cả các thao tác liên quan đến dữ liệu
- **Thao tác**:
    - Cut folder `Data` từ BulkyWeb project
    - Paste vào `Bulky.DataAccess` project
    - Xóa folder `Data` cũ trong BulkyWeb


#### 2. Di chuyển Models folder sang Models Project

- **Thao tác**:
    - Cut folder `Models` từ BulkyWeb
    - Paste vào `Bulky.Models` project
    - Xóa folder `Models` cũ trong BulkyWeb


#### 3. Tạo Utility Class cho Constants

- **Tạo class mới** trong `Bulky.Utility`:

```csharp
public static class SD // SD = Static Details
{
    // Chứa tất cả constants cho website
}
```


#### 4. Di chuyển Migrations

- **Di chuyển folder** `Migrations` từ BulkyWeb sang `Bulky.DataAccess`
- **Lý do**: Migrations liên quan trực tiếp đến database operations


### Cài đặt NuGet Packages cho DataAccess

#### Packages cần thiết

- **Entity Framework Core**: Core functionality
- **Entity Framework Core Tools**: For migrations
- **Entity Framework Core SqlServer**: SQL Server provider


#### Cách cài đặt

- Right-click project → **Manage NuGet Packages**
- Hoặc chọn **Manage NuGet Packages for Solution** để xem toàn bộ solution
- Install các packages cần thiết cho `Bulky.DataAccess`


### Cập nhật Namespaces

#### Thay đổi namespace trong các file

- **ApplicationDbContext**: Từ `BulkyWeb` → `Bulky.DataAccess.Data`
- **Models**: Từ `BulkyWeb.Models` → `Bulky.Models`
- **Migrations**: Từ `BulkyWeb.Migrations` → `Bulky.DataAccess.Migrations`


#### Lưu ý quan trọng về Migration Files

- **Vấn đề**: Migration có 2 files - `.cs` file và `.Designer.cs` file
- **Giải pháp**: Phải cập nhật namespace trong **cả hai files**
- **Designer file** thường bị quên → gây lỗi build


### Thêm Project References

#### Cấu hình references cho BulkyWeb

```
BulkyWeb project cần reference:
├── Bulky.DataAccess
├── Bulky.Models  
└── Bulky.Utility
```


#### Cấu hình references cho DataAccess

```
Bulky.DataAccess project cần reference:
└── Bulky.Models (để sử dụng entity models)
```


### Sử dụng Global Using Statements

#### Thêm vào _ViewImports.cshtml

```csharp
@using Bulky.Models
```


#### Lợi ích

- **Không cần** viết `Bulky.Models.ErrorViewModel` trong mọi view
- Chỉ cần viết `ErrorViewModel`
- **Global scope**: Áp dụng cho tất cả Views trong project


### Xử lý lỗi phổ biến

#### 1. Migration Designer File Errors

- **Nguyên nhân**: Namespace chỉ được update trong `.cs` file, không update trong `.Designer.cs`
- **Giải pháp**: Update namespace trong cả hai files


#### 2. Missing NuGet Packages

- **Triệu chứng**: `ApplicationDbContext` báo lỗi khi được moved
- **Giải pháp**: Install Entity Framework packages trong DataAccess project


#### 3. Project Reference Issues

- **Triệu chứng**: Không thể access models từ các project khác
- **Giải pháp**: Add project references thông qua Right-click → Add Project Reference


### Kiểm tra và Build Solution

#### Thứ tự build để kiểm tra

1. **Bulky.Models** - Build đầu tiên (không dependency)
2. **Bulky.DataAccess** - Build sau (depends on Models)
3. **Bulky.Utility** - Build độc lập
4. **Bulky.Web** - Build cuối cùng (depends on tất cả)

#### Chạy thử nghiệm

- Chạy application để đảm bảo tất cả pages hoạt động bình thường
- Kiểm tra Create, Edit và các chức năng khác


### Lợi ích của Multi-Project Architecture

#### Clean Architecture

- **Separation of Concerns**: Mỗi project có trách nhiệm riêng biệt
- **Maintainability**: Dễ dàng bảo trì và phát triển
- **Scalability**: Dễ mở rộng khi dự án phát triển


#### Best Practices

- **Tách biệt logic**: Database, Models, Utilities riêng biệt
- **Reusability**: Các class library có thể tái sử dụng
- **Testing**: Dễ dàng unit testing từng component


### Ghi chú thêm

- **Phương pháp này** phổ biến trong thực tế: bắt đầu đơn giản, sau đó tách biệt khi complexity tăng
- **Practice value**: Giúp lập trình viên học cách refactor monolithic applications
- **Production ready**: Cấu trúc này phù hợp cho các dự án thực tế

**Liên kết:** [[Multi-Project Architecture]], [[Clean Architecture]], [[Entity Framework Core]], [[Migration]], [[NuGet Packages]], [[Project References]], [[Global Using Statements]], [[Namespace Management]], [[Separation of Concerns]], [[DataAccess Layer]]

