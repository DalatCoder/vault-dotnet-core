## Thêm nút chỉnh sửa và xóa cho danh mục (Category)

### Khái niệm

Trong bước này, chúng ta sẽ bổ sung chức năng chỉnh sửa và xóa danh mục bằng cách thêm các nút **Edit** và **Delete** vào trang danh sách danh mục (Index page). Các nút này sẽ sử dụng biểu tượng từ Bootstrap Icons và liên kết đến các action tương ứng trong Category Controller.

### Chuẩn bị biểu tượng (Icons)

- Sử dụng **Bootstrap Icons** để tạo giao diện trực quan
- Biểu tượng chỉnh sửa: **pencil icon** (biểu tượng bút chì)
- Biểu tượng xóa: **trash fill icon** (biểu tượng thùng rác)


### Cách thực hiện

#### Tạo cấu trúc HTML cho các nút action

Trong trang **Index view** của Category, thêm một cột mới chứa các nút:

```html
<td>
    <div class="w-75 btn-group" role="group">
        <!-- Nút Edit -->
        <a asp-controller="Category" asp-action="Edit" 
           class="btn btn-primary mx-2">
            <i class="bi bi-pencil"></i> Edit
        </a>
        
        <!-- Nút Delete -->
        <a asp-controller="Category" asp-action="Delete" 
           class="btn btn-danger mx-2">
            <i class="bi bi-trash-fill"></i> Delete
        </a>
    </div>
</td>
```


#### Phân tích các class Bootstrap được sử dụng

- `w-75`: Thiết lập chiều rộng 75% cho container
- `btn-group`: Nhóm các nút lại với nhau
- `role="group"`: Thuộc tính accessibility cho screen reader
- `btn btn-primary`: Tạo nút với màu xanh chính (primary)
- `btn btn-danger`: Tạo nút với màu đỏ cảnh báo (danger)
- `mx-2`: Thêm margin trái và phải với kích thước 2


#### Liên kết action (Action Links)

- **Controller**: `Category` - Cả hai nút đều liên kết đến Category Controller
- **Action cho Edit**: `Edit` - Sẽ được tạo trong video tiếp theo
- **Action cho Delete**: `Delete` - Sẽ được tạo trong video tiếp theo


### Ghi chú quan trọng

- Các action `Edit` và `Delete` trong Category Controller chưa được tạo ra trong bước này
- Giao diện người dùng (UI) đã hoàn thiện và sẵn sàng để tích hợp với các action backend
- Sử dụng Bootstrap Icons giúp tạo giao diện chuyên nghiệp và dễ nhận biết


### Kết quả

Sau khi hoàn thành, mỗi dòng danh mục trong bảng sẽ có hai nút:

- Nút **Edit** (màu xanh) với biểu tượng bút chì
- Nút **Delete** (màu đỏ) với biểu tượng thùng rác

**Liên kết:** [[Category Controller]], [[Bootstrap Icons]], [[ASP.NET Core MVC]], [[Index View]], [[Action Links]], [[Edit Action]], [[Delete Action]]

