## Thêm Thông báo Xác nhận Xóa với SweetAlert2 cho Product

### Khái niệm

- Khi xóa sản phẩm (Product), để tránh thao tác nhầm, cần thêm hộp thoại xác nhận (confirmation dialog) cho người dùng.
- Dùng **SweetAlert2** để hiển thị popup xác nhận xóa với giao diện đẹp, hiện đại.
- Sau khi xác nhận xóa, thực hiện xóa bằng **Ajax**, cập nhật trạng thái cho DataTable mà không cần reload trang.


### Cách thực hiện

#### 1. Thêm SweetAlert2 qua CDN

- Vào file `_Layout.cshtml`, thêm link CDN của SweetAlert2 bên dưới các file script/js khác.

```html
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
```


#### 2. Triển khai hàm Delete trong product.js

```javascript
var dataTable;

$(document).ready(function () {
    loadDataTable();
});

function loadDataTable() {
    dataTable = $('#tblData').DataTable({
        // ... các config columns ...
    });
}

// Hàm xác nhận và xóa bằng Ajax + SweetAlert2
function Delete(url) {
    Swal.fire({
        title: "Bạn có chắc muốn xóa?",
        text: "Không thể hoàn tác sau khi xóa sản phẩm này!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Đúng, xóa đi!",
        cancelButtonText: "Hủy"
    }).then((result) => {
        if (result.isConfirmed) {
            $.ajax({
                url: url,
                type: 'DELETE',
                success: function (data) {
                    if (data.success) {
                        toastr.success(data.message);
                        dataTable.ajax.reload();
                    } else {
                        toastr.error(data.message);
                    }
                }
            });
        }
    });
}
```


#### 3. Gọi hàm Delete trong cột Action của DataTable

- Sửa cấu hình column Action để dùng onClick:

```javascript
{
    data: "id",
    render: function (data) {
        return `
          <div class="w-75 btn-group" role="group">
              <a href="/Admin/Product/Upsert?id=${data}" class="btn btn-primary mx-2">
                  <i class="bi bi-pencil-square"></i> Sửa
              </a>
              <a onClick="Delete('/Admin/Product/Delete?id=${data}')" class="btn btn-danger mx-2">
                  <i class="bi bi-trash-fill"></i> Xóa
              </a>
          </div>
        `;
    }, width: "15%"
}
```

- **Lưu ý:** Bỏ href ở nút xóa, thay bằng `onClick="Delete(url)"`


#### 4. Tổng quan xử lý

- Khi bấm nút xóa, SweetAlert2 hiện cảnh báo xác nhận.
- Nếu xác nhận "Đúng, xóa đi!", hệ thống sẽ gửi Ajax DELETE tới API.
- Nếu xóa thành công, Toastr hiển thị thông báo và DataTable tự động reload (không cần reload trang).
- Nếu hủy hoặc có lỗi, không có thay đổi hoặc hiển thị lỗi tương ứng.
---

### Ghi chú thêm

- **SweetAlert2** cung cấp UI xác nhận hiện đại, bảo vệ khỏi thao tác xóa nhầm.
- **toastr** dùng cho thông báo (success/error) nhanh gọn ở góc màn hình.
- **dataTable.ajax.reload()** giúp reload lại bảng mà không phải reload trang, giữ trải nghiệm liền mạch.
- Đảm bảo biến `dataTable` khai báo ở phạm vi toàn cục để hàm Delete truy cập được.

**Liên kết:** [[SweetAlert2]], [[Ajax]], [[DataTables]], [[Toastr]], [[Product Controller]], [[Xác nhận xóa]], [[Repository Pattern]], [[Upsert]], [[wwwroot]], [[Bootstrap]]

