# Thông Tin Deploy — Checkpoint 5

## Thông Tin Học Viên

| Mục | Nội dung |
|-----|----------|
| Họ và tên | Phan Văn Hoàng Nam |
| Mã học viên | 2A202601160 |
| Repo | https://github.com/mazzaw24/K4-DAY12-2A202601160-PhanVanHoangNam |

## Service

| Mục | Nội dung |
|-----|----------|
| Public URL | https://chat-production-25c2.up.railway.app |
| Platform | Railway |
| Ngày deploy | 2026-08-10 |

## Biến Môi Trường Đã Set Trên Cloud

Chỉ liệt kê tên biến và nguồn cấu hình; giá trị `API_TOKEN` không nằm trong tài liệu hoặc repository.

| Biến | Đã set | Ghi chú |
|------|--------|---------|
| `PORT` | ✅ | Railway tự gán |
| `API_TOKEN` | ✅ | Secret đặt trên Railway từ môi trường local |
| `REDIS_URL` | ✅ | Tham chiếu `Redis.REDIS_URL` từ Railway Redis service |
| `BUCKET_CAPACITY` | ✅ | Cấu hình trên Railway |
| `REFILL_PER_MINUTE` | ✅ | Cấu hình trên Railway |
| `DAILY_BUDGET_USD` | ✅ | Cấu hình trên Railway |
| `LOG_LEVEL` | ✅ | Cấu hình trên Railway |

## Lệnh Kiểm Tra

```bash
URL=https://chat-production-25c2.up.railway.app

curl -i "$URL/healthz"
curl -i "$URL/readyz"

curl -i -X POST "$URL/chat" \
  -H "Content-Type: application/json" \
  -d '{"message":"Hello"}'

curl -i -X POST "$URL/chat" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $API_TOKEN" \
  -H "X-Client-Id: sv-test" \
  -d '{"message":"Deploy là gì?"}'
```

## Kết Quả Chạy Thật

```text
GET /healthz
HTTP 200
{"status":"ok","service":"day12-chat-service","version":"1.0.0"}

GET /readyz
HTTP 200
{"status":"ready","redis":true}

POST /chat — không có token
HTTP 401

POST /chat — có Bearer token hợp lệ
HTTP 200
reply_present=true

15 request liên tiếp với cùng client:
200 200 200 200 200 200 200 200 200 200 429 429 429 429 429
```

## Railway

- Project: `k4-day12-phanvanhoangnam`
- Service: `chat`
- Redis service: `Redis`
- Deployment: thành công
- Region: Amsterdam (`ams`)
- Healthcheck: `/healthz`

## Ảnh Chụp Màn Hình

- `screenshots/healthz.png`: kết quả `/healthz` từ public Railway URL.
- `screenshots/dashboard.png`: cần chụp thủ công từ dashboard Railway trong phiên trình duyệt đã đăng nhập trước khi nộp bài.

Không có secret nào được phép xuất hiện trong ảnh.
