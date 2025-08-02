## Tạo JavaScript cho DataTables với Ajax Request

### Khái niệm

Để sử dụng **DataTables** với dữ liệu từ API, cần tạo file JavaScript riêng để thực hiện **Ajax request** và cấu hình bảng dữ liệu. Quá trình này yêu cầu mapping chính xác giữa column names trong DataTables và property names trong JSON response từ API.

### Cách thực hiện

**Bước 1: Tạo file product.js**

Trong thư mục `wwwroot/js`, tạo file `product.js`:

```javascript
$(document).ready(function () {
    loadDataTable();
});

function loadDataTable() {
    var dataTable = $('#tblData').DataTable({
        "ajax": {
            "url": "/Admin/Product/GetAll"
        },
        "columns": [
            { "data": "title", "width": "25%" },
            { "data": "isbn", "width": "15%" },
            { "data": "listPrice", "width": "10%" },
            { "data": "author", "width": "20%" },
            { "data": "category.name", "width": "15%" }
        ]
    });
}
```

**Bước 2: Cập nhật Index.cshtml**

Loại bỏ foreach loop và sử dụng DataTables:

```html
<table id="tblData" class="table table-bordered table-striped" style="width:100%">
    <thead>
        <tr>
            <th>Title</th>
            <th>ISBN</th>
            <th>List Price</th>
            <th>Author</th>
            <th>Category</th>
        </tr>
    </thead>
</table>

@section Scripts {
    <script src="~/js/product.js"></script>
}
```


### Xử lý lỗi Property Names

**Lỗi thường gặp:**

- **"Requested unknown parameter"**: Tên property trong DataTables không khớp với JSON response
- **Case sensitive**: Phải match chính xác với API response

**Cách kiểm tra API Response:**

- Truy cập `/Admin/Product/GetAll` trong browser
- Sử dụng JSON formatter để xem cấu trúc dữ liệu
- Copy exact property names từ JSON response

**Ví dụ JSON Response:**

```json
{
  "data": [
    {
      "title": "Fortune of Time",
      "isbn": "SWD9999001", 
      "listPrice": 99,
      "author": "Billy Spark",
      "category": {
        "name": "Action"
      }
    }
  ]
}
```


### Mapping Properties chính xác

**Column Configuration:**

```javascript
"columns": [
    { "data": "title" },        // Chính xác: "title" (lowercase)
    { "data": "isbn" },         // Chính xác: "isbn" (lowercase)  
    { "data": "listPrice" },    // Chính xác: "listPrice" (camelCase)
    { "data": "author" },       // Chính xác: "author" (lowercase)
    { "data": "category.name" } // Navigation property: "category.name"
]
```

**Lưu ý quan trọng:**

- Property names phải **case sensitive** match với JSON
- Navigation properties sử dụng **dot notation**: `category.name`
- Không được sử dụng tên khác với API response


### Cấu hình DataTables

**Các thành phần chính:**

- **Ajax configuration**: Chỉ định URL endpoint để load dữ liệu
- **Columns mapping**: Map JSON properties với table columns
- **Width settings**: Định nghĩa chiều rộng cho từng cột
- **Table ID**: Sử dụng `#tblData` để target table element

**Document Ready Pattern:**

```javascript
$(document).ready(function () {
    loadDataTable();  // Khởi tạo DataTable khi DOM loaded
});
```


### Cải thiện giao diện

**Table styling:**

- Thêm `style="width:100%"` cho table element
- Sử dụng Bootstrap classes: `table table-bordered table-striped`
- Cấu hình width cho từng column trong JavaScript

**Column width distribution:**

- Title: 25%
- ISBN: 15%
- List Price: 10%
- Author: 20%
- Category: 15%
- **Còn lại 15%**: Dành cho action buttons (sẽ thêm trong video tiếp theo)


### Loại bỏ code không cần thiết

**Trong Index.cshtml:**

- Xóa `@foreach` loop
- Xóa `<tbody>` section
- DataTables sẽ tự động generate table body
- Chỉ giữ lại `<thead>` với column headers


### Chuẩn bị cho bước tiếp theo

**Video tiếp theo sẽ thêm:**

- Custom buttons (Edit, Delete) trong cột cuối
- Action column với 15% width còn lại
- Button formatting và event handling
- Integration với existing Upsert và Delete actions


### Ghi chú thêm

- **Ajax loading**: DataTables tự động thực hiện Ajax call và populate dữ liệu
- **Error debugging**: Kiểm tra browser console để xem lỗi property mapping
- **JSON validation**: Sử dụng JSON formatter để verify API response structure
- **Case sensitivity**: Đây là nguyên nhân chính gây lỗi DataTables
- **Navigation properties**: Sử dụng dot notation để access nested objects
- **Performance**: Ajax loading cải thiện hiệu suất cho large datasets

**Liên kết:** [[DataTables]], [[Ajax]], [[Product Controller]], [[JSON API]], [[jQuery]], [[Navigation Properties]], [[Property Mapping]], [[JavaScript]], [[wwwroot]], [[API Endpoint]], [[Bootstrap]], [[Table Styling]]

