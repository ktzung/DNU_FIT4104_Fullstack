# 📋 YÊU CẦU DỰ ÁN DNU SHOP

## 1. Tổng quan
Xây dựng hệ thống thương mại điện tử phục vụ nhu cầu mua sắm trực tuyến và quản lý kho hàng.

## 2. Phân hệ Khách hàng (Storefront)
- **Xem danh sách sản phẩm**: Hiển thị dạng lưới, có phân trang (10 SP/trang).
- **Tìm kiếm**: Theo tên sản phẩm.
- **Lọc**: Theo danh mục, theo khoảng giá.
- **Chi tiết sản phẩm**: Xem ảnh lớn, mô tả, giá, số lượng tồn kho.
- **Giỏ hàng**: Thêm/Sửa/Xóa sản phẩm, tính tổng tiền tự động.
- **Đặt hàng**: Nhập thông tin giao hàng (Tên, SĐT, Địa chỉ) -> Lưu đơn hàng.

## 3. Phân hệ Quản trị (Admin Portal)
- **Đăng nhập**: Chỉ Admin mới được vào.
- **Dashboard**:
    - Tổng doanh thu tháng này.
    - Số đơn hàng mới chưa duyệt.
    - Top 5 sản phẩm bán chạy.
- **Quản lý Sản phẩm**:
    - Xem danh sách (Table).
    - Thêm mới (Upload ảnh).
    - Sửa thông tin.
    - Xóa (Soft delete - chỉ ẩn đi).
- **Quản lý Đơn hàng**:
    - Xem danh sách đơn hàng.
    - Cập nhật trạng thái: Mới -> Đang giao -> Hoàn thành / Hủy.

## 4. Yêu cầu Phi chức năng
- **Giao diện**: Responsive (dùng tốt trên điện thoại).
- **Bảo mật**: Password phải được mã hóa (Hash). API phải có Token.
- **Hiệu năng**: Tải trang dưới 2 giây.
