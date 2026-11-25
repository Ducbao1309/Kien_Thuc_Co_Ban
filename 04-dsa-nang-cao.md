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

# ➕ Bài tập bổ sung (mỗi bài kèm 1 bài LeetCode tương ứng)

## 8. Tree (tiếp)

### Bài 4.30 ⭐⭐ – Độ sâu cây danh mục / comment
Tính độ sâu lớn nhất của cây (DFS đệ quy và BFS). Áp dụng: giới hạn độ sâu reply comment.

🔗 [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) – Easy

### Bài 4.31 ⭐⭐⭐ – Duyệt sơ đồ tổ chức theo cấp
In nhân viên theo từng cấp quản lý (BFS), và chỉ lấy người “ngoài cùng bên phải” mỗi cấp.

🔗 [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) – Medium · [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/) – Medium

### Bài 4.32 ⭐⭐⭐ – Đường đi có tổng chi phí bằng target
Cây chi phí dịch vụ: tìm tất cả đường từ gốc tới lá có tổng chi phí đúng bằng target (backtracking trên cây).

🔗 [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/) – Medium

### Bài 4.33 ⭐⭐⭐ – Phần tử nhỏ thứ K trong BST
Index giá sản phẩm dạng BST: tìm giá nhỏ thứ K (inorder). **Mở rộng:** nếu BST bị sửa liên tục thì tối ưu thế nào?

🔗 [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) – Medium

### Bài 4.34 ⭐⭐⭐⭐ – Khôi phục cây từ 2 cách duyệt
Có preorder và inorder của cây cấu hình đã lưu. Dựng lại cây trong O(n) (HashMap tra vị trí).

🔗 [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) – Medium

## 9. Graph (tiếp)

### Bài 4.35 ⭐⭐⭐ – Đếm cụm server trong data center
Lưới `1` (server hoạt động) / `0` (trống). Đếm số cụm server kết nối (DFS/BFS/Union-Find).

