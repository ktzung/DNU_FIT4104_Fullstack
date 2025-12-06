# 🟩 TUẦN 5: WEB API NÂNG CAO

## 🎯 Mục tiêu
- Xây dựng API chuẩn RESTful cho Product.
- Sử dụng DTO (Data Transfer Object) để kiểm soát dữ liệu trả về.
- Cấu hình AutoMapper.

---

## 🏗️ 1. RESTful Standards

Chúng ta đã học cơ bản ở môn Backend. Giờ hãy áp dụng chuẩn công nghiệp.

### 1.1. URL Naming
- **Đúng**: `GET /api/products`, `POST /api/products`, `GET /api/products/1`
- **Sai**: `GET /api/getProducts`, `POST /api/createProduct`

### 1.2. Status Codes
- **200 OK**: Thành công.
- **201 Created**: Tạo mới thành công (POST).
- **204 No Content**: Xóa/Sửa thành công nhưng không trả về dữ liệu.
- **400 Bad Request**: Dữ liệu gửi lên sai (Validation).
- **404 Not Found**: Không tìm thấy ID.

---

## 📦 2. DTO & AutoMapper

Không bao giờ trả về trực tiếp Entity của Database ra ngoài!

### 2.1. Tại sao cần DTO?
- **Bảo mật**: Ẩn các trường nhạy cảm (PasswordHash, InternalId).
- **Tối ưu**: Chỉ trả về dữ liệu cần thiết (VD: Danh sách chỉ cần Tên + Giá, không cần Mô tả dài).
- **Decoupling**: Thay đổi DB không làm hỏng Client.

### 2.2. Cài đặt AutoMapper
```powershell
dotnet add package AutoMapper.Extensions.Microsoft.DependencyInjection
```

### 2.3. Cấu hình Mapping
```csharp
public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Product, ProductDto>();
        CreateMap<CreateProductDto, Product>();
    }
}
```

Trong `Program.cs`:
```csharp
builder.Services.AddAutoMapper(typeof(Program));
```

---

## 💻 3. Viết API Product

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly ApplicationDbContext _context;
    private readonly IMapper _mapper;

    public ProductsController(ApplicationDbContext context, IMapper mapper)
    {
        _context = context;
        _mapper = mapper;
    }

    [HttpGet]
    public async Task<ActionResult<List<ProductDto>>> GetAll()
    {
        var products = await _context.Products.ToListAsync();
        return Ok(_mapper.Map<List<ProductDto>>(products));
    }

    [HttpPost]
    public async Task<ActionResult<ProductDto>> Create(CreateProductDto request)
    {
        var product = _mapper.Map<Product>(request);
        _context.Products.Add(product);
        await _context.SaveChangesAsync();
        
        return CreatedAtAction(nameof(GetById), new { id = product.Id }, _mapper.Map<ProductDto>(product));
    }
}
```

---

## 🧪 4. Thực hành

1. Tạo Project Web API mới tên `DNU.Shop.API`.
2. Cài đặt EF Core, SQL Server.
3. Tạo Entity `Product` (Id, Name, Price, Image, Description).
4. Tạo DTOs: `ProductDto`, `CreateProductDto`.
5. Viết Controller hoàn chỉnh CRUD.
6. Test bằng Swagger.
