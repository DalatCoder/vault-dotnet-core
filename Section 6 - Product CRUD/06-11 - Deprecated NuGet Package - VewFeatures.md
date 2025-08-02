## Cập nhật .NET: Xử lý vấn đề SelectListItem trong ViewModel

### Vấn đề với phiên bản .NET mới

Trong video tiếp theo khi thêm Product ViewModel, sẽ xuất hiện vấn đề với `SelectListItem` do có cập nhật trong phiên bản .NET mới. Instructor muốn cảnh báo trước về vấn đề này và cung cấp giải pháp.

### Cấu trúc ViewModel sẽ gặp lỗi

```csharp
public class ProductViewModel
{
    public Product Product { get; set; }
    public IEnumerable<SelectListItem> CategoryList { get; set; }
}
```

Khi nhấn `Ctrl + .` để import namespace cho `SelectListItem`, hệ thống sẽ đề xuất cài đặt NuGet package `Microsoft.AspNetCore.Mvc.Views`.

### Vấn đề với NuGet Package cũ

- **Package deprecated**: `Microsoft.AspNetCore.Mvc.Views` đã bị **deprecated** (không còn được hỗ trợ)
- **Không nên cài đặt**: Tránh làm theo gợi ý cài đặt package này
- **Lý do**: Chức năng đã được chuyển vào default framework của ASP.NET Core


### Giải pháp đúng

**Bước 1: Mở Project File của Models**

Trong thư mục Models project, chỉnh sửa file `.csproj`:

**Bước 2: Thêm Framework Reference**

Thêm dòng sau vào trong thẻ `<ItemGroup>`:

```xml
<ItemGroup>
    <FrameworkReference Include="Microsoft.AspNetCore.App" />
</ItemGroup>
```

**Bước 3: Lưu và kiểm tra**

- Lưu file project
- Quay lại code, nhấn `Ctrl + .`
- Lúc này sẽ có using statement trực tiếp mà không cần cài package


### Lưu ý quan trọng

**Áp dụng cho tất cả project:**

- Khi sử dụng `SelectListItem` trong bất kỳ project nào khác
- **Không cài đặt** deprecated packages
- **Luôn thêm** framework reference vào project file thay vì cài NuGet package

**Framework Reference vs NuGet Package:**

- Framework reference: Tham chiếu đến framework mặc định của ASP.NET Core
- NuGet package: Gói riêng biệt (nhiều gói đã được tích hợp vào framework)


### Lợi ích của phương pháp mới

- **Hiện đại**: Sử dụng cách thức mới nhất của .NET
- **Ổn định**: Không phụ thuộc vào deprecated packages
- **Tích hợp tốt**: Sử dụng chức năng built-in của ASP.NET Core
- **Bảo trì dễ dàng**: Không cần quản lý thêm NuGet dependencies


### Cách xác định project file cần chỉnh sửa

- Mở **Models project** (không phải Web project)
- Tìm file có đuôi `.csproj`
- Chỉnh sửa file này để thêm framework reference
- Framework reference sẽ cung cấp access đến `SelectListItem`


### Ghi chú thêm

- Đây là cập nhật quan trọng trong các phiên bản .NET mới
- Nhiều NuGet packages đã được tích hợp vào core framework
- Trend hiện tại là giảm dependency external packages
- Luôn kiểm tra documentation mới nhất khi gặp vấn đề tương tự
- Phương pháp này áp dụng không chỉ cho SelectListItem mà còn nhiều components khác

**Liên kết:** [[ProductViewModel]], [[SelectListItem]], [[ASP.NET Core Framework]], [[NuGet Package]], [[Project File]], [[Framework Reference]], [[Models Project]], [[Deprecated Package]], [[.NET Updates]]

