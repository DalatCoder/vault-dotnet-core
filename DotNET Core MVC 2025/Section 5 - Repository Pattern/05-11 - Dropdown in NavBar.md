## Thêm Dropdown Menu vào Navigation Bar

### Chuẩn bị cho tính năng mới

Trước khi thêm **Product** và thực hiện các thao tác CRUD, cần tổ chức lại navigation để dễ quản lý. Thay vì để Category riêng lẻ, sẽ nhóm các chức năng quản lý vào một dropdown menu.

### Sử dụng Bootstrap Dropdown

#### Bước 1: Tìm component trên Bootstrap

- Truy cập **getbootstrap.com**
- Vào Documentation
- Tìm kiếm **"navbar"**
- Chọn dropdown component phù hợp


#### Bước 2: Copy HTML code

Lấy cấu trúc dropdown từ Bootstrap documentation:

```html
<li class="nav-item dropdown">
  <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">
    Content Management
  </a>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" asp-area="Admin" asp-controller="Category" asp-action="Index">Category</a></li>
    <li><hr class="dropdown-divider"></li>
    <!-- Thêm các items khác sau -->
  </ul>
</li>
```


#### Bước 3: Thêm vào Layout

- Mở file `_Layout.cshtml`
- Tìm phần navigation (`<nav>`)
- Paste dropdown code sau các `<li>` elements hiện có


### Xử lý CSS Styling

#### Vấn đề màu sắc

- Ban đầu dropdown hiển thị màu xanh do CSS conflicts
- Cần loại bỏ hoặc comment out CSS gây xung đột


#### Giải pháp styling

```css
/* Comment out conflicting CSS */
/* .nav-link { color: blue; } */
```

- Sử dụng class `dropdown-item` của Bootstrap
- Đảm bảo text color phù hợp (dark text)


### Cấu trúc hoàn chỉnh

#### Navigation structure

- **Content Management** (dropdown toggle)
    - Category (link đến Admin/Category/Index)
    - Divider line
    - *Chỗ cho Product và các chức năng khác*


#### Lợi ích của cách tổ chức này

- **Scalable**: Dễ thêm Product, Order management, v.v.
- **User-friendly**: Nhóm các chức năng quản lý lại với nhau
- **Clean UI**: Navigation không bị cluttered khi thêm nhiều tính năng


### Ghi chú kỹ thuật

#### Bootstrap Dependencies

- Cần đảm bảo Bootstrap JavaScript được load đúng cách
- Dropdown functionality cần `data-bs-toggle="dropdown"`


#### Area Routing

- Vẫn sử dụng `asp-area="Admin"` cho Category
- Đảm bảo routing hoạt động đúng trong dropdown context


#### Hot Reload

- Sau khi thay đổi, cần rebuild project
- Hard reload browser để thấy thay đổi CSS


### Chuẩn bị cho bước tiếp theo

Dropdown "Content Management" đã sẵn sàng để thêm:

- **Product management**
- **Order management**
- **User management**
- Các chức năng admin khác

**Liên kết:** [[Bootstrap]], [[Navigation Bar]], [[Dropdown Menu]], [[_Layout.cshtml]], [[CSS Styling]], [[Area Routing]], [[Admin Area]], [[CategoryController]], [[Hot Reload]], [[Content Management]]

