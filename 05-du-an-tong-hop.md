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

# ➕ Dự án bổ sung (Object-Oriented Design & System Design mini)

> Đây là các đề **Low-Level Design (LLD)** hay gặp khi phỏng vấn backend. Mỗi dự án: vẽ UML → liệt kê use case → chọn cấu trúc dữ liệu → code in-memory → viết test.

## Dự án 7 ⭐⭐⭐ – Parking Lot (bãi đỗ xe nhiều tầng)
- Nhiều tầng, nhiều loại chỗ; cấp vé, tính phí theo giờ/loại xe, bảng hiển thị chỗ trống mỗi tầng.
- **OOP:** kế thừa `Vehicle`, Strategy tính phí, Singleton/DI cho `ParkingLot`. **DSA:** min-heap chọn chỗ gần lối vào nhất.
- 🔗 LeetCode: [1603. Design Parking System](https://leetcode.com/problems/design-parking-system/), [1845. Seat Reservation Manager](https://leetcode.com/problems/seat-reservation-manager/)

## Dự án 8 ⭐⭐⭐ – Vending Machine (máy bán hàng)
- Trạng thái: `Idle → HasMoney → Dispensing → Idle`; trả tiền thừa với ít đồng/tờ nhất; hết hàng/hết tiền thối.
- **OOP:** State pattern. **DSA:** Greedy/DP đổi tiền (Bài 4.21).
- 🔗 LeetCode: [2241. Design an ATM Machine](https://leetcode.com/problems/design-an-atm-machine/), [322. Coin Change](https://leetcode.com/problems/coin-change/)

## Dự án 9 ⭐⭐⭐⭐ – Elevator System
- N thang máy, M tầng; request trong/ngoài thang; thuật toán điều phối (SCAN/LOOK – giống disk scheduling).
- **OOP:** State (`MovingUp/MovingDown/Idle`), Strategy cho thuật toán điều phối. **DSA:** 2 heap (đi lên – min-heap, đi xuống – max-heap).
- Mô phỏng theo tick thời gian, in log hoạt động.

## Dự án 10 ⭐⭐⭐⭐ – Booking phòng họp / khách sạn
- Đặt phòng theo khoảng thời gian, phát hiện trùng, gợi ý phòng trống, hủy, đặt lặp lại hàng tuần.
- **DSA:** TreeMap/sorted list + binary search kiểm tra trùng; min-heap số phòng tối thiểu (Bài 4.25).
- **Concurrency:** 2 người đặt cùng phòng cùng lúc.
- 🔗 LeetCode: [729. My Calendar I](https://leetcode.com/problems/my-calendar-i/), [731. My Calendar II](https://leetcode.com/problems/my-calendar-ii/), [732. My Calendar III](https://leetcode.com/problems/my-calendar-iii/), [2402. Meeting Rooms III](https://leetcode.com/problems/meeting-rooms-iii/)

## Dự án 11 ⭐⭐⭐⭐ – Đặt vé xem phim (Movie Ticket Booking)
- Rạp → phòng chiếu → suất chiếu → ghế. **Giữ ghế tạm 10 phút** (seat lock có TTL), hết hạn tự nhả.
- **OOP:** Observer báo ghế được nhả; Strategy giá vé (ngày thường/cuối tuần/VIP). **DSA:** delayed queue nhả ghế (Bài 4.10).
- 🔗 LeetCode: [1912. Design Movie Rental System](https://leetcode.com/problems/design-movie-rental-system/), [1845. Seat Reservation Manager](https://leetcode.com/problems/seat-reservation-manager/)

## Dự án 12 ⭐⭐⭐⭐ – Mini Twitter / News Feed
- Đăng bài, follow/unfollow, timeline 10 bài mới nhất từ những người đang follow, like, hashtag trending.
- **DSA:** Merge K sorted lists bằng heap (fan-out on read) vs đẩy sẵn vào feed (fan-out on write); Top-K hashtag.
- **Câu hỏi:** Người nổi tiếng có 10 triệu follower thì chọn chiến lược nào?
- 🔗 LeetCode: [355. Design Twitter](https://leetcode.com/problems/design-twitter/), [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Dự án 13 ⭐⭐⭐⭐ – Chat App (1-1 và group)
- Gửi/nhận tin, trạng thái `sent/delivered/read`, online/offline, lịch sử phân trang theo cursor.
- **OOP:** Mediator (room), Observer (thông báo), Command (lệnh `/mute`, `/kick`). **DSA:** Queue tin nhắn offline, cursor pagination (Bài 3.22).
- **Nâng cao:** WebSocket server thật.

## Dự án 14 ⭐⭐⭐⭐ – Leaderboard realtime
- `addScore(player, delta)`, `top(k)`, `rank(player)`, leaderboard theo ngày/tuần.
- **DSA:** HashMap + TreeMap/Skip List (giống Redis Sorted Set); so sánh với heap.
- 🔗 LeetCode: [1244. Design A Leaderboard](https://leetcode.com/problems/design-a-leaderboard/) 🔒, [2353. Design a Food Rating System](https://leetcode.com/problems/design-a-food-rating-system/), [2102. Sequentially Ordinal Rank Tracker](https://leetcode.com/problems/sequentially-ordinal-rank-tracker/)

## Dự án 15 ⭐⭐⭐⭐⭐ – Mini Search Engine
- Index tài liệu: tokenize, bỏ dấu, stop words → **inverted index**; xếp hạng TF-IDF; autocomplete; sửa lỗi chính tả.
- **DSA:** HashMap → posting list (giao 2 list đã sort bằng two pointers); Trie; Edit Distance; Top-K heap.
- 🔗 LeetCode: [1268. Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/), [642. Design Search Autocomplete System](https://leetcode.com/problems/design-search-autocomplete-system/) 🔒, [72. Edit Distance](https://leetcode.com/problems/edit-distance/)

## Dự án 16 ⭐⭐⭐⭐ – In-memory File System / Cloud Storage mini
- `mkdir`, `ls`, `addContent`, `readContent`, `mv`, `rm -r`, tính dung lượng thư mục, phân quyền.
- **OOP:** Composite (File/Folder), Proxy (kiểm tra quyền). **DSA:** Trie theo path segment.
- 🔗 LeetCode: [588. Design In-Memory File System](https://leetcode.com/problems/design-in-memory-file-system/) 🔒, [1166. Design File System](https://leetcode.com/problems/design-file-system/) 🔒, [71. Simplify Path](https://leetcode.com/problems/simplify-path/)

## Dự án 17 ⭐⭐⭐⭐⭐ – Ví điện tử / Ledger
- Nạp, rút, chuyển tiền; **double-entry ledger** (mỗi giao dịch có bút toán Nợ/Có cân bằng); sao kê.
- **Idempotency key:** gửi lại cùng request không bị trừ tiền 2 lần. Chống race condition khi chuyển tiền chéo (lock ordering tránh deadlock).
- **OOP:** `Money` (Bài 1.10), Unit of Work (Bài 2.31), Command + audit log.
- 🔗 LeetCode: [2043. Simple Bank System](https://leetcode.com/problems/simple-bank-system/), [1226. The Dining Philosophers](https://leetcode.com/problems/the-dining-philosophers/)

## Dự án 18 ⭐⭐⭐⭐ – Splitwise (chia tiền nhóm)
- Thêm chi tiêu chia đều / theo % / theo số tiền cụ thể; xem ai nợ ai; **tối giản số giao dịch** thanh toán.
- **OOP:** Strategy cho cách chia. **DSA:** Graph nợ, greedy max-heap người nợ/người được nợ; backtracking cho lời giải tối ưu.
- 🔗 LeetCode: [465. Optimal Account Balancing](https://leetcode.com/problems/optimal-account-balancing/) 🔒

## Dự án 19 ⭐⭐⭐⭐⭐ – Metrics / Log Aggregator
- Nhận metric `(name, value, timestamp, tags)`; truy vấn `sum/avg/max/p95` theo khoảng thời gian và bucket (phút/giờ); cảnh báo khi vượt ngưỡng.
- **DSA:** Time-series lưu theo bucket, binary search theo thời gian, 2 heap cho median, Sliding Window Maximum, Fenwick tree.
- 🔗 LeetCode: [981. Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/), [1348. Tweet Counts Per Frequency](https://leetcode.com/problems/tweet-counts-per-frequency/), [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Dự án 20 ⭐⭐⭐⭐⭐ – Workflow Engine / CI Pipeline
- Định nghĩa pipeline dạng DAG (`build → test → [deploy-staging, security-scan] → deploy-prod`); chạy song song các bước độc lập; retry; dừng khi bước bắt buộc lỗi.
- **DSA:** Topological sort (Kahn), phát hiện chu trình, tính **critical path** (thời gian tối thiểu hoàn thành).
- **OOP:** Command cho mỗi step, Observer cho trạng thái, Template Method cho loại step.
- 🔗 LeetCode: [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/), [2050. Parallel Courses III](https://leetcode.com/problems/parallel-courses-iii/)

---

# 🧩 LeetCode cho các dự án gốc

| Dự án | Bài LeetCode luyện trước |
|-------|--------------------------|
| 1 – Thư viện | [208. Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/), [1845. Seat Reservation Manager](https://leetcode.com/problems/seat-reservation-manager/) |
| 2 – URL Shortener | [535. Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/), [146. LRU Cache](https://leetcode.com/problems/lru-cache/) |
| 3 – Rate Limiter | [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/), [359. Logger Rate Limiter](https://leetcode.com/problems/logger-rate-limiter/) 🔒, [362. Design Hit Counter](https://leetcode.com/problems/design-hit-counter/) 🔒 |
| 4 – Mini Redis | [146. LRU Cache](https://leetcode.com/problems/lru-cache/), [460. LFU Cache](https://leetcode.com/problems/lfu-cache/), [981. Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/), [1206. Design Skiplist](https://leetcode.com/problems/design-skiplist/) |
| 5 – E-commerce | [1357. Apply Discount Every n Orders](https://leetcode.com/problems/apply-discount-every-n-orders/), [1268. Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/), [743. Network Delay Time](https://leetcode.com/problems/network-delay-time/) |
| 6 – Job Scheduler | [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/), [1834. Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/), [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) |

> 🔒 = bài Premium.

---

## Sau khi xong – Chuẩn bị phỏng vấn
1. Viết README cho mỗi dự án: sơ đồ class (UML), quyết định thiết kế, Big-O các thao tác chính.
2. Luyện thêm trên LeetCode theo chủ đề: *Top Interview 150*, *Blind 75*.
3. Ôn câu hỏi lý thuyết: 4 tính chất OOP, SOLID, overload vs override, interface vs abstract, HashMap hoạt động thế nào, so sánh các thuật toán sort.
4. Tìm hiểu tiếp **System Design** cơ bản (cache, load balancer, DB index, message queue) – các bài 4.26–4.29 và dự án 2–4 là nền tảng trực tiếp.
