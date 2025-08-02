## Đổi tên Project và xử lý các vấn đề phát sinh

### Tình huống thực tế

Trong quá trình phát triển, đôi khi cần đổi tên project. Ví dụ từ **Bulky** thành **Bulky Book**. Việc này tuy đơn giản nhưng có thể gây ra một số vấn đề cần xử lý.

### Các bước đổi tên Project

#### Bước 1: Đổi tên từng project

- Click phải vào project → Rename
- Đổi tên tất cả projects:
    - `Bulky.Web` → `BulkyBook.Web`
    - `Bulky.DataAccess` → `BulkyBook.DataAccess`
    - `Bulky.Models` → `BulkyBook.Models`
    - `Bulky.Utility` → `BulkyBook.Utility`


#### Bước 2: Thay đổi namespace trong code

- Sử dụng **Find and Replace** (Ctrl+Shift+F)
- Tìm: `Bulky`
- Thay thế: `BulkyBook`
- Chọn "Replace All"


#### Bước 3: Xử lý lỗi dependencies

Sau khi đổi tên, có thể gặp lỗi dependencies không nhận diện được:

```xml
<!-- Cần xóa dependencies cũ và thêm lại -->
<ProjectReference Include="..\BulkyBook.Models\BulkyBook.Models.csproj" />
<ProjectReference Include="..\BulkyBook.Utility\BulkyBook.Utility.csproj" />
```

**Cách khắc phục:**

- Edit project file
- Xóa các dependencies cũ
- Add lại project reference với tên mới
- Restart Visual Studio nếu cần


### CSS Isolation trong .NET 6

#### Khái niệm mới

- **CSS Isolation** (CSS cô lập) được giới thiệu từ .NET 6
- Tự động bundling và localization CSS
- Mỗi file layout có thể có CSS riêng


#### Cách hoạt động

```html
<!-- Trong _Layout.cshtml -->
<link rel="stylesheet" href="~/BulkyBook.Web.styles.css" asp-append-version="true" />
```

- File `_Layout.cshtml` có thể có file `_Layout.cshtml.css` tương ứng
- CSS được tự động scope theo layout
- Tên file CSS phải khớp với tên project: `{ProjectName}.styles.css`


#### Lỗi thường gặp

- Nếu tên project thay đổi nhưng CSS reference không cập nhật → Footer/styling bị lỗi
- Cần đảm bảo `href` khớp với tên project mới


### Vấn đề với Connection String

#### Lỗi không mong muốn

Khi dùng Find/Replace toàn bộ, có thể vô tình thay đổi:

```json
// Trước đây
"DefaultConnection": "Server=.;Database=Bulky;..."

// Sau Find/Replace (KHÔNG MONG MUỐN)
"DefaultConnection": "Server=.;Database=BulkyBook;..."
```


#### Khắc phục

- Database name nên giữ nguyên là `Bulky`
- Chỉ thay đổi tên project, không thay đổi database name
- Kiểm tra lại `appsettings.json` sau khi Find/Replace


### Các lỗi thường gặp và cách xử lý

#### Lỗi ".user file already exists"

- Xóa file `.user` cũ trong thư mục project
- Thực hiện rename lại


#### Dependencies không nhận diện

- Restart Visual Studio
- Rebuild solution
- Kiểm tra project references trong file `.csproj`


#### CSS không load

- Kiểm tra tên file CSS trong `_Layout.cshtml`
- Đảm bảo tên project khớp với CSS reference


### Bài học quan trọng

#### Trong môi trường thực tế

- Đây là những vấn đề thường gặp trong dự án thực
- Cần hiểu rõ cách troubleshoot các lỗi này
- CSS Isolation là tính năng mới cần nắm vững


#### Best Practices

- Backup project trước khi đổi tên
- Kiểm tra từng component sau khi đổi tên
- Test ứng dụng để đảm bảo hoạt động bình thường
- Chú ý đến connection string và database name


### Ghi chú quan trọng

- **CSS Isolation** là tính năng tự động trong .NET 6+
- Việc đổi tên project có thể ảnh hưởng đến nhiều thành phần
- Cần cẩn thận với Find/Replace để tránh thay đổi nhầm database name
- Visual Studio đôi khi cần restart để nhận diện dependencies mới

**Liên kết:** [[Project Rename]], [[CSS Isolation]], [[.NET 6]], [[Dependencies]], [[Project Reference]], [[Connection String]], [[Visual Studio]], [[Find Replace]], [[appsettings.json]], [[Layout CSS]]

