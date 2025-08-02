## Cài đặt Entity Framework Core và các NuGet Packages

### Khái niệm Entity Framework Core

Entity Framework Core là framework cho phép thực hiện tất cả các thao tác liên quan đến cơ sở dữ liệu trực tiếp từ code. Trong quá khứ của .NET Framework, Entity Framework Core đã được tích hợp sẵn trong package chính của ứng dụng .NET. Tuy nhiên, khi .NET Framework phát triển, ứng dụng cơ bản chỉ đi kèm với các packages cần thiết, còn tất cả các thành phần khác phải được thêm thủ công vào dự án.

### Cách cài đặt NuGet Packages

#### Truy cập Manage NuGet Packages

- Chuột phải vào project (Bulky.Web)
- Chọn **Manage NuGet Packages**


#### Package thứ nhất: Microsoft.EntityFrameworkCore

```
Microsoft.EntityFrameworkCore
```

- Đây là package chính của Entity Framework Core
- Nếu đang sử dụng .NET 8 preview, cần bật checkbox **"Include prerelease"**
- Đảm bảo chọn đúng version phù hợp với .NET framework đang sử dụng


#### Package thứ hai: Microsoft.EntityFrameworkCore.SqlServer

```
Microsoft.EntityFrameworkCore.SqlServer
```

- Package này cần thiết để Entity Framework Core có thể làm việc với SQL Server
- Cung cấp provider cho SQL Server database


#### Package thứ ba: Microsoft.EntityFrameworkCore.Tools

```
Microsoft.EntityFrameworkCore.Tools
```

- Package này cung cấp các commands cần thiết cho **migration**
- Migration là quá trình tạo và cập nhật cấu trúc database từ code


### Lưu ý quan trọng về Version Compatibility

- **Luôn đảm bảo tất cả Microsoft packages có cùng version**
- Ví dụ: Nếu cài Entity Framework Core với .NET 8, thì tất cả packages khác cũng phải là .NET 8
- Nếu trộn lẫn versions (ví dụ: EF Core .NET 8 với package khác .NET 7), ứng dụng sẽ không hoạt động
- Có thể chọn version cụ thể thay vì luôn cài bản mới nhất


### Kiểm tra Packages đã cài đặt

#### Trong Manage NuGet Packages

- Chuyển sang tab **"Installed"** để xem tất cả packages đã cài
- Tab **"Updates"** cho phép cập nhật tất cả packages cùng lúc


#### Trong Project File

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">  
    <PropertyGroup>        
    <TargetFramework>net8.0</TargetFramework>  
        <Nullable>enable</Nullable>  
        <ImplicitUsings>enable</ImplicitUsings>  
        <RootNamespace>Bulky</RootNamespace>  
    </PropertyGroup>  
    <ItemGroup>      
	    <PackageReference Include="Microsoft.EntityFrameworkCore" Version="8.0.18" />  
	    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="8.0.18">  
	        <PrivateAssets>all</PrivateAssets>  
	        <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>  
		</PackageReference>      
		<PackageReference Include="Pomelo.EntityFrameworkCore.MySql" Version="8.0.3" />  
    </ItemGroup>  
</Project>
```

- Mọi NuGet package được thêm sẽ tự động xuất hiện trong project file
- Project sử dụng thông tin này để biết cần load packages nào
- Có thể edit project file trực tiếp để xem/chỉnh sửa package references


### Ghi chú thêm

- **Migration** sẽ được giải thích chi tiết trong các video tiếp theo
- Việc cài đặt các packages này là bước chuẩn bị để có thể tạo database từ code
- Project file (.csproj) là nơi lưu trữ tất cả thông tin về dependencies của project

**Liên kết:** [[Entity Framework Core]], [[SQL Server]], [[Migration]], [[NuGet Packages]], [[Connection String]]

<div style="text-align: center">⁂</div>

[^1]: Dependency-Injection-Tiem-phu-thuoc.md

