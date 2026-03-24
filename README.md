# Cinema_CO2013

Ứng dụng quản lý và tra cứu dữ liệu rạp chiếu phim (môn Cơ sở dữ liệu), gồm:

- Backend Node.js + Express để gọi Stored Procedure/Function MySQL.
- Frontend HTML/CSS/JS để thao tác các chức năng truy vấn và cập nhật suất chiếu.

## 1) Cấu trúc dự án

```text
BE/
	server.js           # API gọi stored procedure/function
	fc.js               # Hàm JS dùng cho FE (gọi API, render bảng, sort, CRUD suất chiếu)
FE/
	index.html          # Trang menu chính
	style.css
	Final_CGV.sql       # Script tạo CSDL và dữ liệu logic
	Form/
		TimSuatChieu.html
		TinhDoanhThuSuatChieu.html
		GetTopPhim.html
		insert_showtime.html
package.json
```

## 2) Yêu cầu môi trường

- Node.js (khuyến nghị >= 18)
- MySQL 8.x (có hỗ trợ `JSON_TABLE`)
- npm

## 3) Cài đặt

Tại thư mục gốc dự án:

```bash
npm install
```

Dependencies hiện tại:

- `express`
- `mysql2`
- `cors`

## 4) Khởi tạo cơ sở dữ liệu

1. Mở MySQL client (MySQL Workbench/CLI).
2. Chạy file `FE/Final_CGV.sql` để tạo schema, bảng, ràng buộc và các thủ tục/hàm liên quan.

> Lưu ý quan trọng:
>
> - Trong `FE/Final_CGV.sql` tên database là `CGV_BTL`.
> - Trong `BE/server.js` đang cấu hình:
>   - `database: 'cinema'`
>   - `ROUTINE_SCHEMA = 'CINEMA'`
>
> Bạn cần thống nhất tên DB (ví dụ đổi cả hai về `CGV_BTL`) để API gọi đúng stored procedure/function.

## 5) Cấu hình backend

Mở `BE/server.js`, cập nhật phần kết nối MySQL theo máy của bạn:

```js
const db = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "matkhau_cua_ban",
  database: "CGV_BTL",
});
```

Đồng thời sửa `ROUTINE_SCHEMA` cho khớp:

```sql
WHERE ROUTINE_SCHEMA = 'CGV_BTL'
```

## 6) Chạy ứng dụng

### Bước 1: chạy backend

```bash
node BE/server.js
```

Backend mặc định chạy tại:

```text
http://localhost:3000
```

API chính:

```text
GET /call-function?proc=<ten>&params=<json-array>&func=<mode>
```

Trong đó `func` có các mode:

- `False`: gọi stored procedure
- `True`: gọi stored function
- `Jason`: xử lý các hàm trả JSON (ví dụ Top phim, thống kê doanh thu theo ngày)
- `INSERT`, `UPDATE`, `DELETE`: thao tác suất chiếu

### Bước 2: mở frontend

Mở file `FE/index.html` bằng trình duyệt (hoặc Live Server trong VS Code).

Các chức năng chính từ menu:

- **Tìm, Sửa, Xóa suất chiếu** (`Form/TimSuatChieu.html`)
- **Tính doanh thu rạp theo khoảng ngày** (`Form/TinhDoanhThuSuatChieu.html`)
- **Top 3 phim theo doanh thu** (`Form/GetTopPhim.html`)
- **Thêm suất chiếu** (`Form/insert_showtime.html`)

## 7) Luồng hoạt động tổng quát

1. FE lấy input từ form.
2. `Form.js` tạo `proc` + `params`.
3. `fc.js` gửi request tới `http://localhost:3000/call-function`.
4. `server.js` kiểm tra tên routine hợp lệ và gọi MySQL.
5. Kết quả trả về FE để render bảng/hiển thị thông báo.

## 8) Lỗi thường gặp

- **Không kết nối được DB**
  - Kiểm tra `host/user/password/database` trong `BE/server.js`.
  - Đảm bảo MySQL service đang chạy.

- **API báo thủ tục/hàm không hợp lệ**
  - Kiểm tra tên `ROUTINE_SCHEMA` trong query lấy routines.
  - Đảm bảo procedure/function đã được tạo trong đúng schema.

- **CORS hoặc FE không gọi được API**
  - Đảm bảo backend đang chạy cổng `3000`.
  - Truy cập thử trực tiếp: `http://localhost:3000/call-function`.

- **Lỗi `require is not defined` trên FE**
  - Do trang HTML đang include `../../BE/server.js` (file backend Node.js).
  - Nếu gặp lỗi này, hãy bỏ thẻ `<script src="../../BE/server.js"></script>` trong các file HTML FE.

## 9) Ghi chú

- Dự án chưa khai báo script trong `package.json`, nên chạy trực tiếp bằng `node BE/server.js`.
- Nếu muốn, có thể bổ sung script npm để chạy nhanh hơn (`start`, `dev`).
