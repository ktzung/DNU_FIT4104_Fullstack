# 🟦 TUẦN 3: UI FRAMEWORK (VUETIFY)

## 🎯 Mục tiêu
- Cài đặt thư viện UI Vuetify.
- Sử dụng Grid System để layout responsive.
- Thiết kế giao diện Dashboard và Data Table.

---

## 🎨 1. Tại sao dùng UI Framework?

Thay vì viết CSS từ đầu, UI Framework cung cấp sẵn các component đẹp, chuẩn UX và Responsive.
Trong khóa này, ta dùng **Vuetify** (Material Design cho Vue).

### 1.1. Cài đặt Vuetify
```powershell
npm add vuetify
npm add @mdi/font
```

Cấu hình `main.js`:
```javascript
import 'vuetify/styles'
import { createVuetify } from 'vuetify'
import * as components from 'vuetify/components'
import * as directives from 'vuetify/directives'
import '@mdi/font/css/materialdesignicons.css'

const vuetify = createVuetify({
  components,
  directives,
})

app.use(vuetify)
```

---

## 📐 2. Grid System

Vuetify dùng hệ thống lưới 12 cột (`v-row`, `v-col`).

```html
<v-container>
  <v-row>
    <!-- Cột chiếm 12 phần (full) trên mobile, 6 phần (một nửa) trên desktop -->
    <v-col cols="12" md="6">
      <div class="bg-red">Cột trái</div>
    </v-col>
    <v-col cols="12" md="6">
      <div class="bg-blue">Cột phải</div>
    </v-col>
  </v-row>
</v-container>
```

---

## 📊 3. Các Component quan trọng

### 3.1. Data Table (`v-data-table`)
Dùng để hiển thị danh sách sản phẩm.

```html
<script setup>
const headers = [
  { title: 'Tên sản phẩm', key: 'name' },
  { title: 'Giá', key: 'price' },
  { title: 'Hành động', key: 'actions', sortable: false },
]

const products = [
  { name: 'iPhone 15', price: 20000000 },
  { name: 'Samsung S24', price: 18000000 },
]
</script>

<template>
  <v-data-table :headers="headers" :items="products">
    <template v-slot:item.actions="{ item }">
      <v-icon color="blue">mdi-pencil</v-icon>
      <v-icon color="red">mdi-delete</v-icon>
    </template>
  </v-data-table>
</template>
```

### 3.2. Form Input
```html
<v-text-field label="Tên đăng nhập" variant="outlined"></v-text-field>
<v-btn color="primary">Đăng nhập</v-btn>
```

---

## 🧪 4. Thực hành: Thiết kế Dashboard

1. Trong `views/admin/DashboardPage.vue`:
   - Tạo 4 Card thống kê (Doanh thu, Đơn hàng, Khách hàng...).
   - Dùng `v-row` và `v-col` để chia 4 cột trên Desktop, 2 cột trên Tablet.

2. Trong `views/admin/ProductPage.vue`:
   - Tạo bảng danh sách sản phẩm dùng `v-data-table`.
   - Thêm nút "Thêm mới" ở góc trên.

---

## 💡 Mẹo nhỏ
> [!TIP]
> Tham khảo trang [Vuetify Component Explorer](https://vuetifyjs.com/en/components/all/) để copy code mẫu nhanh chóng.
