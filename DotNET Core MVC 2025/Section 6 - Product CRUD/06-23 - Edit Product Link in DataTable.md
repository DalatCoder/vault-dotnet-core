## DataTable – Thêm Nút Sửa (Edit) và Xóa (Delete) Cho Sản Phẩm

### Khái niệm

- Trong bảng sản phẩm sử dụng DataTables, bạn cần thêm các nút chức năng như **Sửa** (Edit) và **Xóa** (Delete) cho từng dòng sản phẩm.
- Việc này giúp thao tác trên từng sản phẩm trực quan, tối ưu trải nghiệm quản trị.


### Cách thực hiện Render Actions Column

#### Tùy chỉnh cột bằng `render` function

- Trong file `product.js`, thay vì chỉ hiển thị cột id, hãy sử dụng hàm `render` để in ra đoạn HTML mong muốn (button group).
- Sử dụng cú pháp template string (`) của ES6 để viết HTML multi-line thuận tiện.

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
    },
    width: "15%"
}
```

- **data** ở đây chính là id của sản phẩm, dùng để truyền vào đường dẫn hoặc hàm Delete.


#### Các bước bổ sung vào cấu hình DataTable

- Thêm cột này vào mảng `columns` trong hàm khởi tạo DataTable.
- Đảm bảo số lượng `<th>` trong HTML và `columns` trong JS tương ứng nhau.

```javascript
"columns": [
    { "data": "title", "width": "25%" },
    { "data": "isbn", "width": "15%" },
    { "data": "listPrice", "width": "10%" },
    { "data": "author", "width": "15%" },
    { "data": "category.name", "width": "20%" },
    {
        "data": "id",
        "render": function (data) {
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
        }, "width": "15%"
    }
]
```


#### Tùy chỉnh Table Header trong HTML

```html
<table id="tblData" class="table table-bordered table-striped" style="width:100%">
    <thead>
        <tr>
            <th>Tiêu đề</th>
            <th>ISBN</th>
            <th>Giá niêm yết</th>
            <th>Tác giả</th>
            <th>Danh mục</th>
            <th>Thao tác</th> <!-- Cột cho nút Sửa/Xóa -->
        </tr>
    </thead>
</table>
```


#### Lưu ý về template string

- Sử dụng dấu backtick ` để viết nhiều dòng HTML trong JS.
- Có thể interpolate biến trực tiếp bằng `${data}`.


### Cách hoạt động

- Khi bảng được load, cột cuối sẽ hiển thị nút Sửa/Xóa có icon theo đúng Bootstrap.
- Nhấn Sửa sẽ truy cập trang Upsert của sản phẩm ứng với id.
- Nhấn Xóa sẽ gọi hàm `Delete` (sẽ cài đặt logic xóa sản phẩm với modal xác nhận ở phần sau).


### Ghi chú thêm

- Phương pháp sử dụng hàm `render` của DataTables giúp bạn có thể "vẽ" mọi nội dung HTML cho từng dòng trong bảng.
- Có thể thêm sự kiện, modal popup, animation tùy ý trong đây.
- Thứ tự, kiểu icon, màu sắc đều tuân theo Bootstrap để đồng bộ với giao diện site.

**Liên kết:** [[DataTables]], [[AJAX]], [[Bootstrap Button]], [[Product Controller]], [[JavaScript Render Column]], [[Upsert Action]], [[CRUD Operations]]

