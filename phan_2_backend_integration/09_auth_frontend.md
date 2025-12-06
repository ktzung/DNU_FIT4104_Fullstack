# 🟩 TUẦN 9: AUTHENTICATION (FRONTEND)

## 🎯 Mục tiêu
- Thiết kế trang Login đẹp mắt.
- Gọi API Login và lưu Token vào LocalStorage.
- Bảo vệ Router (Navigation Guards).

---

## 🚪 1. Login Logic

Sử dụng `authStore` (Pinia) để xử lý logic đăng nhập.

### 1.1. Cập nhật Store (`stores/auth.js`)
```javascript
import { defineStore } from 'pinia';
import axios from '@/utils/axios';
import router from '@/router';

export const useAuthStore = defineStore('auth', {
    state: () => ({
        user: null,
        token: localStorage.getItem('accessToken') || null
    }),
    
    actions: {
        async login(credentials) {
            try {
                const response = await axios.post('/auth/login', credentials);
                
                // 1. Lưu token
                this.token = response.token;
                localStorage.setItem('accessToken', this.token);
                
                // 2. Chuyển hướng vào Admin
                router.push('/admin/dashboard');
            } catch (error) {
                throw error; // Ném lỗi ra để Component hiển thị
            }
        },
        
        logout() {
            this.token = null;
            this.user = null;
            localStorage.removeItem('accessToken');
            router.push('/login');
        }
    }
});
```

---

## 🛡️ 2. Route Protection

Ngăn chặn người dùng chưa đăng nhập truy cập vào `/admin`.

### 2.1. Navigation Guard (`router/index.js`)

```javascript
router.beforeEach((to, from, next) => {
    const publicPages = ['/login', '/'];
    const authRequired = !publicPages.includes(to.path);
    const loggedIn = localStorage.getItem('accessToken');

    if (authRequired && !loggedIn) {
        // Nếu cần auth mà chưa login -> Đá về login
        return next('/login');
    }

    next();
});
```

---

## 🧪 3. Thực hành

1. Tạo `views/public/LoginPage.vue` với form Username/Password.
2. Gọi `authStore.login()` khi submit form.
3. Thử truy cập trực tiếp `/admin/dashboard` khi chưa login -> Phải bị đẩy về Login.
4. Login thành công -> Phải vào được Dashboard.
5. F5 lại trang -> Vẫn phải giữ trạng thái đăng nhập (nhờ LocalStorage).
