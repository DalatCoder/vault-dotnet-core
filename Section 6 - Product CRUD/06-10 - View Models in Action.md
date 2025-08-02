## Sử dụng ViewModel thay thế ViewBag và ViewData

### Vấn đề với ViewBag và ViewData

Khi ứng dụng phát triển lớn, việc sử dụng nhiều ViewBag và ViewData sẽ gây ra các vấn đề:

- **Khó bảo trì**: Khi có 10 ViewData và 5 ViewBag, code trở nên khó đọc
- **Thiếu tính minh bạch**: Khó xác định nguồn gốc dữ liệu trong View
- **Ràng buộc lỏng lẻo**: View được bound với Product model nhưng thiếu dữ liệu cần thiết
- **Không có IntelliSense**: Dễ gây lỗi runtime do thiếu type checking


### Khái niệm ViewModel

**ViewModel** là mô hình được thiết kế đặc biệt cho một View cụ thể:

- Chứa tất cả dữ liệu cần thiết cho View
- Tạo **Strongly Typed View** - View được ràng buộc chặt chẽ với một model
- Kết hợp nhiều objects khác nhau thành một entity duy nhất
- Thay thế hoàn toàn ViewBag/ViewData


### Cách tạo ProductViewModel

**Bước 1: Tạo thư mục ViewModels**

Trong thư mục Models, tạo folder `ViewModels` và class `ProductViewModel.cs`:

```csharp
public class ProductViewModel
{
    public Product Product { get; set; }
    public IEnumerable<SelectListItem> CategoryList { get; set; }
}
```

**Bước 2: Cài đặt NuGet Package**

Cần cài đặt package cho SelectListItem:

```
Microsoft.AspNetCore.Mvc.ViewFeatures
```

**Bước 3: Cập nhật ViewImports**

Thêm using statement trong các file `_ViewImports.cshtml`:

```csharp
@using BulkyBook.Models.ViewModels
```

Cập nhật trong:

- `Areas/Admin/Views/_ViewImports.cshtml`
- `Views/_ViewImports.cshtml`


### Cập nhật View và Controller

**Cập nhật Create.cshtml:**

```html
@model ProductViewModel

<div class="mb-3">
    <label asp-for="Product.Title" class="form-label"></label>
    <input asp-for="Product.Title" class="form-control" />
    <span asp-validation-for="Product.Title" class="text-danger"></span>
</div>

<!-- Các fields khác tương tự với Product. prefix -->

<div class="mb-3">
    <label asp-for="Product.CategoryId" class="form-label"></label>
    <select asp-for="Product.CategoryId" asp-items="@Model.CategoryList" class="form-select">
        <option disabled selected>Select Category</option>
    </select>
    <span asp-validation-for="Product.CategoryId" class="text-danger"></span>
</div>
```

**Cập nhật ProductController:**

```csharp
public IActionResult Create()
{
    ProductViewModel productVM = new ProductViewModel()
    {
        CategoryList = _unitOfWork.Category.GetAll()
            .Select(u => new SelectListItem
            {
                Text = u.Name,
                Value = u.Id.ToString()
            }),
        Product = new Product()
    };
    
    return View(productVM);
}

[HttpPost]
public IActionResult Create(ProductViewModel productVM)
{
    if (ModelState.IsValid)
    {
        _unitOfWork.Product.Add(productVM.Product);
        _unitOfWork.Save();
        TempData["success"] = "Product created successfully";
        return RedirectToAction("Index");
    }
    else
    {
        // Populate dropdown lại khi có lỗi
        productVM.CategoryList = _unitOfWork.Category.GetAll()
            .Select(u => new SelectListItem
            {
                Text = u.Name,
                Value = u.Id.ToString()
            });
        
        return View(productVM);
    }
}
```


### Xử lý Validation Issues

**Vấn đề ModelState.IsValid = false:**

Khi POST form, một số properties không mong muốn bị validate:

- `CategoryList`: Không cần validate
- `Category`: Navigation property không cần validate
- `ImageURL`: Chưa có input field

**Giải pháp sử dụng [ValidateNever]:**

```csharp
public class ProductViewModel
{
    public Product Product { get; set; }
    
    [ValidateNever]
    public IEnumerable<SelectListItem> CategoryList { get; set; }
}
```

Trong Product model:

```csharp
public class Product
{
    // ... other properties
    
    [ValidateNever]
    public Category Category { get; set; }
    
    [ValidateNever]
    public string ImageURL { get; set; }
}
```


### Xử lý lỗi Exception khi POST

**Vấn đề:** Khi ModelState không valid, View được return nhưng CategoryList = null gây lỗi "Object reference not set to an instance of an object".

**Giải pháp:** Populate lại dropdown trong else block:

```csharp
[HttpPost]
public IActionResult Create(ProductViewModel productVM)
{
    if (ModelState.IsValid)
    {
        _unitOfWork.Product.Add(productVM.Product);
        _unitOfWork.Save();
        return RedirectToAction("Index");
    }
    else
    {
        // QUAN TRỌNG: Populate lại CategoryList
        productVM.CategoryList = _unitOfWork.Category.GetAll()
            .Select(u => new SelectListItem
            {
                Text = u.Name,
                Value = u.Id.ToString()
            });
        
        return View(productVM);
    }
}
```


### Ưu điểm của ViewModel

- **Strongly Typed**: View được bind chặt chẽ với model
- **IntelliSense Support**: Có hỗ trợ autocomplete và type checking
- **Dễ bảo trì**: Code rõ ràng, dễ hiểu
- **Tái sử dụng**: Có thể dùng cho nhiều Views tương tự
- **Validation**: Kiểm soát validation một cách chính xác
- **Performance**: Không có overhead của dynamic typing


### Thuật ngữ quan trọng

**Strongly Typed View:**

- View được ràng buộc chặt chẽ với một model cụ thể
- Ngược lại với Dynamic View sử dụng ViewBag/ViewData
- Cung cấp compile-time checking và IntelliSense support


### Ghi chú thêm

- **ViewModel pattern** là best practice trong ASP.NET Core MVC
- Tránh sử dụng ViewBag/ViewData càng nhiều càng tốt
- Luôn populate lại dropdown data khi return View trong POST action
- Sử dụng `[ValidateNever]` cho properties không cần validation
- ViewModel có thể chứa multiple entities và collections
- Pattern này đặc biệt hữu ích cho forms phức tạp có nhiều data sources

**Liên kết:** [[ViewBag]], [[ViewData]], [[Product Model]], [[Category Model]], [[SelectListItem]], [[Model Validation]], [[Strongly Typed Views]], [[ASP.NET Core MVC]], [[ProductController]], [[Data Annotations]], [[ModelState]], [[TempData]], [[Entity Framework Core]]

