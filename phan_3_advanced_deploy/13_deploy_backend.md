# 🟨 TUẦN 13: DEPLOY BACKEND (IIS/DOCKER)

## 🎯 Mục tiêu
- Publish ứng dụng .NET ra file chạy được.
- Deploy lên IIS (Windows Server) hoặc Docker.

---

## 📦 1. Publish

Mở Terminal tại thư mục API:
```powershell
dotnet publish -c Release -o ./publish
```
Kết quả: Thư mục `publish` chứa file `.dll` và `.exe`.

---

## 🌐 2. Deploy lên IIS (Windows)

1. Cài đặt **IIS** trên Windows (Turn Windows features on or off).
2. Cài đặt **.NET Core Hosting Bundle** (để IIS hiểu được .NET Core).
3. Mở **IIS Manager** -> Add Website.
   - Site name: `DNUShopAPI`
   - Physical path: Chọn thư mục `publish` vừa tạo.
   - Port: 8080.
4. Truy cập `http://localhost:8080/swagger` để kiểm tra.

---

## 🐳 3. Deploy bằng Docker (Linux/Cloud)

### 3.1. Dockerfile
```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY ./publish .
ENTRYPOINT ["dotnet", "DNU.Shop.API.dll"]
```

### 3.2. Build & Run
```powershell
docker build -t dnushop-api .
docker run -d -p 5000:8080 dnushop-api
```

---

## 🧪 4. Thực hành

1. Thực hiện Publish dự án.
2. (Lựa chọn) Cài IIS trên máy cá nhân và deploy thử.
3. Hoặc cài Docker Desktop và chạy container.
