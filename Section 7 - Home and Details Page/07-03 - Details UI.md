## Thiết Kế Giao Diện Trang Chi Tiết Sản Phẩm (Details Page)

### Tổng quan

- Trang chi tiết sản phẩm sẽ hiển thị hình ảnh, danh mục, giá, mô tả và một số thông tin bổ sung khác.
- Giao diện sử dụng HTML, CSS và Bootstrap từ snippet mẫu (Section 6, Section 7). Không dùng Tag Helper hoặc Razor Helper .NET ở phần giao diện cơ bản.
- Dữ liệu động được truyền vào qua *model* (`Product`). Các trường dữ liệu sẽ thay thế vào đúng vị trí trong HTML.


### Các bước thực hiện

#### 1. Định nghĩa Model cho View

```csharp
@model Product
```

- Model là một đối tượng sản phẩm (*Product*) nhận từ Controller.


#### 2. Thay thế dữ liệu động vào HTML/CSS

- **Tiêu đề:**

```html
<h1>@Model.Title</h1>
```

- **Tác giả:**

```html
<span>@Model.Author</span>
```

- **Hình ảnh sản phẩm:**

```html
<img src="@Model.ImageUrl" alt="Hình sản phẩm" />
```

- **Danh mục:**

```html
<span>@Model.Category.Name</span>
```

- **ISBN:**

```html
<span>@Model.ISBN</span>
```

- **Giá niêm yết (ListPrice):**

```html
<span class="text-decoration-line-through">@Model.ListPrice.ToString("c")</span>
```

- **Giá bán:**

```html
<span>@Model.Price100.ToString("c")</span>
```

- **Mô tả sản phẩm:**
    - Với dữ liệu là HTML Rich Text từ Model, sử dụng cú pháp:

```csharp
@Html.Raw(Model.Description)
```

    - Đảm bảo mô tả chuẩn xác cả định dạng: **đậm**, *nghiêng*, __gạch chân__ bằng HTML.


#### 3. Nút Điều Hướng

- Thêm nút/quay về trang chủ ở đầu trang:

```html
<a asp-action="Index" class="btn btn-secondary mb-4">Quay về Trang Chủ</a>
```


#### 4. Lưu ý UI

- Bố cục trang được định sẵn bằng Bootstrap nên chỉ cần thay thế biến động đúng vị trí.
- Có thể bỏ qua các phần chưa có chức năng như "Count" hoặc "Add to Cart".


#### 5. Xử lý hiển thị mô tả có HTML (Rich Text)

- Dùng `@Html.Raw(Model.Description)` để đảm bảo nội dung HTML mô tả được render đúng định dạng trên giao diện chi tiết.


### Mẫu đoạn mã chi tiết (khung cơ bản)

```csharp
@model Product

<a asp-action="Index" class="btn btn-secondary mb-4">Quay về Trang Chủ</a>

<div class="card border-3 p-3 shadow rounded">
    <img src="@Model.ImageUrl" class="card-img-top rounded" />
    <div class="card-body">
        <h3>@Model.Title</h3>
        <p>Tác giả: @Model.Author</p>
        <p>Danh mục: @Model.Category.Name</p>
        <p>ISBN: @Model.ISBN</p>
        <p class="text-muted text-decoration-line-through">@Model.ListPrice.ToString("c")</p>
        <p class="fw-bold">Giá chỉ: @Model.Price100.ToString("c")</p>
        <div>
            @Html.Raw(Model.Description)
        </div>
    </div>
</div>
```


### Ghi chú thêm

- Kiểm tra và đảm bảo các trường dữ liệu có giá trị khi truyền vào view.
- Khi cập nhật hoặc tùy chỉnh mô tả sản phẩm, hãy sử dụng HTML editor để định dạng phong phú trong nội dung.


### Liên kết thuật ngữ

**Liên kết:** [[Product]], [[Product Details]], [[Razor View]], [[Model]], [[ASP.NET MVC]], [[Bootstrap]], [[HTML.Raw]], [[Category]], [[Index Action]], [[Description]], [[View]]

