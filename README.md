# Bài 5 - Docker Compose và file `.env`

Bài tập triển khai hai dịch vụ bằng Docker Compose CLI v2:

- `backend`: HTTP service chạy ở cổng `8080` trong container.
- `database`: PostgreSQL 15, lưu dữ liệu bền vững trong named volume.

Docker Compose tự tạo network `backend-net` để hai container giao tiếp qua hostname
`database`. Backend chỉ khởi động sau khi PostgreSQL vượt qua health check.

## Chuẩn bị

Yêu cầu Docker Engine/Docker Desktop có hỗ trợ lệnh `docker compose`.

Tạo file cấu hình cục bộ từ file mẫu:

```bash
cp .env.example .env
```

Trên PowerShell:

```powershell
Copy-Item .env.example .env
```

Sửa `.env` và đặt mật khẩu mạnh trước khi chạy. File `.env` đã được khai báo trong
`.gitignore`, vì vậy thông tin nhạy cảm không được đưa lên GitHub.

## Khởi chạy và kiểm tra

Chạy các lệnh sau tại thư mục `homework/session_10/ex5/`:

```bash
docker compose up -d
docker compose ps
docker compose logs -f
```

Mở `http://localhost:8080/health` (hoặc cổng đã đặt trong `PORT`) để kiểm tra.
Kết quả thành công có dạng:

```json
{"service":"backend","status":"running","database":{"status":"reachable","host":"database","port":5432}}
```

Nhấn `Ctrl+C` để thoát chế độ theo dõi log. Dừng và xóa container/network bằng:

```bash
docker compose down
```

Dữ liệu PostgreSQL vẫn còn trong named volume. Chỉ khi chủ động muốn xóa cả dữ
liệu, dùng `docker compose down -v`.

## Cấu trúc

```text
ex5/
├── backend/
│   ├── Dockerfile
│   └── server.py
├── .env.example
├── .gitignore
├── docker-compose.yml
└── README.md
```
