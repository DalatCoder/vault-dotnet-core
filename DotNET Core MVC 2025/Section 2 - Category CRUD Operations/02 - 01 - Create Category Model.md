## Tạo Model trong Entity Framework Core

### Khái niệm cơ bản

Khi phát triển website, chúng ta thường làm việc với cơ sở dữ liệu chứa nhiều bảng để thực hiện các thao tác và hiển thị dữ liệu.

**Điểm khác biệt của Entity Framework Core:**

- Không tạo bảng trực tiếp trong cơ sở dữ liệu
- Tạo **mô hình (model)** trong dự án
- Entity Framework tự động phân tích model và tạo bảng tương ứng trong database
- Đây là sức mạnh của [[Entity Framework Core]] trong ứng dụng [[MVC]]


### Tạo Model Category

#### Vị trí thư mục Models

- Thông thường models được đặt trong thư mục `Models`
- **Lưu ý quan trọng:** Khác với `Controllers` và `Views`, tên thư mục `Models` có thể thay đổi
- Thư mục `Controllers` và `Views` bắt buộc phải giữ nguyên tên


#### Các bước tạo model

1. **Tạo class Category:**
    - Chuột phải vào thư mục Models → Add → New Class
    - Đặt tên: `Category.cs`
2. **Định nghĩa properties:**
```csharp
public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int DisplayOrder { get; set; }
}
```


#### Giải thích các thuộc tính

- **Id (int):** [[Primary Key]] của bảng category
- **Name (string):** Tên danh mục
- **DisplayOrder (int):** Thứ tự hiển thị các danh mục trên trang


### Mối quan hệ Model-Database

**Nguyên tắc ánh xạ:**

- Mỗi **property** trong class = một **cột (column)** trong bảng database
- Class Category sẽ tự động tạo bảng Category với 3 cột tương ứng


### Kỹ thuật tạo property nhanh

**Code snippet hữu ích:**

- Gõ `prop` → nhấn Tab 2 lần
- Tự động tạo cấu trúc property cơ bản


### Ghi chú thêm

**Data Annotation:** Để xác định rõ ràng Id là primary key và các quy tắc khác cho model, cần sử dụng [[Data Annotation]]. Nội dung này sẽ được đề cập trong bài học tiếp theo.

**Liên kết tham khảo:**

- [[Entity Framework Core]]
- [[MVC Pattern]]
- [[Data Annotation]]
- [[Primary Key]]

