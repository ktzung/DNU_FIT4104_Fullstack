# 🟨 TUẦN 10: PHÂN QUYỀN (AUTHORIZATION)

## 🎯 Mục tiêu
- Hiểu Role-based Authorization.
- Bảo vệ API chỉ cho phép Admin.
- Ẩn/Hiện menu trên Frontend dựa theo quyền.

---

## 👮 1. Backend: Role-based Access

### 1.1. Tạo Role mặc định
Trong `Program.cs` hoặc một Seeder Service, ta cần tạo sẵn Role "Admin" và "User".

```csharp
await roleManager.CreateAsync(new IdentityRole("Admin"));
await roleManager.CreateAsync(new IdentityRole("User"));
```

### 1.2. Bảo vệ Controller
Sử dụng thuộc tính `Roles` trong `[Authorize]`.

```csharp
[Authorize(Roles = "Admin")] // Chỉ Admin mới vào được
[HttpDelete("{id}")]
public async Task<IActionResult> Delete(int id)
{
    // Code xóa sản phẩm
}
```

Nếu User thường cố tình gọi API này -> Nhận lỗi **403 Forbidden**.

---

## 👁️ 2. Frontend: UI Permission

Ẩn nút "Xóa" nếu không phải Admin.

### 2.1. Lấy Role từ Token
Token JWT chứa thông tin Role (Claim). Ta cần giải mã nó.
Cài thư viện: `npm install jwt-decode`

```javascript
// stores/auth.js
import jwt_decode from "jwt-decode";

// Trong action login:
const decoded = jwt_decode(response.token);
this.user = {
    username: decoded.unique_name,
    role: decoded.role // "Admin" hoặc "User"
};
```

### 2.2. Sử dụng v-if
```html
<template>
  <v-btn v-if="authStore.user?.role === 'Admin'" color="red" @click="deleteItem">
    Xóa
  </v-btn>
</template>
```

---

## 🧪 3. Thực hành

1. Tạo 2 user: `admin@dnu.edu.vn` (Role Admin) và `student@dnu.edu.vn` (Role User).
2. Đăng nhập bằng Student -> Vào trang Product -> Không thấy nút Xóa.
3. Cố tình dùng Postman gọi API Delete với Token của Student -> Bị chặn 403.
4. Đăng nhập bằng Admin -> Thấy nút Xóa và xóa được.
