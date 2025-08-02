## Reset và Làm mới Migrations trong Entity Framework Core

### Vấn đề phổ biến với Migrations

- **Migration corruption** là vấn đề thường gặp khi mới bắt đầu với Entity Framework Core
- Migrations rất **dễ bị hỏng** (corrupted) nếu không chú ý
- Mặc dù có thể roll back hoặc tạo migration mới, đôi khi cần **bắt đầu hoàn toàn từ đầu**


### Cách Reset Database và Migrations hoàn toàn

#### Bước 1: Xóa Database

- Mở **SQL Server Object Explorer**
- Tìm database cần reset (ví dụ: `Bulky`)
- **Right-click và Delete** database


#### Bước 2: Xóa Migrations Folder

- Vào project `DataAccess`
- Tìm folder **Migrations**
- **Xóa hoàn toàn** folder Migrations
- Lúc này không còn migrations và database nào


#### Bước 3: Tạo Migration mới

- Mở **Tools → Package Manager Console**
- **Quan trọng**: Cập nhật Default Project từ `BulkyWeb` sang `DataAccess`
- **Startup Project**: Vẫn giữ là `BulkyWeb`


#### Bước 4: Chạy Add-Migration Command

```powershell
Add-Migration AddCategoryToDbAndSeedTable
```

- Migration này sẽ **nhận diện tất cả thay đổi** cần thiết:
    - Tạo table Categories
    - Insert seed data
    - Tất cả models hiện có trong project


#### Bước 5: Cập nhật Database

```powershell
Update-Database
```

- Lệnh này sẽ:
    - Tạo database mới
    - Tạo tables
    - Populate seed data


### Kiểm tra kết quả

- **Refresh SQL Server Object Explorer**
- Database `Bulky` đã được tạo lại
- Table `Categories` đã có sẵn với seed data


### Lưu ý quan trọng

#### Cấu hình Package Manager Console

- **Default Project**: Phải là `DataAccess` (nơi chứa DbContext)
- **Startup Project**: Phải là `BulkyWeb` (project chính có connection string)


#### Nhược điểm của phương pháp này

- **Mất toàn bộ dữ liệu** đã thêm thủ công trước đó
- Chỉ còn lại **seed data** được định nghĩa trong code
- Phù hợp cho **môi trường học tập và development**


### Khi nào sử dụng phương pháp này

- **Migrations bị corrupt** và không thể sửa chữa
- **Learning phase**: Khi đang học và thử nghiệm
- **Development environment**: Không có dữ liệu quan trọng cần bảo toàn
- **Fresh start**: Muốn bắt đầu lại từ đầu với cấu trúc mới


### Ghi chú thêm

- **Không khuyến khích** sử dụng trong production environment
- **Backup dữ liệu** quan trọng trước khi thực hiện
- Phương pháp này **reset hoàn toàn** - không thể khôi phục dữ liệu cũ
- **Video tham khảo**: Lưu lại link video này để tra cứu khi cần reset migrations

**Liên kết:** [[Entity Framework Core]], [[Migration]], [[Package Manager Console]], [[Database Reset]], [[DataAccess Layer]], [[DbContext]], [[Seed Data]], [[SQL Server]], [[Add-Migration]], [[Update-Database]]

