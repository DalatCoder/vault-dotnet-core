## Tùy chọn asp-validation-summary trong .NET Core MVC

### Các tùy chọn của asp-validation-summary

asp-validation-summary cung cấp 3 tùy chọn chính để hiển thị error messages:

- **All**: Hiển thị tất cả validation errors (cả property-level và model-level)
- **ModelOnly**: Chỉ hiển thị model-level errors (không liên quan đến property cụ thể)
- **None**: Không hiển thị bất kỳ error message nào


### Custom Validation không có Key

#### Cách tạo model-level validation

```csharp
[HttpPost]
public IActionResult Create(Category obj)
{
    // Custom validation với key cụ thể (property-level)
    if (obj.Name.ToLower() == obj.DisplayOrder.ToString())
    {
        ModelState.AddModelError("Name", "The display order cannot exactly match the name");
    }
    
    // Custom validation không có key (model-level)
    if (obj.Name != null && obj.Name.ToLower() == "test")
    {
        ModelState.AddModelError("", "Test is an invalid value");
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


#### Đặc điểm của model-level validation

- **Key rỗng**: Sử dụng `""` thay vì tên property
- **Không gắn vào field**: Lỗi không hiển thị dưới input cụ thể nào
- **Chỉ hiển thị trong summary**: Xuất hiện trong validation summary, không ở individual field


### So sánh các tùy chọn validation-summary

#### Tùy chọn "All"

```html
<div asp-validation-summary="All" class="text-danger"></div>
```

- **Hiển thị**: Tất cả lỗi validation
- **Bao gồm**: Property-level errors + Model-level errors
- **Use case**: Khi muốn hiển thị tất cả lỗi ở một vị trí tập trung


#### Tùy chọn "ModelOnly"

```html
<div asp-validation-summary="ModelOnly" class="text-danger"></div>
```

- **Hiển thị**: Chỉ model-level errors
- **Loại trừ**: Property-level errors (Name, DisplayOrder validation)
- **Use case**: Login page - hiển thị lỗi chung như "Invalid username/password"


#### Tùy chọn "None"

```html
<div asp-validation-summary="None" class="text-danger"></div>
```

- **Hiển thị**: Không hiển thị gì
- **Use case**: Khi chỉ muốn hiển thị lỗi tại individual fields


### Use case thực tế: Login Page

#### Kịch bản sử dụng ModelOnly

```csharp
// Login Controller
[HttpPost]
public IActionResult Login(LoginModel model)
{
    // Property-level validation (Required fields)
    // Tự động hiển thị dưới từng input field
    
    // Model-level validation (Business logic)
    if (model.Username != null && model.Password != null)
    {
        if (!ValidateUser(model.Username, model.Password))
        {
            ModelState.AddModelError("", "Invalid username or password");
        }
    }
    
    return View(model);
}
```


#### Template cho Login Page

```html
<!-- Chỉ hiển thị lỗi chung, không hiển thị lỗi Required -->
<div asp-validation-summary="ModelOnly" class="text-danger"></div>

<div class="mb-3">
    <label asp-for="Username" class="form-label"></label>
    <input asp-for="Username" class="form-control" />
    <!-- Lỗi Required sẽ hiển thị ở đây -->
    <span asp-validation-for="Username" class="text-danger"></span>
</div>

<div class="mb-3">
    <label asp-for="Password" class="form-label"></label>
    <input asp-for="Password" class="form-control" type="password" />
    <!-- Lỗi Required sẽ hiển thị ở đây -->
    <span asp-validation-for="Password" class="text-danger"></span>
</div>
```


### Quy tắc hiển thị Error Messages

#### Property-level errors

- **Có key**: `ModelState.AddModelError("Name", "Error message")`
- **Hiển thị tại**: Individual field validation spans
- **Hiển thị trong summary**: Khi sử dụng "All"


#### Model-level errors

- **Không có key**: `ModelState.AddModelError("", "Error message")`
- **Hiển thị tại**: Chỉ trong validation summary
- **Hiển thị trong summary**: Khi sử dụng "All" hoặc "ModelOnly"


### Best Practices

#### Khi nào sử dụng từng tùy chọn

- **All**: Trang có ít fields, muốn tập trung tất cả lỗi
- **ModelOnly**: Login page, form phức tạp với lỗi business logic
- **None**: Khi chỉ muốn hiển thị lỗi tại individual fields


#### Cấu trúc code clean

```csharp
// Comment out custom validation không cần thiết
// if (obj.Name != null && obj.Name.ToLower() == "test")
// {
//     ModelState.AddModelError("", "Test is an invalid value");
// }
```


### Ghi chú thêm

- **Null checking**: Cần kiểm tra `obj.Name != null` trước khi gọi `.ToLower()`
- **Individual validation**: Có thể comment validation summary và chỉ dùng individual field validation
- **Flexibility**: Có thể kết hợp cả validation summary và individual field validation
- **Restart required**: Cần restart application khi thay đổi validation logic

**Liên kết:** [[asp-validation-summary]], [[ModelState]], [[Custom Validation]], [[asp-validation-for]], [[Property-level Validation]], [[Model-level Validation]], [[Login Page]], [[AddModelError]], [[Category Controller]]

