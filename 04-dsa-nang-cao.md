# Phần 4 – DSA Nâng cao (bối cảnh Backend)

## 1. Tree

### Bài 4.1 ⭐⭐⭐ – Cây danh mục sản phẩm
Dữ liệu từ DB dạng phẳng: `[{id:1, parentId:null, name:"Điện tử"}, {id:2, parentId:1, name:"Điện thoại"}, ...]`.
- Xây dựng cây (O(n) bằng HashMap).
- In cây dạng thụt lề (DFS), in theo từng cấp (BFS).
- `getBreadcrumb(id)` → `"Điện tử > Điện thoại > iPhone"`.
- Tìm tất cả danh mục con (mọi cấp) của một danh mục.

### Bài 4.2 ⭐⭐⭐ – Binary Search Tree
Cài BST: `insert`, `search`, `delete` (3 trường hợp), duyệt inorder/preorder/postorder, `height`, `isValidBST`.
- **Câu hỏi:** Vì sao DB index dùng **B+Tree** mà không dùng BST?

### Bài 4.3 ⭐⭐⭐ – Lowest Common Ancestor
Trong sơ đồ tổ chức công ty (cây), tìm người quản lý chung gần nhất của 2 nhân viên.

### Bài 4.4 ⭐⭐⭐ – Serialize / Deserialize cây
Chuyển cây comment lồng nhau thành chuỗi JSON để lưu cache và khôi phục lại.

---

## 2. Heap / Priority Queue

### Bài 4.5 ⭐⭐⭐ – Tự cài Min-Heap
`push`, `pop`, `peek`, `heapify(array)` O(n).

### Bài 4.6 ⭐⭐⭐ – Top K endpoints (làm lại Bài 3.8)
Dùng min-heap kích thước K → O(n log K). So sánh với cách sort.

### Bài 4.7 ⭐⭐⭐ – Task Scheduler theo độ ưu tiên
Job có `priority` và `createdAt`. Luôn xử lý job ưu tiên cao nhất; cùng ưu tiên thì job cũ trước.

### Bài 4.8 ⭐⭐⭐⭐ – Merge K sorted logs
K server, mỗi server có file log đã sort theo thời gian. Gộp thành 1 luồng sort, O(N log K).

### Bài 4.9 ⭐⭐⭐⭐ – Median của response time (streaming)
Mỗi request trả về latency, sau mỗi lần thêm in ra **median** hiện tại. (2 heap.)

### Bài 4.10 ⭐⭐⭐⭐ – Delayed Job Queue
`schedule(job, runAt)`, `poll(now)` trả về các job đã đến hạn. (Giống cơ chế của Sidekiq/BullMQ delayed jobs.)

---

## 3. Trie

### Bài 4.11 ⭐⭐⭐ – Autocomplete tìm kiếm
Cài Trie: `insert(word, frequency)`, `suggest(prefix, k)` trả về k gợi ý phổ biến nhất.

### Bài 4.12 ⭐⭐⭐⭐ – URL Router
Xây router hỗ trợ path param:
```
router.add("GET", "/users/:id", handlerA)
router.add("GET", "/users/:id/orders/:orderId", handlerB)
router.match("GET", "/users/42/orders/7") -> handlerB, params {id:42, orderId:7}
```
(Đây là cách Express / Gin / FastAPI định tuyến.)

---

## 4. Graph

### Bài 4.13 ⭐⭐⭐ – Biểu diễn đồ thị & BFS/DFS
Mạng xã hội: user là đỉnh, follow là cạnh. Cài adjacency list. Tìm "bạn của bạn" (khoảng cách 2), gợi ý kết bạn theo số bạn chung.

