# 🗄️ CƠ SỞ DỮ LIỆU DNU SHOP

## 1. Sơ đồ ERD (Entity Relationship Diagram)

```mermaid
erDiagram
    AspNetUsers ||--o{ Orders : "places"
    Categories ||--o{ Products : "contains"
    Products ||--o{ OrderItems : "includes"
    Orders ||--o{ OrderItems : "has"

    AspNetUsers {
        string Id PK
        string UserName
        string Email
        string PasswordHash
    }

    Categories {
        int Id PK
        string Name
    }

    Products {
        int Id PK
        string Name
        decimal Price
        string ImageUrl
        int CategoryId FK
    }

    Orders {
        int Id PK
        string UserId FK
        datetime CreatedDate
        decimal TotalAmount
        int Status "0:New, 1:Shipping, 2:Done"
    }

    OrderItems {
        int Id PK
        int OrderId FK
        int ProductId FK
        int Quantity
        decimal UnitPrice
    }
```

## 2. Giải thích bảng

### Products (Sản phẩm)
- `ImageUrl`: Lưu đường dẫn tương đối (VD: `/images/iphone.jpg`).
- `Price`: Kiểu `decimal(18,2)` để tránh sai số tiền tệ.

### Orders (Đơn hàng)
- `Status`: Dùng Enum trong C# để quản lý trạng thái.
- `TotalAmount`: Tổng tiền đơn hàng (đã trừ khuyến mãi nếu có).

### OrderItems (Chi tiết đơn hàng)
- `UnitPrice`: Lưu giá tại thời điểm mua (đề phòng giá sản phẩm thay đổi sau này).
