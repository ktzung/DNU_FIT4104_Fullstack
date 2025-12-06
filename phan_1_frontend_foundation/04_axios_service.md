# 🟦 TUẦN 4: HTTP CLIENT & API SERVICE

## 🎯 Mục tiêu
- Cài đặt Axios.
- Cấu hình Interceptors để xử lý Token tự động.
- Áp dụng Service Pattern để tách biệt logic gọi API.

---

## 📡 1. Axios Setup

**Axios** là thư viện phổ biến nhất để gọi API.

### 1.1. Cài đặt
```powershell
npm add axios
```

### 1.2. Tạo Instance (`utils/axios.js`)
Thay vì gọi `axios.get()` trực tiếp, ta tạo một instance chung để cấu hình Base URL.

```javascript
import axios from 'axios';

const instance = axios.create({
    baseURL: 'https://localhost:5001/api', // URL của Backend .NET
    timeout: 10000, // 10 giây
    headers: {
        'Content-Type': 'application/json'
    }
});

// Interceptor: Chạy trước khi gửi request
instance.interceptors.request.use(config => {
    // Lấy token từ LocalStorage gửi kèm
    const token = localStorage.getItem('accessToken');
    if (token) {
        config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
});

// Interceptor: Chạy sau khi nhận response
instance.interceptors.response.use(
    response => response.data, // Chỉ lấy phần data
    error => {
        // Xử lý lỗi chung (VD: 401 thì logout)
        if (error.response && error.response.status === 401) {
            // Redirect to login
        }
        return Promise.reject(error);
    }
);

export default instance;
```

---

## 🧱 2. Service Pattern

Không nên gọi `axios.get('/products')` trực tiếp trong Component. Hãy gom vào các Service file.

### 2.1. Tạo `services/productService.js`

```javascript
import axios from '@/utils/axios';

export default {
    getAll() {
        return axios.get('/products');
    },
    
    getById(id) {
        return axios.get(`/products/${id}`);
    },
    
    create(data) {
        return axios.post('/products', data);
    },
    
    update(id, data) {
        return axios.put(`/products/${id}`, data);
    },
    
    delete(id) {
        return axios.delete(`/products/${id}`);
    }
};
```

### 2.2. Sử dụng trong Component

```html
<script setup>
import { ref, onMounted } from 'vue';
import productService from '@/services/productService';

const products = ref([]);

async function loadData() {
    try {
        products.value = await productService.getAll();
    } catch (error) {
        console.error("Lỗi tải dữ liệu", error);
    }
}

onMounted(() => {
    loadData();
});
</script>
```

---

## 🧪 3. Thực hành

1. Tạo file `utils/axios.js` với cấu hình như trên.
2. Tạo `services/authService.js` với hàm `login(credentials)`.
3. Tạo `services/productService.js`.
4. (Tạm thời) Dùng [MockAPI.io](https://mockapi.io) để giả lập API test thử việc gọi dữ liệu và hiển thị lên Console.

---

## 💡 Tổng kết Giai đoạn 1
Chúc mừng! Bạn đã hoàn thành nền tảng Frontend.
- Đã có Project Vue 3 chuẩn.
- Đã có Router, Pinia, Vuetify.
- Đã có lớp giao tiếp API.

👉 **Tuần sau chúng ta sẽ code Backend .NET thực sự!**
