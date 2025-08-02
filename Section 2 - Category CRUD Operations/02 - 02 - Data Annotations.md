## Định nghĩa Primary Key và Data Annotations trong Entity Framework Core

### Khái niệm

Entity Framework Core cần biết thuộc tính nào sẽ là khóa chính (primary key) của bảng. Để làm điều này, chúng ta sử dụng các chú thích dữ liệu (data annotations) hoặc dựa vào các quy tắc đặt tên mặc định.

### Cách định nghĩa Primary Key (Khóa chính)

#### Sử dụng Data Annotation `[Key]`

```csharp
public class Category 
{
    [Key]
    public int CategoryId { get; set; }
    // Các thuộc tính khác...
}
```

- Thêm `[Key]` trong dấu ngoặc vuông phía trên thuộc tính
- Namespace: `System.ComponentModel.DataAnnotations`
- Cách này giúp [[Entity Framework Core]] nhận biết rõ ràng thuộc tính nào là primary key


#### Quy tắc tự động nhận diện Primary Key

Entity Framework Core sẽ **tự động** nhận diện primary key trong các trường hợp:

1. **Tên thuộc tính là `Id`**

```csharp
public int Id { get; set; } // Tự động là primary key
```

2. **Tên thuộc tính theo format `[TênModel]Id`**

```csharp
public class Category 
{
    public int CategoryId { get; set; } // Tự động là primary key
}
```


### Data Annotations khác

#### `[Required]` - Bắt buộc

```csharp
public class Category 
{
    public int Id { get; set; }
    
    [Required]
    public string Name { get; set; }
}
```

- Khi tạo bảng SQL, thuộc tính có `[Required]` sẽ có ràng buộc `NOT NULL`
- Đảm bảo dữ liệu không được để trống


### Ghi chú thêm

- Mặc dù Entity Framework Core có thể tự động nhận diện primary key, việc sử dụng `[Key]` giúp code rõ ràng và dễ hiểu hơn
- Có nhiều [[Data Annotations]] khác sẽ được học trong các bài tiếp theo
- Bước tiếp theo sẽ là cấu hình [[Connection String]] và kết nối với SQL Server để thực sự tạo bảng trong cơ sở dữ liệu

**Liên kết:** [[Entity Framework Core]], [[Data Annotations]], [[Connection String]], [[SQL Server]]

