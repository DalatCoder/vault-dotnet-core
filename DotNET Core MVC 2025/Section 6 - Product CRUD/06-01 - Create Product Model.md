## Tạo Model Product cho Website Bán Sách

### Khái niệm

Trong website bán sách, chúng ta cần tạo model **Product** để lưu trữ thông tin về các cuốn sách. Model này sẽ kết nối với **Category** và chứa đầy đủ thông tin sản phẩm cần thiết cho việc thực hiện các thao tác CRUD (Create, Read, Update, Delete).

### Cách thực hiện

- Tạo class mới trong thư mục `BulkyBook.Models`
- Đặt tên class là **Product** và khai báo là `public class`
- Sử dụng **Data Annotations** để định nghĩa các ràng buộc và hiển thị


### Thuộc tính của Product

```csharp
public class Product
{
    [Key]
    public int Id { get; set; }
    
    [Required]
    public string Title { get; set; }
    
    public string Description { get; set; }
    
    public string ISBN { get; set; }
    
    public string Author { get; set; }
    
    [Display(Name = "List Price")]
    [Range(1, 1000)]
    public double ListPrice { get; set; }
    
    [Display(Name = "Price")]
    [Range(1, 1000)]
    public double Price { get; set; }
    
    [Display(Name = "Price 50")]
    [Range(1, 1000)]
    public double Price50 { get; set; }
    
    [Display(Name = "Price 100")]
    [Range(1, 1000)]
    public double Price100 { get; set; }
}
```


### Hệ thống giá theo số lượng (Bulk Pricing)

Vì đây là cửa hàng bán sách với số lượng lớn, khách hàng mua nhiều sẽ được giảm giá:

- **ListPrice**: Giá niêm yết gốc
- **Price**: Giá cho số lượng từ 1-50 cuốn
- **Price50**: Giá cho số lượng từ 51 cuốn trở lên
- **Price100**: Giá cho số lượng từ 100 cuốn trở lên


### Data Annotations sử dụng

- `[Key]`: Định nghĩa khóa chính cho thuộc tính Id
- `[Required]`: Bắt buộc nhập cho Title
- `[Display(Name = "...")]`: Hiển thị tên có khoảng trắng trong giao diện
- `[Range(1, 1000)]`: Giới hạn giá trị từ 1 đến 1000 đô la


### Ghi chú thêm

- Model hiện tại chỉ chứa các thuộc tính cơ bản (string và double)
- Thuộc tính **Category** và **Image** sẽ được thêm vào sau
- Cấu trúc tương tự như model Category đã tạo trước đó
- Bài tập tiếp theo: Thực hành CRUD operations cho Product tương tự như đã làm với Category

**Liên kết:** [[CRUD Operations]], [[Data Annotations]], [[Model]], [[Category]], [[Product]], [[Bulk Pricing]], [[Entity Framework]]

