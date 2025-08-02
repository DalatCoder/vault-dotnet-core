## Tạo View cho chỉnh sửa danh mục (Edit Category View)

### Tạo View trong Visual Studio

#### Cách 1: Empty View

- **Thao tác**: Right-click → Add View → Empty View
- **Đặc điểm**: Tạo một view trống, cần tự viết toàn bộ HTML
- **Sử dụng**: Khi muốn kiểm soát hoàn toàn cấu trúc view


#### Cách 2: Razor View (Khuyên dùng)

- **Thao tác**: Right-click → Add View → Razor View
- **Tùy chọn scaffolding**: Có thể chọn template Create, Details, Edit, Delete
- **Cấu hình quan trọng**:
    - **View name**: Edit
    - **Template**: Empty (để tự tùy chỉnh)
    - **Use a layout page**: ✅ Chọn để sử dụng layout mặc định
    - **Partial view**: ❌ Không chọn


#### Lưu ý về Layout Page

```html
<!-- ❌ Không sử dụng layout (tạo HTML page độc lập) -->
Layout = null;

<!-- ✅ Sử dụng layout mặc định -->
@{
    ViewData["Title"] = "Edit";
}
```


### Sao chép nội dung từ Create View

#### Cấu trúc view Edit

```html
@model Category

@{
    ViewData["Title"] = "Edit Category";
}

<div class="card shadow border-0 mt-4">
    <div class="card-header bg-secondary bg-gradient ml-0 py-3">
        <div class="row">
            <div class="col-12 text-center">
                <h2 class="text-white py-2">Update Category</h2>
            </div>
        </div>
    </div>
    <div class="card-body p-4">
        <form method="post">
            <div class="border p-3">
                <div class="form-floating py-2 col-12">
                    <input asp-for="Name" class="form-control border-0 shadow" />
                    <label asp-for="Name" class="ms-2"></label>
                    <span asp-validation-for="Name" class="text-danger"></span>
                </div>
                <div class="form-floating py-2 col-12">
                    <input asp-for="DisplayOrder" class="form-control border-0 shadow" />
                    <label asp-for="DisplayOrder" class="ms-2"></label>
                    <span asp-validation-for="DisplayOrder" class="text-danger"></span>
                </div>
                <div class="row pt-2">
                    <div class="col-6 col-md-3">
                        <button type="submit" class="btn btn-primary form-control">Update</button>
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


#### Những thay đổi so với Create View

- **Tiêu đề**: "Create Category" → "Update Category"
- **Nút submit**: "Create" → "Update"
- **Form action**: Tự động submit đến Edit POST action


### Cơ chế tự động populate dữ liệu

#### Tag Helpers và Model Binding

```csharp
// Trong Edit GET action
public IActionResult Edit(int? id)
{
    Category categoryFromDb = _db.Categories.Find(id);
    return View(categoryFromDb); // Truyền object có sẵn dữ liệu
}
```


#### Tự động hiển thị dữ liệu

- **Nguyên lý**: Tag helpers `asp-for` tự động bind với properties của model
- **Kết quả**: Các trường input tự động hiển thị giá trị từ `categoryFromDb`
- **Ví dụ**:
    - `<input asp-for="Name" />` sẽ hiển thị giá trị `categoryFromDb.Name`
    - `<input asp-for="DisplayOrder" />` sẽ hiển thị giá trị `categoryFromDb.DisplayOrder`


### Xử lý cập nhật dữ liệu (POST Action)

#### Phương thức Update của Entity Framework Core

```csharp
[HttpPost]
public IActionResult Edit(Category obj)
{
    if (ModelState.IsValid)
    {
        _db.Categories.Update(obj); // EF Core tự động cập nhật
        _db.SaveChanges();
        return RedirectToAction("Index");
    }
    return View(obj);
}
```


#### Cách hoạt động của Update()

- **Input**: Nhận object Category với ID và các properties mới
- **Logic**: EF Core dựa vào **ID** để xác định record cần update
- **Cập nhật**: Tự động cập nhật tất cả properties khác trong database
- **Ưu điểm**: Giảm thiểu code, EF Core xử lý toàn bộ SQL update


### Validation và User Experience

#### Client-side Validation

- **Tự động hoạt động**: Validation attributes từ model tự động áp dụng
- **Ví dụ**: Nếu xóa Display Order, validation sẽ hiển thị lỗi ngay lập tức
- **Benefit**: Không cần reload page, trải nghiệm người dùng mượt mà


#### Server-side Validation

```csharp
if (ModelState.IsValid)
{
    // Xử lý update
}
else
{
    return View(obj); // Trả về view với lỗi validation
}
```


### Ghi chú quan trọng

- **Tag Helpers magic**: Tự động populate dữ liệu mà không cần code thêm
- **EF Core Update**: Đơn giản hóa việc cập nhật database
- **Reusability**: Template giống Create, chỉ khác về logic xử lý
- **Performance**: EF Core tối ưu SQL queries tự động

**Liên kết:** [[Category Controller]], [[Entity Framework Core]], [[Tag Helpers]], [[Model Binding]], [[Razor Views]], [[Update Method]], [[Client-side Validation]], [[Layout Page]], [[ViewData]]

