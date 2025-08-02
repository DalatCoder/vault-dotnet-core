## Xử lý Delete Product và Xóa Hình Ảnh đi kèm trong ASP.NET Core MVC

### Khái niệm

- Khi xóa sản phẩm trong module quản trị (*Admin*) của ứng dụng, cần thực thi:
    - Xóa bản ghi sản phẩm khỏi database
    - Đồng thời xóa file hình ảnh vật lý khỏi thư mục lưu trữ (`wwwroot/images/product`)
    - Đảm bảo trả về kết quả (JSON) nhằm update giao diện (DataTable) một cách đồng bộ, hiện đại


### Triển khai API Delete trong Product Controller

#### Mã nguồn mẫu

```csharp
// API xoá sản phẩm (trả về Json response)
[HttpPost] // Không dùng HttpDelete vì JavaScript dùng Ajax POST thông thường
public IActionResult Delete(int id)
{
    var productToBeDeleted = _unitOfWork.Product.Get(u => u.Id == id);
    if (productToBeDeleted == null)
    {
        return Json(new { success = false, message = "Lỗi khi xoá sản phẩm." });
    }

    // Xoá ảnh khỏi wwwroot/images/product/
    string wwwRootPath = _webHostEnvironment.WebRootPath;
    string oldImagePath = Path.Combine(wwwRootPath, productToBeDeleted.ImageURL.TrimStart('\\'));
    if (System.IO.File.Exists(oldImagePath))
    {
        System.IO.File.Delete(oldImagePath);
    }

    // Xoá dữ liệu sản phẩm khỏi database
    _unitOfWork.Product.Remove(productToBeDeleted);
    _unitOfWork.Save();

    return Json(new { success = true, message = "Xoá thành công." });
}
```

- **Nhận diện Id** sản phẩm từ tham số truyền vào
- **Kiểm tra tồn tại**: Nếu không tồn tại, trả về lỗi JSON để UI xử lý
- **Lấy đường dẫn và xóa file ảnh** tương ứng
- **Xoá bản ghi trong database** thông qua repository pattern (`UnitOfWork`)
- **Phản hồi lại kết quả** thao tác bằng `Json` (success: true/false)


#### Lưu ý

- Sử dụng đúng path `wwwRootPath` và `TrimStart('\\')` để chắc chắn xóa đúng file theo cấu trúc thư mục
- Controller nên annotate `[HttpPost]` (hoặc `[HttpDelete]` nếu client gọi đúng REST method), nhưng DataTables AJAX thường POST


### Tích hợp với DataTable AJAX và UI

- Viết hàm JavaScript thực hiện call API khi click vào nút Delete
- Sau khi thành công, cập nhật lại dữ liệu DataTable mà không cần reload trang

```javascript
function Delete(url) {
    $.ajax({
        type: 'POST',
        url: url,
        success: function (data) {
            if (data.success) {
                $('#tblData').DataTable().ajax.reload();
                toastr.success(data.message); // Hoặc hiện thông báo thành công
            } else {
                toastr.error(data.message); // Thông báo lỗi nếu có
            }
        }
    });
}
```

- Thường kết hợp với **SweetAlert** để xác nhận trước khi thực hiện (sẽ trình bày ở bài kế tiếp)


### Loại bỏ Delete View truyền thống

- Không sử dụng view xóa truyền thống (`Delete.cshtml`)
- Toàn bộ thao tác xoá được thực hiện qua API và Ajax
- Đơn giản hóa trải nghiệm quản trị, đồng thời bảo mật và tránh thao tác nhầm


### Tóm tắt quy trình

- Click Delete trên bảng ⇒ JS hiện dialog xác nhận (SweetAlert)
- Nếu xác nhận, gọi AJAX đến API `/Admin/Product/Delete?id=...`
- API xóa file hình và sản phẩm, trả JSON kết quả
- JS reload lại DataTable hoặc hiển thị thông điệp tương ứng


### Liên kết

[[Product Controller]], [[API Endpoint]], [[DataTables]], [[AJAX]], [[UnitOfWork]], [[Xoá file tĩnh]], [[wwwroot]], [[SweetAlert]], [[JavaScript]], [[Confirmation Dialog]], [[Image Management]], [[Repository Pattern]], [[Json Response]], [[Upsert]]

