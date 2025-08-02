## Thêm nút Create New Category với Bootstrap Layout

### Mục tiêu của bài học

Trong trang danh sách categories, hiện tại chúng ta chỉ hiển thị tất cả categories. Để người dùng có thể tạo category mới, cần thêm một nút sẽ dẫn đến trang tạo category mới.

### Tạo cấu trúc Bootstrap Container

Thay vì sử dụng thẻ H1 đơn giản, chúng ta sẽ tạo layout chuyên nghiệp hơn:

```html
<div class="container">
    <div class="row pt-4">
        <!-- Nội dung sẽ được thêm vào đây -->
    </div>
</div>
```

**Giải thích các class Bootstrap:**

- `container`: Tạo container chính cho nội dung
- `row`: Tạo hàng trong Bootstrap grid system
- `pt-4`: Padding top 4 đơn vị


### Hệ thống Grid 12 cột của Bootstrap

Bootstrap chia mỗi row thành **12 cột (columns)**. Chúng ta sẽ sử dụng:

- **6 cột bên trái**: Hiển thị tiêu đề "Category List"
- **6 cột bên phải**: Hiển thị nút "Create New Category"


### Tạo tiêu đề bên trái (6 cột đầu)

```html
<div class="col-6">
    <h2 class="text-primary">Category List</h2>
</div>
```

**Class được sử dụng:**

- `col-6`: Chiếm 6 cột trong grid system
- `text-primary`: Sử dụng màu chính của theme (màu đen trong Lux theme)


### Tạo nút Create New Category bên phải (6 cột còn lại)

```html
<div class="col-6 text-end">
    <a href="#" class="btn btn-primary">
        <i class="bi bi-plus-circle"></i> Create New Category
    </a>
</div>
```

**Các thành phần quan trọng:**

- `col-6`: Chiếm 6 cột còn lại
- `text-end`: Căn nội dung về bên phải
- `btn btn-primary`: Tạo button với màu chính của theme
- `bi bi-plus-circle`: Icon dấu cộng từ Bootstrap Icons


### Thêm padding bottom cho tổng thể

```html
<div class="row pt-4 pb-3">
```

**Class bổ sung:**

- `pb-3`: Padding bottom 3 đơn vị để tạo khoảng cách phía dưới


### Code hoàn chỉnh

```html
<div class="container">
    <div class="row pt-4 pb-3">
        <div class="col-6">
            <h2 class="text-primary">Category List</h2>
        </div>
        <div class="col-6 text-end">
            <a href="#" class="btn btn-primary">
                <i class="bi bi-plus-circle"></i> Create New Category
            </a>
        </div>
    </div>
    <!-- Bảng hiển thị categories ở đây -->
</div>
```


### Kết quả đạt được

- Layout chuyên nghiệp với 2 cột cân đối
- Tiêu đề "Category List" ở bên trái với màu chính của theme
- Nút "Create New Category" ở bên phải với icon plus
- Khoảng cách hợp lý giữa các phần tử
- Sẵn sàng để liên kết đến action tạo category mới


### Bước tiếp theo

Nút "Create New Category" hiện tại chỉ có href="\#" placeholder. Trong video tiếp theo sẽ định nghĩa controller action cụ thể để xử lý việc tạo category mới.

**Liên kết:** [[Bootstrap Grid System]], [[Bootstrap Classes]], [[Bootstrap Icons]], [[MVC Controller Actions]], [[Category Management]], [[HTML Layout]], [[CSS Styling]]

