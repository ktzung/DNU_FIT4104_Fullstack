# 🟦 TUẦN 2: ROUTING & STATE MANAGEMENT

## 🎯 Mục tiêu
- Cấu hình Vue Router để điều hướng trang.
- Sử dụng Nested Routes cho Layout.
- Quản lý trạng thái toàn cục với Pinia.

---

## 🛣️ 1. Vue Router & Layouts

Trong một ứng dụng thực tế, ta thường có nhiều Layout khác nhau (VD: Trang Admin có Sidebar, Trang User có Menu ngang).

### 1.1. Tạo Layout Components

**`layouts/AdminLayout.vue`**:
```html
<template>
  <div class="admin-layout">
    <aside>Sidebar Menu</aside>
    <main>
      <RouterView /> <!-- Nội dung thay đổi ở đây -->
    </main>
  </div>
</template>
```

**`layouts/PublicLayout.vue`**:
```html
<template>
  <div class="public-layout">
    <header>Header Menu</header>
    <main>
      <RouterView />
    </main>
    <footer>Footer</footer>
  </div>
</template>
```

### 1.2. Cấu hình Router (`router/index.js`)

```javascript
import { createRouter, createWebHistory } from 'vue-router'
import PublicLayout from '@/layouts/PublicLayout.vue'
import AdminLayout from '@/layouts/AdminLayout.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    // Public Routes
    {
      path: '/',
      component: PublicLayout,
      children: [
        { path: '', component: () => import('@/views/public/HomePage.vue') },
        { path: 'login', component: () => import('@/views/public/LoginPage.vue') }
      ]
    },
    // Admin Routes
    {
      path: '/admin',
      component: AdminLayout,
      children: [
        { path: 'dashboard', component: () => import('@/views/admin/DashboardPage.vue') },
        { path: 'products', component: () => import('@/views/admin/ProductPage.vue') }
      ]
    }
  ]
})

export default router
```

---

## 🍍 2. Pinia State Management

Pinia giúp chia sẻ dữ liệu giữa các component (ví dụ: thông tin User đăng nhập, Giỏ hàng).

### 2.1. Định nghĩa Store (`stores/auth.js`)

```javascript
import { ref, computed } from 'vue'
import { defineStore } from 'pinia'

export const useAuthStore = defineStore('auth', () => {
  // State
  const user = ref(null)
  const isAuthenticated = ref(false)

  // Actions
  function login(userData) {
    user.value = userData
    isAuthenticated.value = true
  }

  function logout() {
    user.value = null
    isAuthenticated.value = false
  }

  return { user, isAuthenticated, login, logout }
})
```

### 2.2. Sử dụng trong Component

```html
<script setup>
import { useAuthStore } from '@/stores/auth'

const authStore = useAuthStore()

function handleLogin() {
  authStore.login({ name: 'Admin', role: 'admin' })
}
</script>

<template>
  <div v-if="authStore.isAuthenticated">
    Xin chào, {{ authStore.user.name }}
    <button @click="authStore.logout">Đăng xuất</button>
  </div>
  <button v-else @click="handleLogin">Đăng nhập</button>
</template>
```

---

## 🧪 3. Thực hành

1. Cài đặt Router như hướng dẫn trên.
2. Tạo 2 Layout: Admin và Public.
3. Tạo Store `counter` đơn giản để test Pinia.
4. Chạy thử: Vào `/` thấy PublicLayout, vào `/admin/dashboard` thấy AdminLayout.
