## Tạo chức năng xóa danh mục (Delete Category)

### Khái niệm

Chức năng **Delete** sẽ hiển thị thông tin danh mục giống như trang **Edit**, nhưng tất cả các trường sẽ bị **vô hiệu hóa** (disabled). Người dùng chỉ có thể xem thông tin và xác nhận xóa.

### Tạo Action Methods cho Delete

#### Sao chép từ Edit Action

```csharp
// GET action method cho Delete
public IActionResult Delete(int? id)
{
    if (id == null || id == 0)
    {
        return NotFound();
    }
    
    Category? categoryFromDb = _db.Categories.Find(id);
    
    if (categoryFromDb == null)
    {
        return NotFound();
    }
    
    return View(categoryFromDb);
}
```


#### Xử lý POST Action với tên khác biệt

```csharp
// POST action method với tên khác để tránh conflict
[HttpPost, ActionName("Delete")]
public IActionResult DeletePost(int? id)
{
    Category? obj = _db.Categories.Find(id);
    
    if (obj == null)
    {
        return NotFound();
    }
    
    _db.Categories.Remove(obj);
    _db.SaveChanges();
    return RedirectToAction("Index");
}
```


### Giải quyết xung đột tên Action Method

#### Vấn đề với tham số giống nhau

- **GET Delete**: Nhận tham số `int? id`
- **POST Delete**: Cũng nhận tham số `int? id`
- **Xung đột**: Không thể có 2 action methods cùng tên và cùng signature


#### Giải pháp sử dụng ActionName Attribute

```csharp
[HttpPost, ActionName("Delete")]
public IActionResult DeletePost(int? id)
{
    // Implementation
}
```


#### Giải thích cơ chế

- **Method name**: `DeletePost` (tên thực tế trong code)
- **Action name**: `Delete` (tên được expose ra route)
- **Form submission**: Vẫn post đến action name `Delete`
- **Routing**: Framework tự động map đúng method dựa vào HTTP verb


### Quy trình xóa dữ liệu với Entity Framework

#### Các bước thực hiện

1. **Tìm kiếm**: Sử dụng `Find(id)` để lấy entity từ database
2. **Kiểm tra**: Validate entity có tồn tại không
3. **Xóa**: Sử dụng `Remove(obj)` để đánh dấu xóa
4. **Lưu**: Gọi `SaveChanges()` để commit transaction
5. **Chuyển hướng**: Redirect về trang Index

#### Code chi tiết

```csharp
// Bước 1: Tìm category cần xóa
Category? obj = _db.Categories.Find(id);

// Bước 2: Kiểm tra tồn tại
if (obj == null)
{
    return NotFound();
}

// Bước 3: Đánh dấu xóa
_db.Categories.Remove(obj);

// Bước 4: Lưu thay đổi
_db.SaveChanges();

// Bước 5: Chuyển hướng
return RedirectToAction("Index");
```


### Phương thức Remove() của Entity Framework

#### Cách hoạt động

- **Input**: Nhận object entity cần xóa
- **Process**: Đánh dấu entity ở trạng thái `Deleted`
- **Database**: Chưa xóa thực sự cho đến khi `SaveChanges()`
- **SQL generated**: `DELETE FROM Categories WHERE Id = @id`


#### So sánh với Update()

- **Update**: Modify existing record
- **Remove**: Delete entire record
- **Common**: Đều cần `SaveChanges()` để persist


### Lưu ý quan trọng

#### Validation và Error Handling

- **ID validation**: Kiểm tra `id` null hoặc 0
- **Entity validation**: Kiểm tra object có tồn tại trong database
- **NotFound result**: Trả về 404 nếu không tìm thấy
- **Transaction safety**: EF Core đảm bảo atomic operation


#### Best Practices

- **Confirm before delete**: View sẽ hiển thị thông tin để user xác nhận
- **Soft delete**: Có thể implement soft delete thay vì hard delete
- **Cascade delete**: Cần cân nhắc ảnh hưởng đến related entities
- **Logging**: Nên log các action delete để audit


### Chuẩn bị cho View

Bước tiếp theo sẽ tạo Delete view để hiển thị thông tin danh mục với các trường bị disable, cho phép người dùng xác nhận xóa.

**Liên kết:** [[Category Controller]], [[Entity Framework Core]], [[Remove Method]], [[ActionName Attribute]], [[HTTP POST]], [[Find Method]], [[SaveChanges]], [[Delete View]], [[CRUD Operations]], [[NotFound Result]]

