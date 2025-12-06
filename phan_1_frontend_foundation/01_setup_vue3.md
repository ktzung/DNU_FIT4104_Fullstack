# 🟦 TUẦN 1: KHỞI ĐỘNG & VUE 3 SETUP

## 🎯 Mục tiêu
- Hiểu kiến trúc SPA (Single Page Application).
- Cài đặt môi trường phát triển.
- Khởi tạo dự án Vue 3 với Vite.
- Làm quen với Composition API (`setup`, `ref`, `reactive`).

---

## 📖 1. Kiến trúc SPA là gì?

Khác với Web truyền thống (Server-side Rendering - mỗi lần click là load lại trang), **SPA** chỉ tải trang một lần duy nhất. Sau đó, Javascript sẽ đóng vai trò cập nhật nội dung mà không cần reload.

**Ưu điểm:**
- Trải nghiệm mượt mà như App Mobile.
- Tách biệt Frontend và Backend (Backend chỉ trả JSON).

---

## 🛠️ 2. Khởi tạo dự án Vue 3

Chúng ta sẽ dùng **Vite** (công cụ build siêu tốc) thay vì Vue CLI cũ.

### Bước 1: Tạo dự án
Mở Terminal (VS Code) tại thư mục muốn lưu dự án:

```powershell
npm create vue@latest
```

Điền các thông tin:
- Project name: `dnu-shop-client`
- Add TypeScript? -> **No** (để đơn giản cho môn này)
- Add JSX? -> **No**
- Add Vue Router? -> **Yes** (Cần thiết)
- Add Pinia? -> **Yes** (Quản lý State)
- Add Vitest? -> **No**
- Add ESLint? -> **Yes**

### Bước 2: Chạy thử
```powershell
cd dnu-shop-client
npm install
npm run dev
```
Truy cập `http://localhost:5173` để xem kết quả.

---

## 💻 3. Vue 3 Composition API

Đây là cách viết mới của Vue 3, gọn gàng và dễ tái sử dụng hơn Options API (Vue 2).

### 3.1. Script Setup
Thay vì `export default { ... }`, ta dùng `<script setup>`.

```html
<script setup>
import { ref } from 'vue';

// Khai báo biến (State)
const count = ref(0);

// Khai báo hàm (Method)
function increment() {
  count.value++;
}
</script>

<template>
  <button @click="increment">Đếm: {{ count }}</button>
</template>
```

### 3.2. Ref vs Reactive
- `ref()`: Dùng cho dữ liệu nguyên thủy (số, chuỗi, boolean) hoặc object đơn. Khi truy cập trong script phải dùng `.value`.
- `reactive()`: Dùng cho object phức tạp. Không cần `.value`.

```javascript
const user = reactive({
  name: 'Nguyen Van A',
  age: 20
});

function birthday() {
  user.age++; // Không cần .value
}
```

---

## 🧪 4. Thực hành: Cấu trúc thư mục

Hãy tổ chức thư mục `src` như sau để chuẩn bị cho dự án lớn:

```
src/
├── assets/          # Ảnh, CSS, Fonts
├── components/      # Các component nhỏ (Button, Card...)
├── layouts/         # Bố cục trang (AdminLayout, UserLayout)
├── router/          # Cấu hình đường dẫn
├── stores/          # Pinia Stores
├── views/           # Các màn hình chính (Home, Product, Login)
│   ├── admin/
│   └── public/
├── App.vue          # Component gốc
└── main.js          # Điểm khởi chạy
```

### Bài tập về nhà:
1. Tạo dự án `dnu-shop-client`.
2. Dọn dẹp các file mẫu (HelloWord.vue).
3. Tạo 2 file view trống: `views/public/HomePage.vue` và `views/admin/DashboardPage.vue`.
