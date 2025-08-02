## Tích hợp DataTables.net để nâng cao chức năng bảng Product

### Khái niệm

Thay vì sử dụng bảng HTML thông thường, **DataTables.net** cung cấp các tính năng nâng cao cho bảng dữ liệu:

- **Search functionality**: Tìm kiếm trong bảng
- **Pagination**: Phân trang tự động
- **Sorting**: Sắp xếp theo cột
- **Third-party plugin**: Không cần implement thủ công

**DataTables.net** là giải pháp hoàn chỉnh với tất cả tính năng cơ bản, dễ tích hợp và sử dụng.

### Cách thực hiện

**Bước 1: Thêm CSS và JavaScript Files**

Trong file `_Layout.cshtml`, thêm CDN links:

```html
<!-- Trong phần <head> - CSS file -->
<link rel="stylesheet" href="https://cdn.datatables.net/1.11.5/css/jquery.dataTables.min.css" />

<!-- Trước thẻ đóng </body> - JavaScript file -->
<script src="https://cdn.datatables.net/1.11.5/js/jquery.dataTables.min.js"></script>
```

**Lưu ý:** CSS phải được thêm trong `<head>`, JavaScript thêm cuối trang sau jQuery.

**Bước 2: Tạo API Endpoint trong Controller**

Trong `ProductController.cs`, thêm API method:

```csharp
#region API CALLS

[HttpGet]
public IActionResult GetAll()
{
    List<Product> objProductList = _unitOfWork.Product
        .GetAll(includeProperties: "Category").ToList();
    
    return Json(new { data = objProductList });
}

#endregion
```

**Đặc điểm API Endpoint:**

- **MVC Architecture**: Hỗ trợ API sẵn có, không cần tạo Web API project riêng
- **Return JSON**: Trả về dữ liệu dạng JSON object với property `data`
- **Include Navigation**: Vẫn cần include Category để hiển thị tên category
- **HTTP GET**: Sử dụng GET method để retrieve dữ liệu


### Test API Endpoint

**Cách kiểm tra API hoạt động:**

1. **Chạy application**: Start project và copy base URL
2. **Navigate to endpoint**: Truy cập `/Admin/Product/GetAll`
3. **Kiểm tra JSON response**: Sẽ thấy dữ liệu JSON với structure:
```json
{
  "data": [
    {
      "id": 1,
      "title": "Fortune of Time",
      "author": "Billy Spark",
      "isbn": "SWD9999001",
      "listPrice": 99,
      "price": 90,
      "price50": 85,
      "price100": 80,
      "categoryId": 1,
      "category": {
        "id": 1,
        "name": "Action"
      },
      "imageURL": "/images/product/..."
    }
    // ... other products
  ]
}
```


### Ưu điểm của phương pháp này

**MVC Built-in API Support:**

- Không cần tạo separate API project
- Controller methods có thể return JSON data
- Tận dụng existing business logic và data access layer
- Đơn giản và nhanh chóng implement

**DataTables Integration:**

- **Ajax-based**: Load dữ liệu bất đồng bộ
- **Performance**: Không cần reload trang khi search/sort
- **User Experience**: Smooth interactions với bảng dữ liệu
- **Customizable**: Có thể tùy chỉnh giao diện và chức năng


### Chuẩn bị cho bước tiếp theo

**Video tiếp theo sẽ thực hiện:**

- Khởi tạo DataTable với JavaScript
- Cấu hình Ajax call đến API endpoint `/Admin/Product/GetAll`
- Setup columns mapping để hiển thị đúng dữ liệu
- Thêm action buttons (Edit, Delete) vào bảng

**Structure dữ liệu đã sẵn sàng:**

- ✅ API endpoint trả về JSON data
- ✅ Include Category navigation property
- ✅ CDN files đã được thêm vào layout
- ✅ Controller method tested và functional


### Ghi chú thêm

- **DataTables.net** rất mạnh mẽ và có nhiều options tùy chỉnh
- **jQuery dependency**: DataTables yêu cầu jQuery (đã có sẵn trong template)
- **CDN vs Local**: Sử dụng CDN cho development, có thể download local cho production
- **API Convention**: Return data trong object với property `data` là standard của DataTables
- **Performance**: Ajax loading giúp tối ưu hiệu suất khi có nhiều records
- **Browser Compatibility**: DataTables hỗ trợ hầu hết browsers hiện đại

**Liên kết:** [[Product Controller]], [[DataTables]], [[Ajax]], [[JSON API]], [[MVC Architecture]], [[Product Model]], [[Category Model]], [[Navigation Properties]], [[Include]], [[HTTP GET]], [[Bootstrap]], [[jQuery]], [[CDN]], [[API Endpoint]]

