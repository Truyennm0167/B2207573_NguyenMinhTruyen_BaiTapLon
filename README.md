# B2207573 Nguyễn Minh Truyền

# Quản Lý Mượn Sách

Đây là dự án web ứng dụng quản lý mượn sách thư viện, gồm hai phần: **backend** (Node.js/Express) và **frontend** (Vue 3 + Vite).

## Tính năng

- Quản lý sách, độc giả, nhân viên, nhà xuất bản
- Đăng nhập, xác thực người dùng
- Quản lý mượn/trả sách, theo dõi lịch sử mượn sách
- Upload và quản lý ảnh bìa sách
- Giao diện thân thiện, dễ sử dụng

## Cấu trúc dự án

```
├── backend/   # API server (Node.js/Express)
├── frontend/  # Giao diện người dùng (Vue 3 + Vite)
```

## Hướng dẫn chạy dự án trên local

### 1. Khởi động backend

```sh
cd backend
npm install
node server.js
```

- Server backend mặc định chạy ở http://localhost:3000

### 2. Khởi động frontend

```sh
cd frontend
npm install
npm run dev
```

- Ứng dụng frontend mặc định chạy ở http://localhost:5173

### 3. Cấu hình

- Có thể cần tạo file `.env` trong thư mục `backend` để cấu hình kết nối database (nếu có).
- Ảnh upload sẽ được lưu trong thư mục `backend/uploads/sach/`.

## Công nghệ sử dụng

- **Backend:** Node.js, Express, MongoDB/Mongoose
- **Frontend:** Vue 3, Vite, Vuex, Vue Router, Bootstrap 5

---

> Để biết thêm chi tiết về từng phần, vui lòng xem README trong từng thư mục `backend` và `frontend`.
