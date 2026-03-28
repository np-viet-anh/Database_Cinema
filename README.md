# The Greenmart Vietnam

Website bán hàng sản phẩm xanh xây dựng theo mô hình **MVC thuần PHP**.
Dự án có 2 khu vực chính:

- **Client**: xem sản phẩm, tin tức, giỏ hàng, đặt hàng, liên hệ, FAQ.
- **Admin**: quản lý dashboard, sản phẩm, đơn hàng, tin tức, FAQ, liên hệ và cài đặt website.

---

## 1) Công nghệ sử dụng

- PHP (PDO)
- MySQL
- HTML/CSS/JS
- Mô hình MVC tự xây dựng (không dùng framework)

---

## 2) Cấu trúc thư mục chính

```text
The-Greenmart-Vietnam/
├── index.php                # Front Controller + Router
├── database.sql             # Cấu trúc và dữ liệu mẫu
├── config/
│   └── database.php         # Cấu hình kết nối DB
├── controllers/             # Xử lý request
├── models/                  # Tương tác dữ liệu
├── views/                   # Giao diện client/admin
└── assets/                  # Hình ảnh, uploads
```

---

## 5.1) Yêu cầu môi trường

- Web Server: Apache (Khuyên dùng XAMPP hoặc Laragon)
- PHP Version: 7.4 hoặc 8.0 trở lên
- Database: MySQL / MariaDB
- Trình duyệt: Chrome, Firefox, Edge phiên bản mới nhất

---

## 5.2) Các bước cài đặt

1. Bước 1: Tải mã nguồn về và giải nén vào thư mục `htdocs` (XAMPP) hoặc `www` (Laragon).
2. Bước 2: Mở phpMyAdmin, tạo cơ sở dữ liệu mới tên `database`.
3. Bước 3: Import file `database.sql` (có sẵn trong thư mục code) vào database vừa tạo.
4. Bước 4: Mở file `config/database.php`, cấu hình thông tin kết nối (User: `root`, Password: rỗng).
5. Bước 5: Truy cập trình duyệt theo đường dẫn `http://localhost`.

---

## 6) Tài khoản mẫu

Tài khoản Admin mặc định:

- Email: `admin@gmail.com`
- Mật khẩu: `123456`

Dữ liệu có sẵn trong `database.sql` vẫn bao gồm tài khoản User:

- Email: `khach@gmail.com`
- Mật khẩu: `123456`

---

## 7) Cơ chế định tuyến

Dự án dùng query string với format:

```text
index.php?controller=<ten_controller>&action=<ten_action>
```

Ví dụ:

- Trang chủ: `index.php`
- Đăng nhập: `index.php?controller=auth&action=login`
- Đăng ký: `index.php?controller=auth&action=register`
- Dashboard admin: `index.php?controller=admin&action=dashboard`

---

## 8) Một số lưu ý

- Thư mục upload avatar: `assets/uploads/`
- Router mặc định ở `index.php` với controller mặc định `HomeController`, action mặc định `index`.
- Nếu truy cập route admin mà chưa đăng nhập admin, hệ thống sẽ chuyển về trang đăng nhập.

---

## 9) Tác giả / Nhóm phát triển

Nhóm 2 thành viên.
