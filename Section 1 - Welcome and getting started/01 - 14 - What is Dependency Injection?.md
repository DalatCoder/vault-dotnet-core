## Dependency Injection (Tiêm phụ thuộc)

### Khái niệm cơ bản

**[[Dependency Injection]]** là một design pattern (mẫu thiết kế) trong đó một class hoặc object có các dependent classes được tiêm vào thay vì tạo trực tiếp chúng.

- Mục đích: Không phải tự tạo, quản lý và dispose object
- Cải thiện [[Loose Coupling]] (kết nối lỏng lẻo) giữa các class
- Là câu hỏi phỏng vấn phổ biến trong lập trình .NET


### Ví dụ thực tế - Bob đi hiking

Để hiểu rõ khái niệm, hãy tưởng tượng Bob chuẩn bị đi hiking:

- **Chuẩn bị**: Bob cần nhiều đồ dùng (bản đồ, đèn pin, protein bar...)
- **Container**: Anh ấy đặt tất cả vào ba lô (backpack)
- **Sử dụng**: Khi cần, Bob lấy đồ từ ba lô ra dùng

➡️ **Ba lô chính là container**, chứa sẵn những thứ cần thiết để Bob có thể lấy ra khi cần.

### So sánh: Không có DI vs Có DI

#### Tình huống KHÔNG có Dependency Injection

```csharp
// Trong mỗi page phải viết lặp lại:
public class Page1 
{
    public void ProcessData() 
    {
        // Tạo object database
        DB db = new DB();
        db.GetData();
        db.Dispose(); // Phải tự dispose
        
        // Tạo object email  
        Email email = new Email();
        email.SendEmail();
        email.Dispose(); // Phải tự dispose
    }
}
```

**Vấn đề**:

- Code bị lặp lại trên nhiều page (3 page, 30 page, 300 page...)
- Phải tự tạo, quản lý và dispose object
- Khi thay đổi implementation, phải sửa ở tất cả nơi sử dụng


#### Tình huống CÓ Dependency Injection

```csharp
// Đăng ký trong DI Container
services.AddScoped<IDb, DB>();
services.AddScoped<IEmail, Email>();

// Trong page chỉ cần:
public class Page1 
{
    private readonly IDb _db;
    private readonly IEmail _email;
    
    public Page1(IDb db, IEmail email) // Constructor injection
    {
        _db = db;
        _email = email;
    }
    
    public void ProcessData() 
    {
        _db.GetData(); // Không cần tạo object
        _email.SendEmail(); // Framework tự động inject
    }
}
```


### Lợi ích của Dependency Injection

#### 1. **Loose Coupling (Kết nối lỏng lẻo)**

- Page chỉ biết về interface (`IDb`, `IEmail`)
- Không quan tâm implementation cụ thể là gì


#### 2. **Dễ bảo trì và mở rộng**

- Muốn thay đổi implementation? Chỉ cần sửa trong [[DI Container]]
- Không phải sửa ở từng page riêng lẻ

```csharp
// Thay đổi implementation chỉ ở một nơi:
services.AddScoped<IDb, DB_New>(); // Từ DB sang DB_New
services.AddScoped<IEmail, Email_New>(); // Từ Email sang Email_New
```


#### 3. **Tự động quản lý object lifecycle**

- Framework tự tạo, inject và dispose object
- Developer không cần quan tâm đến memory management


### DI trong .NET Framework

- **[[Dependency Injection]]** được tích hợp sẵn trong .NET
- Chỉ cần đăng ký service trong [[DI Container]]
- Framework sẽ tự động xử lý việc injection


### Ghi chú thêm

- DI giúp code clean, dễ test và maintainable
- Là một trong những nguyên tắc quan trọng của [[SOLID Principles]]
- Thường được sử dụng cùng với [[Interface]] để đạt hiệu quả tối đa
- Câu hỏi phỏng vấn: Hãy chuẩn bị ví dụ thực tế để giải thích concept này

