# 🟨 TUẦN 12: TESTING & OPTIMIZATION

## 🎯 Mục tiêu
- Viết Unit Test cơ bản cho Backend.
- Tối ưu hiệu năng Frontend (Lazy Loading).

---

## 🧪 1. Backend Unit Testing

Sử dụng **xUnit** (Project Test riêng).

### 1.1. Tạo Project Test
```powershell
dotnet new xunit -n DNU.Shop.Tests
dotnet add reference ../DNU.Shop.API
```

### 1.2. Viết Test Case
Test xem hàm tính tổng tiền có đúng không.

```csharp
[Fact]
public void CalculateTotal_ShouldReturnCorrectSum()
{
    // Arrange
    var order = new Order();
    order.Items.Add(new OrderItem { Price = 10, Quantity = 2 }); // 20
    order.Items.Add(new OrderItem { Price = 5, Quantity = 1 });  // 5

    // Act
    var total = order.CalculateTotal();

    // Assert
    Assert.Equal(25, total);
}
```

---

## ⚡ 2. Frontend Optimization

### 2.1. Lazy Loading Routes
Thay vì tải toàn bộ trang Admin khi người dùng mới vào trang chủ, ta chỉ tải khi họ thực sự bấm vào link Admin.

Trong `router/index.js`:
```javascript
// Thay vì import trực tiếp ở đầu file
// import Dashboard from '@/views/admin/Dashboard.vue'

// Hãy dùng dynamic import
component: () => import('@/views/admin/Dashboard.vue')
```

### 2.2. Phân tích Bundle
Khi chạy `npm run build`, Vite sẽ báo kích thước từng file. Nếu file `index.js` quá lớn (>500KB), cần xem xét tách nhỏ code.

---

## 🧪 3. Thực hành

1. Viết 3 Unit Test cho logic nghiệp vụ (VD: Không được đặt hàng số lượng âm).
2. Kiểm tra lại Router xem đã dùng Lazy Loading chưa.
3. Chạy `npm run build` để xem kết quả tối ưu.
