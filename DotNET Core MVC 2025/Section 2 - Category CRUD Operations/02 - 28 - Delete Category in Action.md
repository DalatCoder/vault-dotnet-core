## Hoàn thiện chức năng xóa danh mục (Delete Category View)

### Tạo Delete View

#### Thao tác tạo view

- **Right-click** trên Delete action → **Add View** → **Empty View**
- **Đặt tên**: `Delete.cshtml`
- **Cấu trúc**: Sao chép hoàn toàn từ Edit view làm template


#### Cấu hình view Delete

```html
@model Category

@{
    ViewData["Title"] = "Delete Category";
}

<div class="card shadow border-0 mt-4">
    <div class="card-header bg-secondary bg-gradient ml-0 py-3">
        <div class="row">
            <div class="col-12 text-center">
                <h2 class="text-white py-2">Delete Category</h2>
            </div>
        </div>
    </div>
    <div class="card-body p-4">
        <form method="post">
            <input asp-for="Id" type="hidden" />
            <div class="border p-3">
                <div class="form-floating py-2 col-12">
                    <input asp-for="Name" class="form-control border-0 shadow" disabled />
                    <label asp-for="Name" class="ms-2"></label>
                </div>
                <div class="form-floating py-2 col-12">
                    <input asp-for="DisplayOrder" class="form-control border-0 shadow" disabled />
                    <label asp-for="DisplayOrder" class="ms-2"></label>
                </div>
                <div class="row pt-2">
                    <div class="col-6 col-md-3">
                        <button type="submit" class="btn btn-danger form-control">Delete Record</button>
                    </div>
                    <div class="col-6 col-md-3">
                        <a asp-controller="Category" asp-action="Index" class="btn btn-outline-primary border form-control">
                            Back to List
                        </a>
                    </div>
                </div>
            </div>
        </form>
    </div>
</div>
```


### Những thay đổi quan trọng so với Edit View

#### Loại bỏ validation

- **Lý do**: Người dùng không thể chỉnh sửa dữ liệu
- **Thao tác**: Xóa tất cả `<span asp-validation-for="..." class="text-danger"></span>`


#### Vô hiệu hóa các trường input

- **Thuộc tính**: Thêm `disabled` vào tất cả input fields
- **Mục đích**: Chỉ cho phép xem, không cho phép chỉnh sửa
- **Kết quả**: Các trường hiển thị dữ liệu nhưng không thể thay đổi


#### Thay đổi giao diện

- **Tiêu đề**: "Edit Category" → "Delete Category"
- **Nút action**: "Update" → "Delete Record"
- **Màu nút**: `btn-primary` → `btn-danger` (màu đỏ cảnh báo)


### Cấu hình Route ID trong Index View

#### Cập nhật link Delete

```html
<a asp-controller="Category" asp-action="Delete" asp-route-id="@obj.Id"
   class="btn btn-danger mx-2">
    <i class="bi bi-trash-fill"></i> Delete
</a>
```


#### Cơ chế hoạt động

- **Route parameter**: `asp-route-id="@obj.Id"` truyền ID qua URL
- **URL pattern**: `/Category/Delete/7` (với ID = 7)
- **Controller mapping**: Tự động bind vào parameter `id` của Delete action


### Hidden Field cho ID

#### Thêm hidden input

```html
<input asp-for="Id" type="hidden" />
```


#### Tầm quan trọng của hidden field

- **Đảm bảo**: ID được truyền trong POST request
- **Tránh magic**: Code rõ ràng, không dựa vào automatic binding
- **Best practice**: Áp dụng cho cả Edit và Delete actions
- **An toàn**: Đảm bảo không có lỗi khi property name khác `Id`


#### Khuyến nghị sử dụng

```html
<!-- Trong Edit view -->
<input asp-for="Id" type="hidden" />

<!-- Trong Delete view -->
<input asp-for="Id" type="hidden" />
```


### Kiểm tra hoạt động

#### Test case thực tế

- **Danh mục**: "Action" với ID = 7, Display Order = 7
- **Thao tác**: Click Delete → hiển thị trang xác nhận
- **Giao diện**: Tất cả trường bị disable, chỉ hiển thị thông tin
- **Xác nhận**: Click "Delete Record" → record bị xóa khỏi database


#### Xác minh trong SQL Server

- **Trước khi xóa**: Record "test" tồn tại trong bảng Categories
- **Sau khi xóa**: Record "test" không còn trong database
- **Kết luận**: Chức năng xóa hoạt động chính xác


### Hoàn tất CRUD Operations

#### Các chức năng đã implement

- **Create**: Tạo danh mục mới
- **Read**: Hiển thị danh sách danh mục (Index)
- **Update**: Chỉnh sửa danh mục (Edit)
- **Delete**: Xóa danh mục


#### Tính năng của Delete

- **User-friendly**: Hiển thị thông tin trước khi xóa
- **Safe**: Các trường bị disable, không thể chỉnh sửa nhầm
- **Confirmatory**: Yêu cầu xác nhận trước khi thực hiện
- **Complete**: Xóa hoàn toàn khỏi database


### Ghi chú quan trọng

- **Disabled fields**: Vẫn hiển thị dữ liệu nhưng không cho phép chỉnh sửa
- **Hidden ID field**: Best practice cho tất cả form submissions
- **Danger button**: Sử dụng màu đỏ để cảnh báo hành động nguy hiểm
- **Database sync**: Thay đổi được reflect ngay lập tức trong database

**Liên kết:** [[Category Controller]], [[Delete View]], [[CRUD Operations]], [[Hidden Field]], [[Route Parameters]], [[Entity Framework Core]], [[Remove Method]], [[Disabled Input]], [[Button Styling]], [[Form Submission]], [[Database Operations]]