### Bài 4.14 ⭐⭐⭐ – Topological Sort: Thứ tự chạy migration / build
Các service có phụ thuộc: `auth` cần `db`, `order` cần `auth` và `product`... Tìm thứ tự khởi động. Phát hiện **phụ thuộc vòng** (Kahn's algorithm).

### Bài 4.15 ⭐⭐⭐⭐ – Dijkstra: Định tuyến giao hàng
Bản đồ kho hàng (đỉnh) và đường đi có trọng số (km). Tìm đường ngắn nhất từ kho đến khách hàng, in ra lộ trình.

### Bài 4.16 ⭐⭐⭐⭐ – Union-Find: Gộp tài khoản
Mỗi tài khoản có nhiều email; 2 tài khoản chung 1 email là cùng 1 người. Gộp lại. (Disjoint Set với path compression + union by rank.)

### Bài 4.17 ⭐⭐⭐⭐ – Phát hiện deadlock
Cho danh sách "transaction T1 đang chờ lock giữ bởi T2". Phát hiện chu trình trong wait-for graph.

---

## 5. Sliding Window & Two Pointers

### Bài 4.18 ⭐⭐⭐ – Rate Limiter: Sliding Window Counter
Cải tiến Bài 3.14 để tiết kiệm bộ nhớ: chỉ lưu count của window hiện tại và trước đó, ước lượng theo tỷ lệ.

### Bài 4.19 ⭐⭐⭐ – Phát hiện spike traffic
Mảng số request mỗi giây. Tìm cửa sổ K giây có tổng request lớn nhất. Tìm đoạn ngắn nhất có tổng ≥ S.

### Bài 4.20 ⭐⭐⭐ – Chuỗi con dài nhất không lặp ký tự
Áp dụng: tìm phiên dài nhất user truy cập toàn trang khác nhau.

---

## 6. Dynamic Programming & Greedy

### Bài 4.21 ⭐⭐⭐ – Đổi tiền (Coin Change)
Máy ATM có mệnh giá `[10k, 20k, 50k, 100k, 200k, 500k]`. Số tờ ít nhất để rút X? Số cách rút? Khi nào greedy sai?

### Bài 4.22 ⭐⭐⭐⭐ – Knapsack: Chọn job trong ngân sách server
Mỗi job có `cpuCost` và `value`. Server có `C` CPU. Chọn tập job tối đa value (0/1 Knapsack).

### Bài 4.23 ⭐⭐⭐⭐ – Edit Distance: "Có phải bạn muốn tìm..."
Tính khoảng cách Levenshtein để gợi ý sửa lỗi chính tả từ khóa tìm kiếm.

### Bài 4.24 ⭐⭐⭐⭐ – Weighted Interval Scheduling
Các booking có `[start, end, price]`, chọn tập không chồng nhau có tổng tiền lớn nhất (DP + binary search).

### Bài 4.25 ⭐⭐⭐ – Greedy: Lập lịch phòng họp
Số phòng họp tối thiểu để chứa tất cả cuộc họp (sort + min-heap).

---

## 7. Cấu trúc dữ liệu đặc biệt cho Backend

### Bài 4.26 ⭐⭐⭐⭐ – LRU Cache
`get(key)`, `put(key, value)` đều O(1), capacity cố định. (HashMap + Doubly Linked List.)
- **Mở rộng:** Thêm TTL cho từng key. Làm thread-safe.

### Bài 4.27 ⭐⭐⭐⭐⭐ – LFU Cache
Loại bỏ key ít được dùng nhất, hòa thì loại key cũ nhất. Mọi thao tác O(1).

### Bài 4.28 ⭐⭐⭐⭐ – Bloom Filter
Kiểm tra nhanh "username đã tồn tại chưa" trước khi query DB. Cài với k hàm hash, đo tỷ lệ false positive.

### Bài 4.29 ⭐⭐⭐⭐⭐ – Consistent Hashing
Phân phối key cho N cache server. Khi thêm/bớt server chỉ di chuyển ~1/N key. Có virtual nodes. (Dùng TreeMap / sorted array + binary search.)

---

## Checklist sau Phần 4
- [ ] DFS/BFS trên cây và đồ thị viết trơn tru
- [ ] Dùng Heap cho các bài Top-K / Scheduling
- [ ] Tự viết LRU Cache trong 20 phút
- [ ] Nhận diện được bài nào dùng Sliding Window / DP / Greedy
