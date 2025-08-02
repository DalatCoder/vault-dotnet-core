## Kiểm tra và tối ưu chức năng chỉnh sửa danh mục (Edit Category)

### Kiểm tra hoạt động của Edit functionality

#### Test cập nhật dữ liệu

- **Thao tác test**: Chỉnh sửa danh mục "Action" thành "Action 111" và display order thành 5
- **Kết quả**: Cập nhật thành công, dữ liệu được lưu vào database
- **Kiểm tra ngược**: Cập nhật lại về giá trị ban đầu cũng hoạt động tốt
- **Kết luận**: Chức năng Edit đang hoạt động như mong đợi


### Vấn đề quan trọng về ID trong Model Binding

#### Cơ chế tự động populate ID

```csharp
// Khi debug, obj.ID được populate tự động
public IActionResult Edit(Category obj)
{
    // obj.ID có giá trị mặc dù không có hidden field
}
```


#### Lý do ID được populate

- **Magic của ASP.NET Core**: Framework tự động bind ID từ route parameter
- **Route parameter**: URL `/Category/Edit/2` tự động map với property `ID` trong model
- **Điều kiện**: Chỉ hoạt động khi tên property là **chính xác** `ID`


### Trường hợp cần Hidden Field cho ID

#### Khi nào cần thêm hidden field

```html
<!-- Bắt buộc khi property không phải là "ID" -->
<input asp-for="CategoryId" type="hidden" />
<input asp-for="CustomId" type="hidden" />
```


#### Ví dụ code cần thiết

```html
<!-- Trong trường hợp property name khác "ID" -->
@model Category

<form method="post">
    <input asp-for="ID" type="hidden" />
    <!-- Hoặc nếu property là CategoryId -->
    <input asp-for="CategoryId" type="hidden" />
    
    <div class="form-floating py-2 col-12">
        <input asp-for="Name" class="form-control border-0 shadow" />
        <label asp-for="Name" class="ms-2"></label>
    </div>
    <!-- Các field khác -->
</form>
```


### Hậu quả của việc thiếu ID

#### Khi ID = 0 hoặc null

- **Hành vi EF Core**: Coi như tạo record mới thay vì update
- **Kết quả**: Record cũ vẫn tồn tại, record mới được tạo với dữ liệu đã chỉnh sửa
- **Database**: Có 2 records thay vì 1 record được cập nhật


#### Cách debug và khắc phục

```csharp
[HttpPost]
public IActionResult Edit(Category obj)
{
    // Đặt breakpoint ở đây để kiểm tra obj.ID
    if (obj.ID == 0)
    {
        // Vấn đề: ID không được populate
        // Giải pháp: Thêm hidden field cho ID
    }
    
    if (ModelState.IsValid)
    {
        _db.Categories.Update(obj);
        _db.SaveChanges();
        return RedirectToAction("Index");
    }
    return View(obj);
}
```


### Quy tắc quan trọng cần nhớ

#### Best Practices cho Edit functionality

- **Luôn kiểm tra**: Đặt breakpoint để verify `obj.ID` có giá trị
- **Thêm hidden field**: Khi property name không phải là `ID`
- **Convention over Configuration**: ASP.NET Core tự động map `ID` từ route
- **Debugging tip**: Nếu tạo record mới thay vì update → kiểm tra ID = 0


#### Cảnh báo thường gặp

- **Triệu chứng**: Record bị duplicate thay vì update
- **Nguyên nhân**: `obj.ID = 0` trong POST action
- **Giải pháp**: Thêm `<input asp-for="ID" type="hidden" />`
- **Kiểm tra**: Debug để confirm ID có được populate đúng


### Chuẩn bị cho Delete functionality

Với Edit đã hoàn thiện, bước tiếp theo sẽ implement chức năng **Delete category** để hoàn tất các thao tác CRUD cơ bản.

**Liên kết:** [[Category Controller]], [[Model Binding]], [[Hidden Field]], [[Entity Framework Core]], [[Update Method]], [[Route Parameters]], [[CRUD Operations]], [[Debug Techniques]], [[ASP.NET Core Conventions]]

