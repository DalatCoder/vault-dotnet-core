## Sử dụng TempData để hiển thị thông báo trong ASP.NET Core

### Khái niệm TempData

**TempData** là một tính năng đặc biệt trong .NET Core được thiết kế để lưu trữ dữ liệu tạm thời chỉ **có sẵn cho lần render tiếp theo**. Đây là giải pháp lý tưởng để hiển thị thông báo (notification) sau khi thực hiện các thao tác CRUD.

#### Đặc điểm của TempData

- **Tồn tại ngắn hạn**: Chỉ available cho 1 request tiếp theo
- **Tự động xóa**: Sau khi hiển thị, dữ liệu sẽ biến mất
- **Cross-request**: Có thể truyền dữ liệu giữa các action methods
- **Mục đích chính**: Hiển thị thông báo thành công/lỗi sau redirect


### Cách thức hoạt động

```
Action POST (Create/Edit/Delete) → Lưu message vào TempData → Redirect → Index Page → Hiển thị message → TempData tự động xóa
```


### Triển khai TempData trong Controller

#### Thêm thông báo vào các POST actions

```csharp
// Trong Create POST action
[HttpPost]
public IActionResult Create(Category obj)
{
    if (ModelState.IsValid)
    {
        _db.Categories.Add(obj);
        _db.SaveChanges();
        
        // Thêm thông báo vào TempData
        TempData["success"] = "Category created successfully";
        
        return RedirectToAction("Index");
    }
    return View(obj);
}

// Trong Edit POST action
[HttpPost]
public IActionResult Edit(Category obj)
{
    if (ModelState.IsValid)
    {
        _db.Categories.Update(obj);
        _db.SaveChanges();
        
        TempData["success"] = "Category updated successfully";
        
        return RedirectToAction("Index");
    }
    return View(obj);
}

// Trong Delete POST action
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
    
    TempData["success"] = "Category deleted successfully";
    
    return RedirectToAction("Index");
}
```


#### Cơ chế lưu trữ

- **Key-Value pattern**: `TempData["key_name"] = "message"`
- **Key name**: Sử dụng tên key nhất quán (ví dụ: "success")
- **Message content**: Nội dung thông báo tương ứng với từng action


### Hiển thị thông báo trong View

#### Thêm logic hiển thị trong Index view

```html
@* Ở đầu trang Index.cshtml *@
@if (TempData["success"] != null)
{
    <h2>@TempData["success"]</h2>
}

@* Phần còn lại của view *@
<div class="container">
    <!-- Nội dung danh sách categories -->
</div>
```


#### Cơ chế kiểm tra

- **Conditional rendering**: Chỉ hiển thị khi TempData có giá trị
- **Razor syntax**: Sử dụng `@TempData["key"]` để truy xuất giá trị
- **Null check**: Kiểm tra `!= null` trước khi hiển thị


### Kiểm tra hoạt động

#### Test case thực tế

1. **Tạo mới**: Tạo "Test category 4" → Hiển thị "Category created successfully"
2. **Chỉnh sửa**: Sửa display order thành 44 → Hiển thị "Category updated successfully"
3. **Refresh page**: Thông báo biến mất sau khi refresh
4. **Xóa**: Xóa category → Hiển thị "Category deleted successfully"

#### Đặc điểm quan trọng

- **One-time display**: Thông báo chỉ hiển thị 1 lần
- **Auto-cleanup**: Tự động xóa sau khi render
- **Refresh behavior**: F5 sẽ làm thông báo biến mất


### Ưu điểm của TempData

#### So với các phương pháp khác

- **Session**: TempData tự động cleanup, Session cần xóa thủ công
- **ViewBag/ViewData**: Chỉ hoạt động trong cùng 1 request
- **Query string**: TempData không làm URL dài và phức tạp


#### Best practices

- **Consistent key names**: Sử dụng tên key nhất quán
- **Simple messages**: Thông báo ngắn gọn, rõ ràng
- **User experience**: Cải thiện feedback cho người dùng
- **No persistence**: Không cần quản lý cleanup manual


### Ghi chú thêm

- **Framework design**: TempData được thiết kế chuyên biệt cho notification scenarios
- **Request lifecycle**: Hoạt động hoàn hảo với pattern POST-Redirect-GET
- **Memory efficient**: Tự động giải phóng bộ nhớ sau sử dụng
- **Cross-controller**: Có thể sử dụng giữa các controller khác nhau

**Liên kết:** [[Category Controller]], [[TempData]], [[CRUD Operations]], [[POST-Redirect-GET Pattern]], [[Razor Syntax]], [[User Notifications]], [[Session State]], [[ViewBag]], [[ViewData]], [[Request Lifecycle]], [[Model State Validation]]

