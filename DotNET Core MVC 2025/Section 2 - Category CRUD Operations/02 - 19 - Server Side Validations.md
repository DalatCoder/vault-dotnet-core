## Validation (Xác thực) trong .NET Core MVC

### Khái niệm Server-side Validation

- **Server-side Validation**: Xác thực dữ liệu được thực hiện trên server trước khi lưu vào database
- .NET Core tích hợp sẵn hệ thống validation mạnh mẽ thông qua Data Annotations
- Hỗ trợ jQuery validation được tích hợp sẵn trong project


### Data Annotations cho Validation

#### Các loại validation phổ biến

```csharp
public class Category
{
    [Required]
    [MaxLength(30)]
    [Display(Name = "Category Name")]  
    public string Name { get; set; }
    
    [Range(1, 100, ErrorMessage = "Display Order for category must be between 1-100")]
    [Display(Name = "Display Order")]
    public int DisplayOrder { get; set; }
}
```


#### Các Data Annotations thông dụng

- **`[Required]`**: Bắt buộc phải nhập
- **`[Range(min, max)]`**: Giới hạn giá trị trong khoảng
- **`[MaxLength(n)]`**: Giới hạn độ dài tối đa
- **`[ErrorMessage]`**: Tùy chỉnh thông báo lỗi


### Kiểm tra Validation trong Controller

#### Sử dụng ModelState.IsValid

```csharp
[HttpPost]
public IActionResult Create(Category obj)
{
    if (ModelState.IsValid)
    {
        _db.Categories.Add(obj);
        _db.SaveChanges();
        return RedirectToAction("Index");
    }
    
    // Nếu validation fail, trả về view với dữ liệu cũ
    return View(obj);
}
```


#### Cách hoạt động

- **ModelState.IsValid**: Kiểm tra tất cả validation rules trên model
- Nếu **valid**: Thực hiện logic business và redirect
- Nếu **invalid**: Trả về view để hiển thị lỗi


### Hiển thị Error Messages với Tag Helpers

#### Cú pháp asp-validation-for

```html
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
```


#### Đặc điểm của asp-validation-for

- **Tự động binding**: Liên kết với property tương ứng trong model
- **Bootstrap classes**: Có thể thêm class CSS như `text-danger` để định dạng
- **Dynamic content**: Chỉ hiển thị khi có lỗi validation


### Tùy chỉnh Error Messages

#### Cách thêm custom error message

```csharp
[Range(1, 100, ErrorMessage = "Display Order for category must be between 1-100")]
public int DisplayOrder { get; set; }
```


#### Lợi ích

- **User-friendly**: Thông báo lỗi dễ hiểu hơn
- **Đa ngôn ngữ**: Có thể tùy chỉnh theo ngôn ngữ
- **Branding**: Phù hợp với tone của ứng dụng


### Debug và kiểm tra Validation

#### Cách debug ModelState

```csharp
// Thêm breakpoint để kiểm tra
if (ModelState.IsValid)
{
    // Kiểm tra ModelState.Values để xem chi tiết lỗi
}
```


#### Thông tin debug hữu ích

- **ModelState.IsValid**: True/False
- **ModelState.Values**: Chi tiết từng field validation
- **Error messages**: Thông báo lỗi cụ thể cho từng trường


### Quy trình hoàn chỉnh

1. **Định nghĩa validation** trong model với Data Annotations
2. **Kiểm tra ModelState.IsValid** trong controller
3. **Xử lý hai trường hợp**:
    - Valid: Lưu data và redirect
    - Invalid: Trả về view với error messages
4. **Hiển thị lỗi** trong view bằng `asp-validation-for`
5. **Tùy chỉnh messages** nếu cần thiết

### Ghi chú thêm

- Cần restart application sau khi thay đổi validation rules
- Tag helpers tự động render error messages khi có lỗi
- Server-side validation luôn chạy trước khi client-side validation
- Có thể kết hợp với client-side validation để có trải nghiệm tốt hơn

**Liên kết:** [[Data Annotations]], [[ModelState]], [[Tag Helpers]], [[Category Model]], [[Server-side Validation]], [[asp-validation-for]], [[Custom Error Messages]], [[Bootstrap Classes]]

