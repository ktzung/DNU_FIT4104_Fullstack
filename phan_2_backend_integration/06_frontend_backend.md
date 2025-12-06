# 🟩 TUẦN 6: KẾT NỐI FRONTEND - BACKEND

## 🎯 Mục tiêu
- Xử lý lỗi CORS.
- Gọi API từ Vue để hiển thị danh sách sản phẩm.
- Hiển thị Loading State và Error State.

---

## 🚧 1. CORS Policy

Khi Vue (localhost:5173) gọi API (localhost:5000), trình duyệt sẽ chặn vì khác cổng (Cross-Origin).

### Giải pháp: Cấu hình CORS ở Backend
Trong `Program.cs`:

```csharp
var builder = WebApplication.CreateBuilder(args);

// 1. Add Service
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowVueApp",
        policy =>
        {
            policy.WithOrigins("http://localhost:5173") // Cho phép Vue
                  .AllowAnyHeader()
                  .AllowAnyMethod();
        });
});

var app = builder.Build();

// 2. Use Middleware (Đặt trước MapControllers)
app.UseCors("AllowVueApp");

app.MapControllers();
app.Run();
```

---

## 📡 2. Fetching Data trong Vue

Sử dụng `productService` đã viết ở Tuần 4.

**`views/admin/ProductPage.vue`**:

```html
<script setup>
import { ref, onMounted } from 'vue';
import productService from '@/services/productService';

const products = ref([]);
const loading = ref(false);
const error = ref(null);

const headers = [
  { title: 'ID', key: 'id' },
  { title: 'Tên', key: 'name' },
  { title: 'Giá', key: 'price' },
];

async function loadProducts() {
  loading.value = true;
  error.value = null;
  try {
    // Gọi API thực tế
    products.value = await productService.getAll();
  } catch (err) {
    error.value = "Không thể tải dữ liệu. Vui lòng thử lại.";
    console.error(err);
  } finally {
    loading.value = false;
  }
}

onMounted(() => {
  loadProducts();
});
</script>

<template>
  <v-container>
    <h1>Quản lý Sản phẩm</h1>
    
    <!-- Thông báo lỗi -->
    <v-alert v-if="error" type="error" class="mb-4">{{ error }}</v-alert>

    <!-- Bảng dữ liệu -->
    <v-data-table
      :headers="headers"
      :items="products"
      :loading="loading"
    ></v-data-table>
  </v-container>
</template>
```

---

## 🧪 3. Thực hành

1. Bật cả 2 project: Backend (Visual Studio) và Frontend (VS Code).
2. Đảm bảo Backend đã cấu hình CORS.
3. Vào trang Admin/Products trên Vue.
4. Kiểm tra Network Tab (F12) xem request có thành công không (Status 200).
5. Nếu thấy dữ liệu từ SQL hiện lên bảng -> Thành công!
