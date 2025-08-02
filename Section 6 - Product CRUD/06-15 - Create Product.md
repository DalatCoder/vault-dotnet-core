## Xử lý File Upload và Lưu trữ Hình ảnh cho Product

### Khái niệm

Khi người dùng chọn hình ảnh và nhấn nút Create, cần lưu trữ file hình ảnh vào server và cập nhật đường dẫn **ImageURL** trong database. Tất cả **static files** của ứng dụng phải được lưu trong thư mục **wwwroot**.

### Tạo cấu trúc thư mục lưu trữ

**Bước 1: Tạo thư mục Images**

- Trong thư mục `wwwroot`, tạo folder `images`
- Trong folder `images`, tạo subfolder `product`
- Cấu trúc: `wwwroot/images/product/`

**Mục đích:**

- Tổ chức file theo danh mục sản phẩm
- Dễ quản lý và truy cập static files
- Tuân thủ convention của ASP.NET Core


### Inject IWebHostEnvironment

**Bước 2: Thêm Dependency Injection**

Trong `ProductController.cs`:

```csharp
public class ProductController : Controller
{
    private readonly IUnitOfWork _unitOfWork;
    private readonly IWebHostEnvironment _webHostEnvironment;
    
    public ProductController(IUnitOfWork unitOfWork, IWebHostEnvironment webHostEnvironment)
    {
        _unitOfWork = unitOfWork;
        _webHostEnvironment = webHostEnvironment;
    }
}
```

**Đặc điểm IWebHostEnvironment:**

- **Built-in service**: Được cung cấp sẵn bởi .NET, không cần đăng ký
- **WebRootPath**: Cung cấp đường dẫn tuyệt đối đến thư mục wwwroot
- **Dependency Injection**: Inject tương tự như IUnitOfWork


### Xử lý File Upload trong POST Action

**Bước 3: Cập nhật Upsert POST Method**

```csharp
[HttpPost]
public IActionResult Upsert(ProductViewModel productVM, IFormFile? file)
{
    if (ModelState.IsValid)
    {
        // Lấy đường dẫn wwwroot
        string wwwRootPath = _webHostEnvironment.WebRootPath;
        
        // Kiểm tra nếu có file upload
        if (file != null)
        {
            // Tạo tên file ngẫu nhiên với GUID
            string fileName = Guid.NewGuid().ToString() + Path.GetExtension(file.FileName);
            
            // Xác định đường dẫn lưu trữ
            string productPath = Path.Combine(wwwRootPath, @"images\product");
            
            // Lưu file vào server
            using (var fileStream = new FileStream(Path.Combine(productPath, fileName), FileMode.Create))
            {
                file.CopyTo(fileStream);
            }
            
            // Cập nhật ImageURL trong model
            productVM.Product.ImageURL = @"\images\product\" + fileName;
        }
        
        // Thêm hoặc cập nhật product
        if (productVM.Product.Id == 0)
        {
            _unitOfWork.Product.Add(productVM.Product);
        }
        else
        {
            _unitOfWork.Product.Update(productVM.Product);
        }
        
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


### Chi tiết kỹ thuật

**Tạo tên file ngẫu nhiên:**

```csharp
string fileName = Guid.NewGuid().ToString() + Path.GetExtension(file.FileName);
```

- **GUID**: Tạo tên unique, tránh trùng lặp
- **Path.GetExtension()**: Giữ nguyên phần mở rộng file gốc (.jpg, .png, etc.)

**Xây dựng đường dẫn:**

```csharp
string productPath = Path.Combine(wwwRootPath, @"images\product");
```

- **Path.Combine()**: Kết hợp đường dẫn an toàn cross-platform
- **productPath**: Đường dẫn tuyệt đối đến thư mục lưu trữ

**Lưu file:**

```csharp
using (var fileStream = new FileStream(Path.Combine(productPath, fileName), FileMode.Create))
{
    file.CopyTo(fileStream);
}
```

- **FileStream**: Tạo stream để ghi file
- **FileMode.Create**: Tạo file mới hoặc ghi đè file cũ
- **Using statement**: Tự động dispose resources


### Khắc phục lỗi File Binding

**Vấn đề**: File upload không bind được với parameter `IFormFile? file`

**Nguyên nhân**: Input file thiếu attribute `name`

**Giải pháp**: Cập nhật input trong `Upsert.cshtml`

```html
<!-- Trước đây: Thiếu name attribute -->
<input type="file" class="form-control" />

<!-- Sau khi sửa: Thêm name="file" -->
<input type="file" name="file" class="form-control" />
```

**Lưu ý quan trọng:**

- Attribute `name` phải khớp với tên parameter trong action method
- Không có `name` = file không được bind to controller


### Debug và Test

**Các bước kiểm tra:**

1. **Model State**: Đảm bảo `ModelState.IsValid = true`
2. **File Parameter**: Kiểm tra `file != null`
3. **File Properties**: Xem file name, size, extension
4. **Directory**: Kiểm tra thư mục đích có tồn tại
5. **File Creation**: Xác nhận file được tạo trong wwwroot/images/product

**Debugging Points:**

- Trước và sau khi gán `ProductVM.Product.ImageURL`
- Kiểm tra `file.FileName` và generated `fileName`
- Xem `productPath` có đúng không


### Kết quả

- **File Upload**: Hình ảnh được lưu thành công vào `wwwroot/images/product/`
- **Random Naming**: File có tên GUID unique + extension gốc
- **Database Update**: ImageURL được lưu với đường dẫn relative
- **Create Function**: Hoạt động đầy đủ với file upload


### Ghi chú thêm

- **Static Files**: Chỉ files trong wwwroot mới accessible qua HTTP
- **GUID Naming**: Tránh conflict và security issues với original filename
- **Extension Preservation**: Giữ nguyên loại file để browser hiển thị đúng
- **Path Combine**: Best practice cho cross-platform compatibility
- **Using Statement**: Quan trọng để avoid memory leaks với FileStream
- **Edit Functionality**: Sẽ cần logic phức tạp hơn để handle existing images

**Liên kết:** [[ProductViewModel]], [[IFormFile]], [[IWebHostEnvironment]], [[File Upload]], [[Static Files]], [[GUID]], [[Path.Combine]], [[FileStream]], [[Product Controller]], [[Upsert]], [[wwwroot]], [[ImageURL]], [[ASP.NET Core MVC]], [[Dependency Injection]]

