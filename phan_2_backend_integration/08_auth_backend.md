# 🟩 TUẦN 8: AUTHENTICATION (BACKEND)

## 🎯 Mục tiêu
- Cài đặt ASP.NET Core Identity.
- Viết API Login sinh JWT Token.
- Viết API Register tạo User mới.

---

## 🔐 1. Identity Setup

**ASP.NET Core Identity** là thư viện quản lý User, Role, Login, Password Hashing chuẩn của Microsoft.

### 1.1. Cài đặt Package
```powershell
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
```

### 1.2. Kế thừa IdentityDbContext
Sửa `ApplicationDbContext`:
```csharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser>
{
    public ApplicationDbContext(DbContextOptions options) : base(options) { }
    
    public DbSet<Product> Products { get; set; }
}
```

### 1.3. Đăng ký Service (`Program.cs`)
```csharp
builder.Services.AddIdentity<IdentityUser, IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>()
    .AddDefaultTokenProviders();
```

---

## 🔑 2. JWT Generation

Xem lại kiến thức ở Ebook Backend (Chương 10). Ở đây ta áp dụng vào dự án.

### 2.1. AuthController
```csharp
[HttpPost("login")]
public async Task<IActionResult> Login([FromBody] LoginDto model)
{
    // 1. Tìm user
    var user = await _userManager.FindByNameAsync(model.Username);
    
    // 2. Check password
    if (user != null && await _userManager.CheckPasswordAsync(user, model.Password))
    {
        // 3. Sinh Token
        var token = GenerateJwtToken(user);
        return Ok(new { token });
    }
    
    return Unauthorized();
}

private string GenerateJwtToken(IdentityUser user)
{
    // ... Code sinh token (Header, Payload, Signature)
    // Payload chứa: Sub (Id), Jti, Iat, ClaimTypes.Name
}
```

---

## 🧪 3. Thực hành

1. Chạy Migration để tạo các bảng Identity (`AspNetUsers`, `AspNetRoles`...).
2. Viết API `POST /api/auth/register` để tạo user mẫu.
3. Viết API `POST /api/auth/login`.
4. Test bằng Postman: Gửi user/pass -> Nhận về chuỗi Token `eyJ...`.
