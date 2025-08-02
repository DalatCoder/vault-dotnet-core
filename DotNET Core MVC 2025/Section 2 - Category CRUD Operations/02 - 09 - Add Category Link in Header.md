## Thêm Navigation cho Category trong MVC Layout

### Mục tiêu

Thay vì phải nhập URL thủ công `/Category/Index` mỗi lần chạy ứng dụng, chúng ta muốn thêm link **Category** vào thanh navigation để dễ dàng truy cập trang danh sách categories.

### Vị trí chỉnh sửa Navigation

#### Tìm file Layout chính

- Navigation header nằm trong file `_Layout.cshtml`
- Đường dẫn: `Views/Shared/_Layout.cshtml`
- Đây là layout chung cho toàn bộ ứng dụng


#### Cấu trúc HTML hiện tại

```html
<ul>
    <li>
        <a asp-controller="Home" asp-action="Index">Home</a>
    </li>
    <li>
        <a asp-controller="Home" asp-action="Privacy">Privacy</a>
    </li>
</ul>
```


### Khái niệm Tag Helpers (Trợ giúp thẻ)

#### So sánh với href truyền thống

```html
<!-- Cách cũ với href -->
<a href="/Home/Index">Home</a>

<!-- Cách mới với Tag Helpers -->
<a asp-controller="Home" asp-action="Index">Home</a>
```


#### Lợi ích của Tag Helpers

- **Rõ ràng hơn**: Thể hiện rõ controller và action nào được gọi
- **Type-safe**: IDE có thể kiểm tra tính hợp lệ của controller/action
- **Dễ bảo trì**: Khi đổi tên controller/action, có thể refactor dễ dàng
- **URL Generation**: Tự động tạo URL đúng format


#### Các thuộc tính Tag Helper

- `asp-controller`: Tên [[Controller]] (không cần từ khóa "Controller")
- `asp-action`: Tên [[Action Method]]
- `asp-area`: Khu vực (có thể bỏ qua nếu không dùng Areas)


### Thêm Navigation cho Category

#### Code cần thêm

```html
<li>
    <a asp-controller="Category" asp-action="Index">Category</a>
</li>
```


#### Vị trí chèn

Thêm vào danh sách `<ul>` hiện có trong `_Layout.cshtml`, cùng với các link Home và Privacy.

#### Cấu trúc hoàn chỉnh

```html
<ul>
    <li>
        <a asp-controller="Home" asp-action="Index">Home</a>
    </li>
    <li>
        <a asp-controller="Home" asp-action="Privacy">Privacy</a>
    </li>
    <li>
        <a asp-controller="Category" asp-action="Index">Category</a>
    </li>
</ul>
```


### Kiểm tra kết quả

#### Chạy ứng dụng

- Xuất hiện link **Category** trong navigation bar
- Click vào **Category** sẽ điều hướng đến `/Category/Index`
- Không cần nhập URL thủ công nữa


#### Debug và test

- Có thể đặt breakpoint trong [[Action Method]] để kiểm tra
- Click vào navigation sẽ trigger debugging point
- Remove breakpoint bằng cách click lại vào dấu đỏ


### Ghi chú thêm

#### Bootstrap Classes

- Các class CSS như `nav`, `navbar` là của Bootstrap framework
- Không cần quan tâm đến styling, chỉ tập trung vào chức năng navigation


#### Navigation Pattern

- Mỗi [[Controller]] nên có link navigation tương ứng
- Sử dụng [[Tag Helpers]] thay vì hardcode href URLs
- Layout được chia sẻ cho tất cả views trong ứng dụng


#### Bước tiếp theo

- Trang Category hiện đang hiển thị text tĩnh
- Sẽ cần kết nối với [[Entity Framework Core]] để hiển thị dữ liệu thực từ database

**Liên kết:** [[Controller]], [[Action Method]], [[Tag Helpers]], [[Entity Framework Core]], [[_Layout.cshtml]], [[Navigation]]

