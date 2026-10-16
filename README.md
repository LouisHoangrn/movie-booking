# 🎬 CineBook — Hệ thống Đặt Vé Xem Phim Trực Tuyến

<div align="center">

[![Node.js](https://img.shields.io/badge/Node.js-18.x-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express.js](https://img.shields.io/badge/Express.js-5.x-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-7.x-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![EJS](https://img.shields.io/badge/View-EJS-B4CA65?style=for-the-badge)](https://ejs.co/)

*Ứng dụng web đặt vé xem phim trực tuyến — từ chọn phim, chọn suất chiếu, chọn ghế đến thanh toán QR.*

</div>

---

## 📋 Mục lục

- [Giới thiệu](#-giới-thiệu)
- [Tính năng](#-tính-năng)
- [Tech Stack](#-tech-stack)
- [Cấu trúc dự án](#-cấu-trúc-dự-án)
- [Hướng dẫn cài đặt & chạy](#-hướng-dẫn-cài-đặt--chạy)
- [Tài khoản trải nghiệm](#-tài-khoản-trải-nghiệm)
- [Danh sách Routes](#-danh-sách-routes)
- [Nhóm phát triển](#-nhóm-phát-triển)

---

## 📖 Giới thiệu

**CineBook** là ứng dụng web đặt vé xem phim trực tuyến xây dựng theo mô hình **MVC** (Model – View – Controller), sử dụng **Node.js + Express.js** ở tầng backend, **MongoDB** làm cơ sở dữ liệu và **EJS** làm template engine render phía server.

Hệ thống hỗ trợ 2 vai trò người dùng (**User** và **Admin**) với luồng nghiệp vụ hoàn chỉnh: từ duyệt phim đang chiếu / sắp chiếu, chọn suất chiếu, đặt ghế theo sơ đồ phòng thực tế, thêm combo bắp nước, đến thanh toán qua mã **VietQR** và nhận vé điện tử.

---

## 🌟 Tính năng

### 🎭 Người dùng (User)

- Xem danh sách **phim đang chiếu** và **phim sắp chiếu** trên trang chủ.
- Xem chi tiết phim: mô tả, đạo diễn, diễn viên, thể loại, thời lượng, trailer YouTube.
- Chọn **suất chiếu** theo phim, lọc theo ngày / rạp / phòng.
- **Sơ đồ ghế tương tác**: phân biệt ghế Thường / VIP / Đôi, hiển thị ghế đã đặt và ghế trống theo thời gian thực.
- Thêm **combo dịch vụ** (Bắp rang bơ, Nước ngọt…) khi xác nhận đặt vé.
- Thanh toán bằng **mã QR VietQR** (ngân hàng MB, tự động điền số tiền và nội dung).
- Nhận **vé điện tử** kèm mã QR xác thực sau thanh toán.
- Xem **lịch sử đặt vé** cá nhân (trạng thái: Chờ thanh toán / Đã thanh toán / Đã hủy).
- **Hủy vé** khi đơn còn ở trạng thái `pending`.

### 🛠 Quản trị viên (Admin)

- **Dashboard** thống kê: tổng phim, suất chiếu, lượt đặt vé, doanh thu toàn hệ thống.
- **Quản lý Phim** (CRUD): thêm, sửa, xóa phim; đổi trạng thái (đang chiếu / sắp chiếu / đã kết thúc); upload poster.
- **Quản lý Rạp chiếu** (CRUD): thêm, sửa, xóa rạp theo thành phố.
- **Quản lý Phòng chiếu** (CRUD): cấu hình số hàng, số cột, hàng ghế VIP, hàng ghế đôi.
- **Quản lý Suất chiếu** (CRUD): tạo lịch chiếu gắn với phim, phòng, rạp và hệ số giá.
- **Quản lý Đặt vé**: xem danh sách toàn bộ booking, cập nhật trạng thái.
- **Quản lý Người dùng**: xem danh sách, thay đổi quyền (user ↔ admin).

---

## 🛠 Tech Stack

| Layer | Công nghệ |
|---|---|
| **Runtime** | Node.js 18+ |
| **Web Framework** | Express.js 5.x |
| **Database** | MongoDB 7.x (Mongoose ODM 9.x) |
| **Template Engine** | EJS 5.x |
| **Session** | express-session + connect-mongo |
| **Mã hóa mật khẩu** | bcrypt / bcryptjs |
| **Upload ảnh** | Multer 2.x |
| **Môi trường** | dotenv |
| **Dev tool** | nodemon |

---

## 🏗 Cấu trúc dự án

```text
movie-booking/
├── app.js                          # Entry point — khởi tạo Express, cấu hình middleware & routes
├── seed.js                         # Seed phim, rạp, phòng chiếu, suất chiếu
├── seedUser.js                     # Seed tài khoản admin và user mẫu
├── package.json
│
├── config/
│   └── db.js                       # Kết nối MongoDB
│
├── models/                         # Mongoose Schemas
│   ├── User.js                     # Người dùng: username, email, password (bcrypt), role
│   ├── Movie.js                    # Phim: title, genre, duration, status, basePrice, posterUrl
│   ├── Cinema.js                   # Rạp chiếu: name, address, city
│   ├── Room.js                     # Phòng chiếu: rowCount, colCount, vipRows, coupleRows
│   ├── Showtime.js                 # Suất chiếu: movieId, roomId, cinemaId, startTime, priceMultiplier
│   └── Booking.js                  # Đặt vé: tickets (ghế), services (combo), totalAmount, status, ticketCode
│
├── controllers/
│   ├── authController.js           # Đăng ký, Đăng nhập, Đăng xuất
│   ├── movieController.js          # Danh sách phim, Chi tiết phim
│   ├── showtimeController.js       # Danh sách suất chiếu theo phim
│   ├── bookingController.js        # Chọn ghế, Checkout, Thanh toán QR, Vé điện tử, Lịch sử, Hủy
│   └── admin/
│       ├── DashboardController.js  # Thống kê tổng quan
│       ├── MovieController.js      # CRUD phim + upload poster
│       ├── CinemaController.js     # CRUD rạp chiếu
│       ├── RoomController.js       # CRUD phòng chiếu
│       ├── ShowtimeController.js   # CRUD suất chiếu
│       ├── BookingController.js    # Xem & cập nhật trạng thái đặt vé
│       └── UserController.js       # Xem & phân quyền người dùng
│
├── routes/
│   ├── authRoutes.js               # /auth/*
│   ├── movieRoutes.js              # /movies/*
│   ├── showtimeRoutes.js           # /showtimes/*
│   ├── bookingRoutes.js            # /bookings/*
│   ├── adminRoutes.js              # /admin/* (yêu cầu quyền admin)
│   └── homeRoutes.js
│
├── middleware/
│   ├── auth.js                     # requireLogin, requireAdmin — kiểm tra session & phân quyền
│   └── upload.js                   # Multer cấu hình upload poster phim
│
├── services/                       # Business logic tách biệt
├── utils/                          # Hàm tiện ích: buildSeatMap, generateTicketCode, buildTicketQrUrl
│
├── views/                          # EJS Templates
│   ├── home.ejs                    # Trang chủ
│   ├── layouts/                    # Header, Footer dùng chung
│   ├── auth/                       # Đăng nhập, Đăng ký
│   ├── movies/                     # Danh sách phim (index.ejs), Chi tiết phim (show.ejs)
│   ├── showtimes/                  # Chọn suất chiếu
│   ├── bookings/                   # Sơ đồ ghế, Xác nhận, Thanh toán QR, Vé điện tử, Lịch sử
│   └── admin/                      # Dashboard, Movies, Cinemas, Rooms, Showtimes, Bookings, Users
│
└── public/                         # Static: CSS, JS client-side, ảnh poster, assets
```

---

## 🚀 Hướng dẫn Cài đặt & Chạy

### Yêu cầu hệ thống

- [Node.js](https://nodejs.org/) >= 18.x
- [MongoDB](https://www.mongodb.com/try/download/community) >= 7.x (local) hoặc [MongoDB Atlas](https://www.mongodb.com/atlas) (cloud)
- [Git](https://git-scm.com/)

---

### Bước 1 — Clone dự án

```bash
git clone https://github.com/trvannhan/movie-booking.git
cd movie-booking
```

### Bước 2 — Cài đặt dependencies

```bash
npm install
```

### Bước 3 — Cấu hình môi trường

Tạo file `.env` tại thư mục gốc:

```env
PORT=3000
MONGO_URI=mongodb://localhost:27017/movie_booking
SESSION_SECRET=cinebook_secret_2024
```

> Nếu dùng **MongoDB Atlas**, thay `MONGO_URI` bằng connection string của Atlas.

### Bước 4 — Nạp dữ liệu mẫu

```bash
# Nạp phim, rạp, phòng chiếu, suất chiếu
npm run seed

# Nạp tài khoản admin và user
node seedUser.js
```

> Để **reset toàn bộ** dữ liệu và nạp lại từ đầu:
> ```bash
> RESET_DB=true node seed.js
> ```

### Bước 5 — Khởi chạy server

```bash
# Production
npm start

# Development (tự reload khi sửa code)
npm run dev
```

> Truy cập tại: **`http://localhost:3000`**

---

## 🔐 Tài khoản Trải nghiệm

| Username | Email | Mật khẩu | Vai trò |
|---|---|---|---|
| `admin` | `admin@gmail.com` | `admin` | **Admin** — toàn quyền quản trị |
| `user` | `user@gmail.com` | `user` | **User** — đặt vé, xem lịch sử |

> Tài khoản được tạo bởi script `seedUser.js`.

---

## 📡 Danh sách Routes

| Route | Phương thức | Chức năng |
|---|---|---|
| `/` | GET | Trang chủ — phim đang chiếu & sắp chiếu |
| `/movies` | GET | Danh sách toàn bộ phim |
| `/movies/:id` | GET | Chi tiết phim + trailer |
| `/showtimes/:movieId` | GET | Chọn suất chiếu theo phim |
| `/bookings/seat/:showtimeId` | GET | Sơ đồ ghế tương tác |
| `/bookings/checkout` | POST | Xác nhận chọn ghế + combo → tạo booking |
| `/bookings/:id/payment` | GET | Trang thanh toán QR VietQR |
| `/bookings/:id/confirm` | POST | Xác nhận thanh toán → đổi trạng thái `paid` |
| `/bookings/:id/ticket` | GET | Vé điện tử sau thanh toán |
| `/bookings/history` | GET | Lịch sử đặt vé cá nhân |
| `/bookings/cancel/:id` | GET | Hủy vé (chỉ khi `pending`) |
| `/auth/register` | GET / POST | Đăng ký tài khoản |
| `/auth/login` | GET / POST | Đăng nhập |
| `/auth/logout` | GET | Đăng xuất |
| `/admin/dashboard` | GET | Thống kê tổng quan |
| `/admin/movies` | GET / POST | Quản lý phim |
| `/admin/cinemas` | GET / POST | Quản lý rạp chiếu |
| `/admin/rooms` | GET / POST | Quản lý phòng chiếu |
| `/admin/showtimes` | GET / POST | Quản lý suất chiếu |
| `/admin/bookings` | GET / POST | Quản lý đặt vé |
| `/admin/users` | GET / POST | Quản lý người dùng |

---

## 👥 Nhóm phát triển

- Nguyễn Vũ Hoàng Long
- Trần Văn Nhân
- Trần Văn Lực

---

<div align="center">

*Được xây dựng với ❤️ — CineBook Team*

</div>
