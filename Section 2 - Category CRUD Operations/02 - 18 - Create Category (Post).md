## Tạo Action Method POST để Thêm Category

### Khái niệm HTTP POST Action Method

- Khi form được submit với method POST, cần tạo action method tương ứng để xử lý
- Action method POST phải có **cùng tên** với action method GET nhưng khác HTTP verb
- Sử dụng attribute `[HttpPost]` để phân biệt với GET method


### Cách tạo Action Method POST

#### Cấu trúc cơ bản

```csharp
[HttpPost]
public IActionResult Create(Category obj)
{
    // Xử lý logic thêm category
    return RedirectToAction("Index");
}
```


#### Model Binding tự động

- **Model Binding**: .NET Core tự động map dữ liệu từ form vào object parameter
- Object `Category obj` sẽ chứa các giá trị từ form fields
- ID sẽ có giá trị 0 (phù hợp cho việc tạo mới)


### Sử dụng Entity Framework Core để thêm dữ liệu

#### Các bước thực hiện

1. **Thêm object vào DbContext**:
```csharp
_db.Categories.Add(obj);
```

2. **Lưu thay đổi vào database**:
```csharp
_db.SaveChanges();
```


#### Cơ chế hoạt động

- **Add()**: Đánh dấu object cần được thêm vào database (chưa thực thi)
- **SaveChanges()**: Thực thi tất cả thay đổi đã được track vào database
- Entity Framework Core tự động tạo câu lệnh SQL INSERT


### Redirect sau khi thêm thành công

#### Tại sao cần Redirect?

- Sau khi thêm thành công, cần chuyển hướng về trang danh sách
- Trang Index sẽ reload và hiển thị category mới được thêm


#### Cú pháp RedirectToAction

```csharp
return RedirectToAction("Index"); // Cùng controller
return RedirectToAction("Index", "Category"); // Khác controller
```


### Ví dụ hoàn chỉnh

```csharp
[HttpPost]
public IActionResult Create(Category obj)
{
    // Thêm category vào DbContext
    _db.Categories.Add(obj);
    
    // Lưu thay đổi vào database
    _db.SaveChanges();
    
    // Redirect về trang Index
    return RedirectToAction("Index");
}
```


### Debug và kiểm tra

#### Các điểm cần debug

- **Breakpoint tại parameter**: Kiểm tra dữ liệu từ form
- **Breakpoint tại Add()**: Xác nhận object được thêm vào DbContext
- **Breakpoint tại SaveChanges()**: Kiểm tra thực thi database


#### Kết quả mong đợi

- Object `obj` chứa đầy đủ thông tin từ form
- ID = 0 (cho việc tạo mới)
- Sau SaveChanges(), dữ liệu xuất hiện trong database
- Redirect thành công về trang Index


### Lưu ý quan trọng

- **Không cần viết SQL**: Entity Framework Core tự động generate câu lệnh INSERT
- **Không cần quản lý connection**: Framework tự động mở/đóng kết nối database
- **Change Tracking**: EF Core theo dõi các thay đổi trước khi thực thi
- **Batch Operations**: Có thể gộp nhiều thay đổi trong một lần SaveChanges()


### Ghi chú thêm

- Nên thêm validation trước khi save vào database
- Có thể sử dụng async/await cho hiệu suất tốt hơn với `SaveChangesAsync()`
- Cần xử lý exception khi có lỗi database

**Liên kết:** [[Entity Framework Core]], [[Model Binding]], [[HTTP POST]], [[DbContext]], [[Category Controller]], [[SaveChanges]], [[RedirectToAction]], [[Change Tracking]]

