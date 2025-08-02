## Thay đổi Connection String và Migration trong Entity Framework Core

### Tình huống thực tế

#### Vấn đề phát sinh

- Instructor phải reset lại máy tính, dẫn đến việc connection string cũ không hoạt động
- Connection string ban đầu sử dụng server name là `"."` (dot)
- Sau khi reset, server này không thể kết nối được


#### Giải pháp thay đổi Connection String

```json
// appsettings.json - Trước khi thay đổi
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=.;Database=Bulky;..."
  }
}

// appsettings.json - Sau khi thay đổi  
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=LocalDB\\MSSQLLocalDB;Database=Bulky;..."
  }
}
```


### Cách cập nhật Connection String

#### Bước 1: Kiểm tra server khả dụng

- Sử dụng SQL Server Management Studio hoặc công cụ tương tự
- Test kết nối với server `LocalDB\MSSQLLocalDB`
- Copy connection string từ công cụ kết nối


#### Bước 2: Cập nhật appsettings.json

- Thay thế server name từ `"."` thành `"LocalDB\\MSSQLLocalDB"`
- Lưu ý: Sử dụng hai dấu gạch chéo ngược `\\` thay vì một dấu `\`


#### Bước 3: Cập nhật database

```bash
# Chạy trong Package Manager Console
update-database
```


### Tầm quan trọng của Migration

#### Khôi phục dữ liệu tự động

- Database cũ (Bulky) không tồn tại trên server mới
- Chỉ cần chạy `update-database` để tạo lại hoàn toàn
- Tất cả migration được áp dụng tự động
- Seed data được khôi phục (3 categories mặc định)


#### Lợi ích cho team development

- **Developer mới**: Chỉ cần clone repository và chạy `update-database`
- **Môi trường khác nhau**: Local SQL Server, Azure SQL, v.v.
- **Setup nhanh chóng**: Không cần cấu hình phức tạp


### So sánh với thời kỳ trước đây

#### Trước Entity Framework Core

- Mất gần một ngày để setup code trên máy mới
- Nhiều dependencies phức tạp cần xử lý thủ công
- Database schema phải tạo và đồng bộ thủ công


#### Với Entity Framework Core

- Chỉ cần thay đổi connection string
- Chạy một lệnh `update-database`
- Migration tự động được push
- Ứng dụng sẵn sàng chạy


### Quy trình chuẩn khi thay đổi môi trường

#### Cho developer mới

1. Clone repository từ source control
2. Cập nhật connection string trong `appsettings.json`
3. Chạy `update-database` trong Package Manager Console
4. Ứng dụng ready để sử dụng

#### Khi deploy lên production

1. Cập nhật connection string cho môi trường production
2. Chạy `update-database` trên server
3. Tất cả migration và seed data được áp dụng tự động

### Ghi chú quan trọng

#### Connection String Format

- Local SQL Server: `Server=.;` hoặc `Server=(local);`
- LocalDB: `Server=LocalDB\\MSSQLLocalDB;`
- Azure SQL: Connection string cụ thể từ Azure portal


#### Migration Best Practices

- Luôn commit migration files vào source control
- Không xóa migration files cũ
- Test migration trên môi trường development trước

**Liên kết:** [[Entity Framework Core]], [[Migration]], [[Connection String]], [[Package Manager Console]], [[update-database]], [[appsettings.json]], [[LocalDB]], [[Seed Data]], [[Repository Pattern]]