🔗 [200. Number of Islands](https://leetcode.com/problems/number-of-islands/) – Medium

### Bài 4.36 ⭐⭐⭐ – Lan truyền sự cố
Lưới service: `2` = đã lỗi, `1` = khỏe, `0` = không có. Mỗi phút lỗi lan sang hàng xóm. Sau bao lâu toàn bộ lỗi? (Multi-source BFS.)

🔗 [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) – Medium

### Bài 4.37 ⭐⭐⭐ – Sao chép đồ thị phụ thuộc
Deep clone đồ thị microservice (mỗi node có danh sách neighbors) – dùng HashMap `old → new`.

🔗 [133. Clone Graph](https://leetcode.com/problems/clone-graph/) – Medium

### Bài 4.38 ⭐⭐⭐⭐ – Thời gian lan truyền broadcast
Gửi message từ node K, mỗi cạnh có độ trễ. Bao lâu để mọi node nhận được? (Dijkstra.)

🔗 [743. Network Delay Time](https://leetcode.com/problems/network-delay-time/) – Medium

### Bài 4.39 ⭐⭐⭐⭐ – Chặng bay rẻ nhất với tối đa K điểm dừng
Đặt vé/định tuyến với giới hạn số hop (Bellman-Ford K vòng hoặc BFS theo tầng).

🔗 [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/) – Medium

### Bài 4.40 ⭐⭐⭐⭐ – Đi dây mạng tối thiểu (MST)
N server ở các tọa độ, chi phí nối = khoảng cách Manhattan. Nối tất cả với chi phí nhỏ nhất (Kruskal + Union-Find hoặc Prim).

🔗 [1584. Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/) – Medium

### Bài 4.41 ⭐⭐⭐ – Phát hiện kết nối thừa
Mạng ban đầu là cây, sau đó bị thêm 1 cạnh tạo vòng. Tìm cạnh thừa (Union-Find).

🔗 [684. Redundant Connection](https://leetcode.com/problems/redundant-connection/) – Medium

## 10. Backtracking

### Bài 4.42 ⭐⭐⭐ – Sinh tổ hợp feature flags
Với N feature flag, sinh tất cả tổ hợp bật/tắt để chạy test (subsets). **Câu hỏi:** độ phức tạp là bao nhiêu và vì sao không thể test hết khi N lớn?

🔗 [78. Subsets](https://leetcode.com/problems/subsets/) – Medium

### Bài 4.43 ⭐⭐⭐ – Ghép gói dịch vụ đúng ngân sách
Các gói dịch vụ (dùng lặp lại được) có giá; liệt kê mọi cách chọn có tổng đúng bằng ngân sách.

🔗 [39. Combination Sum](https://leetcode.com/problems/combination-sum/) – Medium

## 11. Dynamic Programming (tiếp)

### Bài 4.44 ⭐⭐⭐ – Tách từ khóa tìm kiếm
`"iphonecase"` có tách được thành các từ trong từ điển không (`"iphone case"`)? Áp dụng cho search tiếng Việt viết liền.

🔗 [139. Word Break](https://leetcode.com/problems/word-break/) – Medium

### Bài 4.45 ⭐⭐⭐ – Diff 2 phiên bản file (LCS)
Tìm dãy con chung dài nhất giữa 2 danh sách dòng → nền tảng của `git diff`. In ra các dòng thêm/xóa.

🔗 [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) – Medium

### Bài 4.46 ⭐⭐⭐ – Chuỗi phiên bản tăng dài nhất
Dãy số version/điểm số theo thời gian; tìm dãy con tăng dài nhất – O(n²) rồi O(n log n) (patience sorting).

🔗 [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) – Medium

### Bài 4.47 ⭐⭐⭐ – Chia tải cân bằng cho 2 server
Danh sách job có trọng số; có chia được thành 2 nhóm tổng bằng nhau không? (0/1 Knapsack dạng boolean.)

🔗 [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) – Medium

## 12. Greedy / Sliding Window / Cấu trúc đặc biệt

### Bài 4.48 ⭐⭐⭐ – Lập lịch task có cooldown
CPU chạy các loại task `A, B, C...`; cùng loại phải cách nhau n đơn vị thời gian. Tổng thời gian tối thiểu?

🔗 [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/) – Medium

### Bài 4.49 ⭐⭐⭐ – Hủy ít booking nhất để không trùng lịch
Bỏ ít khoảng nhất để các khoảng còn lại không chồng nhau (greedy theo end time).

🔗 [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) – Medium

### Bài 4.50 ⭐⭐⭐⭐ – Peak CPU trong mỗi cửa sổ K giây
Với mỗi cửa sổ trượt K giây, in ra CPU lớn nhất trong O(n) (Monotonic Deque).

🔗 [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) – Hard

### Bài 4.51 ⭐⭐⭐⭐ – Đoạn log ngắn nhất chứa đủ các loại lỗi
Tìm đoạn log liên tiếp ngắn nhất chứa đủ tất cả mã lỗi cần điều tra.

🔗 [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) – Hard

### Bài 4.52 ⭐⭐⭐⭐ – Doanh thu realtime có cập nhật (Fenwick / Segment Tree)
Như Bài 3.5 nhưng doanh thu ngày có thể bị **sửa** (hoàn tiền). `update(i, val)` và `sumRange(l, r)` đều O(log n).

🔗 [307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/) – Medium

### Bài 4.53 ⭐⭐⭐ – Tìm K tài xế gần nhất
Tọa độ tài xế và khách hàng; trả về K tài xế gần nhất (max-heap kích thước K hoặc Quickselect).

🔗 [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) – Medium

---

# 🧩 Đối chiếu bài gốc ↔ LeetCode

| Bài trong file | Bài LeetCode tương ứng | Độ khó |
|----------------|------------------------|--------|
| 4.1 Cây danh mục | [429. N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/) | Medium |
| 4.2 BST | [700](https://leetcode.com/problems/search-in-a-binary-search-tree/), [701](https://leetcode.com/problems/insert-into-a-binary-search-tree/), [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/), [98. Validate BST](https://leetcode.com/problems/validate-binary-search-tree/) | Easy–Medium |
| 4.3 LCA | [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) | Medium |
| 4.4 Serialize cây | [297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) | Hard |
| 4.6 Top K | [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/), [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/) | Medium |
| 4.7 Task Scheduler | [1834. Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/) | Medium |
| 4.8 Merge K logs | [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | Hard |
| 4.9 Median streaming | [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | Hard |
| 4.10 Delayed Job Queue | [2402. Meeting Rooms III](https://leetcode.com/problems/meeting-rooms-iii/) | Hard |
| 4.11 Autocomplete | [208. Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/), [1268. Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/) | Medium |
| 4.12 URL Router | [211. Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/) | Medium |
| 4.13 BFS/DFS mạng xã hội | [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/), [1971. Find if Path Exists in Graph](https://leetcode.com/problems/find-if-path-exists-in-graph/) | Easy–Medium |
| 4.14 Topological Sort | [207. Course Schedule](https://leetcode.com/problems/course-schedule/), [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) | Medium |
| 4.15 Dijkstra | [743. Network Delay Time](https://leetcode.com/problems/network-delay-time/), [1631. Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/) | Medium |
| 4.16 Gộp tài khoản | [721. Accounts Merge](https://leetcode.com/problems/accounts-merge/) | Medium |
| 4.17 Deadlock | [802. Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/) | Medium |
| 4.18 Sliding Window Counter | [362. Design Hit Counter](https://leetcode.com/problems/design-hit-counter/) 🔒 | Medium |
| 4.19 Spike traffic | [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/), [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/) | Easy–Medium |
| 4.20 Chuỗi không lặp | [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Medium |
| 4.21 Coin Change | [322. Coin Change](https://leetcode.com/problems/coin-change/), [518. Coin Change II](https://leetcode.com/problems/coin-change-ii/) | Medium |
| 4.22 Knapsack | [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/), [494. Target Sum](https://leetcode.com/problems/target-sum/) | Medium |
| 4.23 Edit Distance | [72. Edit Distance](https://leetcode.com/problems/edit-distance/) | Medium |
| 4.24 Weighted Interval | [1235. Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/) | Hard |
| 4.25 Số phòng họp | [253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) 🔒, bản free: [2406. Divide Intervals Into Minimum Number of Groups](https://leetcode.com/problems/divide-intervals-into-minimum-number-of-groups/) | Medium |
| 4.26 LRU Cache | [146. LRU Cache](https://leetcode.com/problems/lru-cache/) | Medium |
| 4.27 LFU Cache | [460. LFU Cache](https://leetcode.com/problems/lfu-cache/) | Hard |

**Luyện thêm (tùy chọn):** [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) · [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) · [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) · [127. Word Ladder](https://leetcode.com/problems/word-ladder/) · [212. Word Search II](https://leetcode.com/problems/word-search-ii/) · [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) · [198. House Robber](https://leetcode.com/problems/house-robber/) · [55. Jump Game](https://leetcode.com/problems/jump-game/) · [134. Gas Station](https://leetcode.com/problems/gas-station/) · [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

> 🔒 = bài Premium. 4.28 (Bloom Filter) và 4.29 (Consistent Hashing) không có trên LeetCode – đây là kiến thức System Design.

---

## Checklist sau Phần 4
- [ ] DFS/BFS trên cây và đồ thị viết trơn tru
- [ ] Dùng Heap cho các bài Top-K / Scheduling
- [ ] Tự viết LRU Cache trong 20 phút
- [ ] Nhận diện được bài nào dùng Sliding Window / DP / Greedy
