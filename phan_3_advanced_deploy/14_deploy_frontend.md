# 🟨 TUẦN 14: DEPLOY FRONTEND

## 🎯 Mục tiêu
- Build Vue project ra file tĩnh (HTML/CSS/JS).
- Deploy lên các nền tảng miễn phí (Vercel/Netlify) hoặc Nginx.

---

## 📦 1. Build Production

```powershell
npm run build
```
Kết quả: Thư mục `dist` được tạo ra. Đây là tất cả những gì cần để chạy web.

---

## ☁️ 2. Deploy lên Vercel (Khuyên dùng)

Cách nhanh nhất để có link demo nộp bài.

1. Đẩy code lên GitHub.
2. Vào [Vercel.com](https://vercel.com) -> Login bằng GitHub.
3. Chọn "Add New Project" -> Chọn Repo `dnu-shop-client`.
4. Vercel tự nhận diện là Vue/Vite. Bấm **Deploy**.
5. Nhận link: `https://dnu-shop-client.vercel.app`.

---

## 🕸️ 3. Deploy lên Nginx (Server riêng)

Nếu deploy trên VPS (Ubuntu/CentOS):

1. Cài Nginx: `sudo apt install nginx`.
2. Copy thư mục `dist` lên server (VD: `/var/www/dnushop`).
3. Cấu hình Nginx (`/etc/nginx/sites-available/default`):

```nginx
server {
    listen 80;
    server_name my-shop.com;

    location / {
        root /var/www/dnushop;
        index index.html;
        try_files $uri $uri/ /index.html; # Quan trọng cho SPA Router
    }
}
```

---

## 🧪 4. Thực hành

1. Build dự án ra thư mục `dist`.
2. Deploy lên Vercel để lấy link public.
3. Gửi link cho bạn bè test thử.
