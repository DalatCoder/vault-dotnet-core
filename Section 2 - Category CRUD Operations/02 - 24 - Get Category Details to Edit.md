## Tạo trang chỉnh sửa danh mục (Edit Category Page)

### Khái niệm

Trang **Edit** về cơ bản sẽ giống hệt trang **Create**, nhưng các trường **tên danh mục** và **thứ tự hiển thị** sẽ được điền sẵn dựa trên danh mục mà người dùng muốn chỉnh sửa. Điểm khác biệt chính là khi tạo danh mục mới, chúng ta không cần biết danh mục nào, nhưng khi chỉnh sửa, chúng ta cần **ID** để xác định danh mục cụ thể.

### Tạo Action Methods cho Edit

#### Cấu trúc cơ bản

Trong **Category Controller**, cần thêm cả action method **GET** và **POST** cho Edit:

```csharp
// GET action method cho Edit
public IActionResult Edit(int? id)
{
    if (id == null || id == 0)
    {
        return NotFound();
    }
    
    Category? categoryFromDb = _db.Categories.Find(id);
    
    if (categoryFromDb == null)
    {
        return NotFound();
    }
    
    return View(categoryFromDb);
}

// POST action method cho Edit
[HttpPost]
public IActionResult Edit(Category obj)
{
    // Logic xử lý cập nhật sẽ được thêm sau
}
```


#### Xử lý tham số ID

- **ID parameter**: Nhận ID của danh mục cần chỉnh sửa qua tham số `int? id`
- **Validation**: Kiểm tra ID có hợp lệ không (không null và khác 0)
- **Error handling**: Trả về `NotFound()` nếu ID không hợp lệ hoặc không tìm thấy danh mục


### Ba cách lấy dữ liệu từ Database

#### 1. Sử dụng Find() - Phương pháp đơn giản nhất

```csharp
Category? categoryFromDb1 = _db.Categories.Find(id);
```

- **Ưu điểm**: Đơn giản, chỉ cần truyền primary key
- **Hạn chế**: Chỉ hoạt động với primary key, không thể tùy chỉnh điều kiện


#### 2. Sử dụng FirstOrDefault() - Linh hoạt hơn

```csharp
Category? categoryFromDb2 = _db.Categories.FirstOrDefault(u => u.Id == id);
```

- **Ưu điểm**: Có thể sử dụng LINQ expressions, tìm kiếm theo bất kỳ trường nào
- **Ví dụ mở rộng**: `FirstOrDefault(u => u.Name.Contains("search_term"))`


#### 3. Sử dụng Where() kết hợp FirstOrDefault()

```csharp
Category? categoryFromDb3 = _db.Categories
    .Where(u => u.Id == id)
    .FirstOrDefault();
```

- **Ưu điểm**: Phù hợp khi cần thêm nhiều điều kiện lọc phức tạp
- **Sử dụng**: Khi có nhiều tính toán hoặc filtering trước khi lấy kết quả


### Cấu hình Route ID trong View

#### Truyền ID qua asp-route-id

Trong trang Index, cần cập nhật link Edit để truyền ID:

```html
<a asp-controller="Category" asp-action="Edit" asp-route-id="@obj.Id"
   class="btn btn-primary mx-2">
    <i class="bi bi-pencil"></i> Edit
</a>
```


#### Giải thích:

- `asp-route-id="@obj.Id"`: Truyền ID của danh mục vào route parameter
- Tương đương với URL: `/Category/Edit/2` (với ID = 2)
- Parameter `id` trong action method sẽ tự động nhận giá trị này


### Kiểm tra hoạt động

Khi nhấn nút Edit cho danh mục "SciFi" có ID = 2:

1. URL sẽ là: `/Category/Edit/2`
2. Action method nhận `id = 2`
3. Database query tìm category có ID = 2
4. Truyền object category này đến View để hiển thị

### Ghi chú quan trọng

- **Null safety**: Sử dụng `int?` cho parameter ID và kiểm tra null
- **Error handling**: Luôn validate ID trước khi query database
- **Performance**: `Find()` thường nhanh nhất cho primary key lookup
- **Flexibility**: `FirstOrDefault()` linh hoạt hơn cho custom conditions


### Bước tiếp theo

Sau khi hoàn thiện action method, bước tiếp theo sẽ là tạo View để hiển thị form Edit với dữ liệu đã được populate từ database.

**Liên kết:** [[Category Controller]], [[Entity Framework Core]], [[Action Methods]], [[Route Parameters]], [[LINQ Queries]], [[Find Method]], [[FirstOrDefault]], [[Primary Key]], [[NotFound Result]]

