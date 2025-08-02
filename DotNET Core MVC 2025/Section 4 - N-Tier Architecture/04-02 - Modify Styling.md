## Tùy chỉnh Bootstrap Theme và Cải thiện Giao diện

### Thay đổi Bootstrap Theme từ Bootswatch

- **Chuyển từ theme Luxe sang Sandstone** trên trang web Bootswatch
- **Cách thực hiện**:
    - Truy cập Bootswatch và chọn theme Sandstone
    - Download file Bootstrap CSS của theme
    - Copy toàn bộ nội dung file CSS
    - Vào dự án tại `wwwroot/css`, xóa theme Luxe cũ
    - Paste theme Sandstone mới vào file CSS


### Tùy chỉnh màu sắc Button và CSS

#### Nguồn tài liệu

- Download các snippet CSS từ **dotnetmastery.com**
- Vào phần course detail để lấy tất cả snippets
- File cần sử dụng: `site.css` trong section 4


#### Nội dung tùy chỉnh

- **Override các Bootstrap classes** thay vì tạo class mới:
    - `btn-outline`
    - `bg-success`
    - `btn-primary`
    - `btn-success`
- Copy toàn bộ CSS từ snippet vào file `site.css`
- **Xóa hết nội dung cũ** trong `site.css` và paste CSS mới


### Cải tiến giao diện Create Category

#### Cấu trúc HTML mới với Bootstrap Classes

```html
<div class="card shadow border-0 mt-4">
    <div class="card-header">
        <div class="row">
            <div class="col-12 text-center">
                <h2 class="text-white py-2">Create Category</h2>
            </div>
        </div>
    </div>
    <div class="card-body p-4">
        <form class="row">
            <div class="form-floating py-2 col-12">
                <input class="form-control border-0 shadow ms-2" />
                <label>Category Name</label>
            </div>
            <div class="form-floating py-2 col-12">
                <input class="form-control border-0 shadow ms-2" />
                <label>Display Order</label>
            </div>
        </form>
    </div>
</div>
```


#### Các Bootstrap Classes được sử dụng

- **Card structure**: `card`, `shadow`, `border-0`, `mt-4`
- **Card header**: `card-header`, `text-white`, `py-2`
- **Card body**: `card-body`, `p-4`
- **Form styling**: `form-floating`, `form-control`, `border-0`, `shadow`
- **Button styling**: `btn-outline-primary` (thay cho `btn-secondary`)


### Điều chỉnh Layout

#### Vị trí file CSS

- Di chuyển `site.css` xuống **cuối cùng** trong `_Layout.cshtml`
- Đảm bảo CSS tùy chỉnh được load sau Bootstrap để override đúng cách


#### Lưu ý quan trọng

- **Form-floating structure**: Input phải đặt trước Label
- Cần **rebuild project** sau khi thay đổi CSS
- Có thể cần **restart application** để thấy thay đổi
- Sử dụng hard reload nếu cần thiết


### Kết quả

- **Giao diện mới**: Màu sắc hài hòa hơn với theme Sandstone
- **Card design**: Tạo hiệu ứng đẹp mắt với shadow và border
- **Form styling**: Sử dụng form-floating cho trải nghiệm người dùng tốt hơn
- **Button colors**: Màu sắc tùy chỉnh phù hợp với theme tổng thể


### Ghi chú thêm

- Tất cả **sử dụng Bootstrap classes** có sẵn, không tạo custom classes mới
- Phương pháp này giống như **Bootswatch theme** - override các class có sẵn
- CSS được áp dụng cho tất cả các trang khác trong video tiếp theo

**Liên kết:** [[Bootstrap]], [[Bootswatch]], [[CSS Override]], [[Card Component]], [[Form Styling]], [[Theme Customization]], [[Site Layout]], [[ASP.NET Core MVC]]

