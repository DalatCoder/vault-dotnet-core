## Custom Validation trong .NET Core MVC

### Khái niệm Custom Validation (Xác thực tùy chỉnh)

- **Data Annotations** cung cấp các validation có sẵn, nhưng đôi khi cần validation phức tạp hơn
- **Custom Validation** cho phép tạo các rules xác thực đặc biệt theo yêu cầu business logic
- Thực hiện trong Controller thông qua kiểm tra điều kiện và thêm lỗi vào ModelState


### Cách tạo Custom Validation

#### Thêm logic kiểm tra trong Controller

```csharp
[HttpPost]
public IActionResult Create(Category obj)
{
    // Custom validation: Name và DisplayOrder không được giống nhau
    if (obj.Name.ToLower() == obj.DisplayOrder.ToString())
    {
        ModelState.AddModelError("Name", "The display order cannot exactly match the name");
    }
    
    if (ModelState.IsValid)
    {
        _db.Categories.Add(obj);
        _db.SaveChanges();  
        return RedirectToAction("Index");
    }
    
    return View(obj);
}
```


#### Cách hoạt động của ModelState.AddModelError()

- **Key parameter**: Tên field mà lỗi sẽ được gắn vào (ví dụ: "Name")
- **Error message**: Thông báo lỗi hiển thị cho người dùng
- Key phải khớp chính xác với tên property trong model


### Hiển thị Custom Error Messages

#### Sử dụng asp-validation-for

```html
<div class="mb-3">
    <label asp-for="Name" class="form-label"></label>
    <input asp-for="Name" class="form-control" />
    <span asp-validation-for="Name" class="text-danger"></span>
</div>
```


#### Lỗi sẽ hiển thị tại field tương ứng

- Custom error message xuất hiện dưới field "Category Name"
- Vị trí hiển thị phụ thuộc vào **key** trong AddModelError()


### Validation Summary (Tóm tắt xác thực)

#### Khái niệm asp-validation-summary

- Hiển thị **tất cả error messages** ở một vị trí tập trung
- Bổ sung cho việc hiển thị lỗi tại từng field riêng lẻ
- Hữu ích khi có nhiều lỗi validation cùng lúc


#### Cách sử dụng

```html
<div class="text-danger">
    <div asp-validation-summary="All"></div>
</div>

<!-- Form fields -->
<div class="mb-3">
    <label asp-for="Name" class="form-label"></label>
    <input asp-for="Name" class="form-control" />
    <span asp-validation-for="Name" class="text-danger"></span>
</div>
```


#### Các tùy chọn cho validation-summary

- **`All`**: Hiển thị tất cả lỗi validation
- **`ModelOnly`**: Chỉ hiển thị lỗi model-level (không liên quan đến field cụ thể)
- **`None`**: Không hiển thị gì


### Ví dụ thực tế hoàn chỉnh

#### Custom validation rule

```csharp
// Kiểm tra Name và DisplayOrder không được giống nhau
if (obj.Name.ToLower() == obj.DisplayOrder.ToString())
{
    ModelState.AddModelError("Name", "The display order cannot exactly match the name");
}
```


#### View template

```html
<div class="text-danger">
    <div asp-validation-summary="All"></div>
</div>

<h2>Create Category</h2>

<form method="post">
    <div class="mb-3">
        <label asp-for="Name" class="form-label"></label>
        <input asp-for="Name" class="form-control" />
        <span asp-validation-for="Name" class="text-danger"></span>
    </div>
    
    <div class="mb-3">
        <label asp-for="DisplayOrder" class="form-label"></label>
        <input asp-for="DisplayOrder" class="form-control" />
        <span asp-validation-for="DisplayOrder" class="text-danger"></span>
    </div>
    
    <button type="submit" class="btn btn-primary">Create</button>
</form>
```


### Quy trình thực hiện

1. **Thêm logic kiểm tra** trong Controller action method
2. **Sử dụng ModelState.AddModelError()** để thêm lỗi tùy chỉnh
3. **Chỉ định key chính xác** để lỗi hiển thị đúng vị trí
4. **Thêm asp-validation-summary** để hiển thị tổng hợp lỗi
5. **Restart application** để thay đổi có hiệu lực

### Ghi chú thêm

- **Key sensitivity**: Key trong AddModelError() phải khớp chính xác với property name
- **Multiple validations**: Có thể thêm nhiều custom validation trong cùng một action
- **Validation order**: Custom validation chạy sau Data Annotations validation
- **Performance**: Custom validation chỉ chạy khi các validation cơ bản đã pass
- **Restart required**: Cần restart application khi thay đổi validation logic

**Liên kết:** [[ModelState]], [[Data Annotations]], [[Custom Validation]], [[asp-validation-summary]], [[asp-validation-for]], [[Category Controller]], [[Server-side Validation]], [[AddModelError]], [[Tag Helpers]]

