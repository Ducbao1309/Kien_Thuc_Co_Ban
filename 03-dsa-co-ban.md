# Phần 3 – DSA Cơ bản (bối cảnh Backend)

> Với mỗi bài: viết lời giải brute-force trước, sau đó tối ưu và ghi rõ Big-O.

## Bảng Big-O cần nhớ
| Cấu trúc | Truy cập | Tìm kiếm | Thêm | Xóa |
|----------|---------|---------|------|-----|
| Array | O(1) | O(n) | O(n) | O(n) |
| HashMap | – | O(1) TB | O(1) TB | O(1) TB |
| Linked List | O(n) | O(n) | O(1)* | O(1)* |
| Stack/Queue | – | – | O(1) | O(1) |
| BST cân bằng | – | O(log n) | O(log n) | O(log n) |
| Heap | O(1) top | O(n) | O(log n) | O(log n) |

---

## 1. Array & String

### Bài 3.1 ⭐ – Phân tích log truy cập
Input: danh sách dòng log `"2026-11-25 10:00:01 GET /api/users 200 35ms"`.
- Đếm số request theo status code.
- Tính thời gian phản hồi trung bình, max, min.
- Lấy ra các request > 500ms.

### Bài 3.2 ⭐ – Two Sum (bài kinh điển)
Cho danh sách giá sản phẩm và ngân sách `target`. Tìm 2 sản phẩm có tổng giá đúng bằng `target`.
- Brute force O(n²) → HashMap O(n).
```
prices = [120, 450, 300, 80, 200], target = 500 -> (300, 200), chỉ số [2, 4]
```

### Bài 3.3 ⭐ – Slug generator
`"Lập Trình Backend với Java!!"` → `"lap-trinh-backend-voi-java"`. (Bỏ dấu tiếng Việt, ký tự đặc biệt, gộp dấu `-`.)

### Bài 3.4 ⭐⭐ – Merge khoảng thời gian (Merge Intervals)
Lịch đặt phòng họp `[[9,10],[9.5,11],[13,14],[13.5,15]]` → gộp thành `[[9,11],[13,15]]`.
- **Mở rộng:** Kiểm tra một booking mới có bị trùng lịch không.

### Bài 3.5 ⭐⭐ – Prefix Sum: Doanh thu theo khoảng ngày
Có mảng doanh thu 365 ngày. Trả lời 10⁵ truy vấn "tổng doanh thu từ ngày L đến R" mỗi truy vấn O(1).

---

## 2. HashMap / HashSet

### Bài 3.6 ⭐ – Phát hiện email trùng
Import 1 triệu user từ CSV, tìm các email bị trùng (không phân biệt hoa thường).

### Bài 3.7 ⭐⭐ – Group Anagrams → Gom nhóm tag
Gom các tag có cùng tập ký tự: `["eat","tea","tan","ate","nat","bat"]` → `[["eat","tea","ate"],["tan","nat"],["bat"]]`.

### Bài 3.8 ⭐⭐ – Top K API được gọi nhiều nhất
Từ log, trả về K endpoint được gọi nhiều nhất. (HashMap đếm + sort; sau Phần 4 làm lại bằng Heap.)

### Bài 3.9 ⭐⭐ – Tự cài đặt HashMap
Implement `MyHashMap<K,V>` với `put`, `get`, `remove`, xử lý collision bằng **chaining** (mảng các linked list), tự **resize** khi load factor > 0.75.

---

## 3. Stack

### Bài 3.10 ⭐ – Kiểm tra JSON/ngoặc hợp lệ
`"{ \"a\": [1, 2, {\"b\": 3}] }"` → hợp lệ. `"{[}]"` → không hợp lệ.

### Bài 3.11 ⭐⭐ – Undo/Redo cho trình soạn thảo cấu hình
Dùng 2 stack cài đặt `execute(cmd)`, `undo()`, `redo()`.

### Bài 3.12 ⭐⭐ – Chuẩn hóa đường dẫn URL
`"/api/v1/../v2/./users//42/"` → `"/api/v2/users/42"`.

---

## 4. Queue

### Bài 3.13 ⭐ – Hàng đợi xử lý job
Implement `JobQueue` với `enqueue(job)`, `dequeue()`, `size()` bằng **mảng vòng (circular buffer)** kích thước cố định. Đầy thì từ chối job.

### Bài 3.14 ⭐⭐ – Rate Limiter (Sliding Log)
Mỗi user tối đa **5 request / 10 giây**. `allow(userId, timestamp) -> bool`. Dùng `Map<userId, Queue<timestamp>>`.

### Bài 3.15 ⭐⭐ – Stack bằng 2 Queue và Queue bằng 2 Stack
Cài cả 2, phân tích amortized O(1).

---

## 5. Linked List

### Bài 3.16 ⭐ – Tự cài Singly & Doubly Linked List
`addFirst`, `addLast`, `remove(value)`, `reverse()`, `find()`.

### Bài 3.17 ⭐⭐ – Phát hiện vòng lặp
Danh sách redirect URL dạng linked list: `A → B → C → A`. Phát hiện vòng lặp redirect (Floyd's cycle detection – O(1) bộ nhớ).

### Bài 3.18 ⭐⭐ – Merge 2 danh sách đã sắp xếp
Gộp 2 danh sách event (đã sort theo timestamp) từ 2 server thành 1 danh sách sort.
- **Mở rộng:** Merge K danh sách (làm ở Phần 4 với Heap).

---

## 6. Sorting & Binary Search

### Bài 3.19 ⭐⭐ – Tự cài các thuật toán sort
Bubble, Insertion, **Merge Sort**, **Quick Sort**. So sánh thời gian với n = 10⁵. Thuật toán nào **stable**? Vì sao điều đó quan trọng khi sort đơn hàng theo nhiều tiêu chí?

### Bài 3.20 ⭐⭐ – Sort đa tiêu chí
Sort danh sách sản phẩm: giá tăng dần, cùng giá thì rating giảm dần, cùng rating thì tên A→Z. (Viết Comparator.)

### Bài 3.21 ⭐⭐ – Binary Search trên log theo thời gian
Log đã sort theo timestamp (10⁷ dòng). Tìm tất cả log trong khoảng `[t1, t2]` với O(log n + k). Cài `lowerBound`, `upperBound`.

### Bài 3.22 ⭐⭐ – Phân trang (Pagination)
- Offset pagination: `getPage(list, page, size)`.
- Cursor pagination: `getAfter(sortedList, cursorId, size)` dùng binary search.
- **Câu hỏi:** Vì sao cursor pagination tốt hơn với dữ liệu lớn trong DB?

---

## Checklist sau Phần 3
- [ ] Tự cài được HashMap, LinkedList, Stack, Queue
- [ ] Viết Merge Sort, Quick Sort, Binary Search không cần tra
- [ ] Phân tích Big-O cho mọi lời giải
