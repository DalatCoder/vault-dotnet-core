## Tag Helpers và Data Annotations trong .NET Core

### Khái niệm Tag Helpers (Trình trợ giúp thẻ)

- **Tag Helpers** trong .NET Core giúp tạo các input fields động và linh hoạt hơn
- Thay vì sử dụng thuộc tính `name` như các phiên bản .NET cũ, .NET Core đơn giản hóa việc binding dữ liệu
- Tag Helpers tự động liên kết các input fields với properties trong model


### Cách sử dụng ASP-for Tag Helper

#### Cú pháp cơ bản

```html
<input asp-for="PropertyName" />
<label asp-for="PropertyName"></label>
```


#### Ví dụ thực tế

- Với model `Category` có properties `Name` và `DisplayOrder`:

```html
<input asp-for="Name" />
<input asp-for="DisplayOrder" />
```


#### Các lợi ích chính

- **Tự động xác định kiểu dữ liệu**: Không cần khai báo `type`, hệ thống tự động nhận biết dựa trên property type
- **IntelliSense hỗ trợ**: IDE sẽ gợi ý các properties có sẵn trong model
- **Validation tự động**: Tích hợp sẵn với client-side validation
- **Liên kết label-input**: Khi click vào label sẽ tự động focus vào input tương ứng


### Data Annotations để tùy chỉnh hiển thị

#### Vấn đề tên thuộc tính

- Properties trong model thường không có khoảng trắng (ví dụ: `DisplayOrder`)
- Cần hiển thị tên thân thiện hơn trên UI (ví dụ: "Display Order")


#### Giải pháp với DisplayName Attribute

```csharp
public class Category
{
    [Display(Name = "Category Name")]  
    public string Name { get; set; }
    
    [Display(Name = "Display Order")]
    public int DisplayOrder { get; set; }
}
```


#### Kết quả

- Tự động hiển thị "Category Name" thay vì "Name"
- Tự động hiển thị "Display Order" thay vì "DisplayOrder"
- Không cần thay đổi code trong view


### Cách thực hiện trong thực tế

1. **Định nghĩa model** với các Data Annotations cần thiết
2. **Sử dụng `asp-for`** trong các thẻ input và label
3. **Khởi động lại ứng dụng** nếu cần thiết để áp dụng thay đổi UI
4. **Kiểm tra kết quả** trên trình duyệt

### Ghi chú thêm

- Tag Helpers chỉ hoạt động với properties được định nghĩa trong model
- Nếu viết sai tên property, IntelliSense sẽ báo lỗi ngay lập tức
- Có thể cần khởi động lại ứng dụng để thay đổi UI có hiệu lực
- Input type được tự động xác định (ví dụ: `int` sẽ tạo input number)

**Liên kết:** [[Entity Framework Core]], [[Model Binding]], [[Data Annotations]], [[ASP.NET Core MVC]], [[Tag Helpers]], [[Category Model]], [[Client-side Validation]]

