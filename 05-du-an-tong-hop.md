# Phần 5 – Dự án tổng hợp (OOP + DSA + Backend)

Mỗi dự án nên làm trong 3–7 ngày, **ban đầu không dùng framework** (thuần ngôn ngữ, lưu dữ liệu in-memory) để tập trung vào thiết kế. Sau đó có thể port lên Spring Boot / NestJS / FastAPI.

---

## Dự án 1 ⭐⭐⭐⭐ – Thư viện mini (Library Management)
**Chức năng:** Quản lý sách, độc giả, mượn/trả, phí trễ hạn, đặt trước sách đang được mượn.

| Kiến thức | Áp dụng |
|-----------|---------|
| OOP | `Book`, `Member`, `Loan`, `Reservation`; `FineStrategy` (Strategy) |
| DSA | HashMap tra cứu ISBN; Queue cho hàng đợi đặt trước; Trie tìm sách theo tiền tố tên |
| Pattern | Observer: báo cho người đặt trước khi sách được trả |

---

## Dự án 2 ⭐⭐⭐⭐ – URL Shortener
**Chức năng:** `shorten(longUrl) -> shortCode`, `resolve(shortCode)`, thống kê click, hết hạn link.

| Kiến thức | Áp dụng |
|-----------|---------|
| DSA | Base62 encoding; HashMap 2 chiều; LRU Cache cho link hot; Heap lấy top link |
| OOP | `IdGenerator` interface (Counter / Random / Snowflake) |
| Câu hỏi thiết kế | Xử lý collision? Scale lên nhiều server thì sinh ID thế nào? |

---

## Dự án 3 ⭐⭐⭐⭐ – Rate Limiter Library
Thư viện hỗ trợ nhiều thuật toán, chọn qua config:
- Fixed Window, Sliding Log, Sliding Window Counter, **Token Bucket**, Leaky Bucket.
- Interface `RateLimiter.allow(clientId) -> bool`; Factory tạo theo config.
- Viết test giả lập thời gian (inject `Clock` để test – áp dụng DIP).
- Benchmark bộ nhớ & tốc độ từng thuật toán.

---

## Dự án 4 ⭐⭐⭐⭐⭐ – In-memory Key-Value Store (Mini Redis)
**Lệnh hỗ trợ:** `SET`, `GET`, `DEL`, `EXPIRE`, `TTL`, `INCR`, `LPUSH/RPOP` (list), `ZADD/ZRANGE` (sorted set).

| Kiến thức | Áp dụng |
|-----------|---------|
| DSA | HashMap; Heap/sorted structure cho key hết hạn; Skip List hoặc TreeMap cho sorted set; LRU eviction khi đầy |
| OOP | Command Pattern cho mỗi lệnh; Parser tách riêng |
| Nâng cao | Ghi log lệnh ra file (AOF) và khôi phục khi khởi động; mở TCP socket nhận lệnh |

---

## Dự án 5 ⭐⭐⭐⭐⭐ – Hệ thống E-commerce mini
**Chức năng:** Sản phẩm (cây danh mục), giỏ hàng, mã giảm giá, đặt hàng, thanh toán, tồn kho, thông báo.

| Kiến thức | Áp dụng |
|-----------|---------|
| Kiến trúc | Controller / Service / Repository / DTO (Bài 2.15) |
| OOP | State (đơn hàng), Strategy (giảm giá, phí ship), Adapter (payment gateway), Observer (event bus) |
| DSA | Cây danh mục; Trie autocomplete; Heap sản phẩm bán chạy; Dijkstra chọn kho gần nhất |
| Concurrency | 2 người cùng mua sản phẩm cuối cùng → tránh bán âm kho (lock / optimistic version) |

---

## Dự án 6 ⭐⭐⭐⭐⭐ – Job Scheduler (Mini Cron/BullMQ)
- Đăng ký job chạy 1 lần, chạy trễ, chạy định kỳ.
- Priority Queue theo thời gian chạy; worker pool nhiều thread.
- Retry với exponential backoff; dead-letter queue cho job lỗi quá N lần.
- Job có phụ thuộc → Topological Sort.

---

## Sau khi xong – Chuẩn bị phỏng vấn
1. Viết README cho mỗi dự án: sơ đồ class (UML), quyết định thiết kế, Big-O các thao tác chính.
2. Luyện thêm trên LeetCode theo chủ đề: *Top Interview 150*, *Blind 75*.
3. Ôn câu hỏi lý thuyết: 4 tính chất OOP, SOLID, overload vs override, interface vs abstract, HashMap hoạt động thế nào, so sánh các thuật toán sort.
4. Tìm hiểu tiếp **System Design** cơ bản (cache, load balancer, DB index, message queue) – các bài 4.26–4.29 và dự án 2–4 là nền tảng trực tiếp.
