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

# ➕ Bài tập bổ sung (mỗi bài kèm 1 bài LeetCode tương ứng)

## 7. Array & Two Pointers

### Bài 3.23 ⭐ – Thời điểm mua/bán tốt nhất
Mảng giá cổ phiếu/coin theo ngày. Tìm lợi nhuận lớn nhất với 1 lần mua và 1 lần bán (O(n), 1 lần duyệt).

🔗 [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) – Easy

### Bài 3.24 ⭐ – Loại bỏ ID trùng trong danh sách đã sort
Danh sách `userId` đã sort từ nhiều nguồn, loại trùng **tại chỗ** (in-place, O(1) bộ nhớ), trả về độ dài mới.

🔗 [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) – Easy

### Bài 3.25 ⭐ – Gộp 2 danh sách đơn hàng đã sort
2 mảng đơn hàng đã sort theo thời gian, gộp vào mảng thứ nhất (đã có chỗ trống ở cuối) – duyệt từ cuối lên.

🔗 [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) – Easy

### Bài 3.26 ⭐⭐ – Tỷ lệ chuyển đổi trừ chính nó
Mảng số liệu từng bước funnel; với mỗi phần tử, tính tích của tất cả phần tử còn lại **không dùng phép chia**, O(n).

🔗 [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) – Medium

### Bài 3.27 ⭐⭐ – Round-robin xoay server
Danh sách server cần xoay vòng k vị trí (in-place, O(1) bộ nhớ – dùng 3 lần reverse).

🔗 [189. Rotate Array](https://leetcode.com/problems/rotate-array/) – Medium

### Bài 3.28 ⭐⭐ – Ba giao dịch có tổng bằng 0 (đối soát kế toán)
Tìm tất cả bộ 3 giao dịch (+/-) có tổng bằng 0, không trùng lặp. Sort + two pointers O(n²).

🔗 [15. 3Sum](https://leetcode.com/problems/3sum/) – Medium

## 8. String

### Bài 3.29 ⭐ – Prefix chung của các route
`["/api/v1/users", "/api/v1/orders", "/api/v1/products"]` → `"/api/v1/"`. Dùng để gom nhóm route khi viết document API.

🔗 [14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/) – Easy

### Bài 3.30 ⭐ – Kiểm tra 2 chuỗi là hoán vị (anagram)
So sánh 2 mã voucher có cùng tập ký tự hay không – O(n) bằng mảng đếm 26 ký tự.

🔗 [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/) – Easy

### Bài 3.31 ⭐ – Ký tự đầu tiên không lặp
Tìm sự kiện (ký tự) đầu tiên chỉ xuất hiện 1 lần trong luồng log.

🔗 [387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/) – Easy

### Bài 3.32 ⭐⭐ – Khớp mẫu route
Pattern `"abba"` và chuỗi `"user order order user"` có khớp song ánh không? (2 HashMap.)

🔗 [290. Word Pattern](https://leetcode.com/problems/word-pattern/) – Easy

## 9. HashMap / Prefix Sum

### Bài 3.33 ⭐⭐ – Số đoạn ngày có doanh thu đúng bằng K
Đếm số đoạn con liên tiếp có tổng bằng K (có số âm – hoàn tiền). Prefix sum + HashMap, O(n).

🔗 [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) – Medium

### Bài 3.34 ⭐⭐ – Chuỗi ngày đăng nhập liên tiếp dài nhất
Cho tập các ngày (dạng số nguyên, không sort) user đăng nhập, tìm streak dài nhất trong O(n).

🔗 [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) – Medium

### Bài 3.35 ⭐ – Node trả lời chiếm đa số
Trong log của cluster, tìm node xuất hiện > n/2 lần với O(1) bộ nhớ (Boyer–Moore Voting).

🔗 [169. Majority Element](https://leetcode.com/problems/majority-element/) – Easy

## 10. Stack

### Bài 3.36 ⭐⭐ – Rule engine với ký pháp Ba Lan ngược
Đánh giá biểu thức tính phí dạng RPN: `["2","1","+","3","*"]` → 9.

🔗 [150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/) – Medium

### Bài 3.37 ⭐⭐ – Số ngày chờ giá tăng (Monotonic Stack)
Mảng giá/tải hệ thống theo ngày; với mỗi ngày, bao nhiêu ngày sau thì giá trị cao hơn?

🔗 [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) – Medium

### Bài 3.38 ⭐⭐ – Giải nén template
`"3[ab]2[c]"` → `"abababcc"` – dùng stack, hỗ trợ lồng nhau `"2[a3[b]]"`.

🔗 [394. Decode String](https://leetcode.com/problems/decode-string/) – Medium

## 11. Linked List

### Bài 3.39 ⭐ – Đảo ngược danh sách event
Đảo linked list cả 2 cách: vòng lặp và đệ quy.

🔗 [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) – Easy

### Bài 3.40 ⭐⭐ – Xóa phần tử thứ N từ cuối
Xóa request thứ N tính từ cuối trong 1 lần duyệt (2 con trỏ cách nhau N).

🔗 [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) – Medium

### Bài 3.41 ⭐⭐ – Cộng 2 số rất lớn
Số dư tài khoản lớn hơn kiểu `long`, lưu từng chữ số trong linked list (ngược). Cộng 2 số.

🔗 [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) – Medium

## 12. Binary Search

### Bài 3.42 ⭐ – Tìm bản deploy lỗi đầu tiên
Có 1000 phiên bản deploy, từ một bản nào đó trở đi đều lỗi. Tìm bản lỗi đầu tiên với ít lần gọi `isBad(v)` nhất (đây chính là `git bisect`).

🔗 [278. First Bad Version](https://leetcode.com/problems/first-bad-version/) – Easy

### Bài 3.43 ⭐⭐ – Tìm kiếm trong log bị xoay vòng
Log ring buffer đã sort nhưng bị xoay (`[40,50,60,10,20,30]`). Tìm timestamp trong O(log n).

🔗 [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) – Medium

### Bài 3.44 ⭐⭐ – Binary search trên đáp án: Công suất worker tối thiểu
Danh sách job (kích thước) phải xử lý **theo thứ tự** trong D ngày. Tìm công suất/ngày nhỏ nhất của worker.

🔗 [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) – Medium

## 13. Sorting nâng cao

### Bài 3.45 ⭐⭐ – Phân loại ticket theo mức ưu tiên
Mảng ticket có priority `0 (LOW), 1 (MEDIUM), 2 (HIGH)`, sắp xếp 1 lần duyệt, in-place (Dutch National Flag).

🔗 [75. Sort Colors](https://leetcode.com/problems/sort-colors/) – Medium

### Bài 3.46 ⭐⭐ – Phần tử lớn thứ K (Quickselect)
Tìm request có latency lớn thứ K (VD: P99) – trung bình O(n) bằng Quickselect, so sánh với sort O(n log n).

🔗 [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) – Medium

---

# 🧩 Đối chiếu bài gốc ↔ LeetCode

| Bài trong file | Bài LeetCode tương ứng | Độ khó |
|----------------|------------------------|--------|
| 3.2 Two Sum | [1. Two Sum](https://leetcode.com/problems/two-sum/) | Easy |
| 3.3 Slug generator | [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) (xử lý ký tự) | Easy |
| 3.4 Merge Intervals | [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/), [57. Insert Interval](https://leetcode.com/problems/insert-interval/) | Medium |
| 3.5 Prefix Sum | [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) | Easy |
| 3.6 Email trùng | [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) | Easy |
| 3.7 Group Anagrams | [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/) | Medium |
| 3.8 Top K API | [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | Medium |
| 3.9 Tự cài HashMap | [706. Design HashMap](https://leetcode.com/problems/design-hashmap/) | Easy |
| 3.10 Ngoặc hợp lệ | [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) | Easy |
| 3.11 Undo/Redo | [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/) | Easy |
| 3.12 Chuẩn hóa URL | [71. Simplify Path](https://leetcode.com/problems/simplify-path/) | Medium |
| 3.13 Circular buffer | [622. Design Circular Queue](https://leetcode.com/problems/design-circular-queue/) | Medium |
| 3.14 Rate Limiter | [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/), [359. Logger Rate Limiter](https://leetcode.com/problems/logger-rate-limiter/) 🔒 | Easy |
| 3.15 Stack ↔ Queue | [232](https://leetcode.com/problems/implement-queue-using-stacks/), [225](https://leetcode.com/problems/implement-stack-using-queues/) | Easy |
| 3.16 Linked List | [707. Design Linked List](https://leetcode.com/problems/design-linked-list/) | Medium |
| 3.17 Phát hiện vòng lặp | [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/), [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) | Easy/Medium |
| 3.18 Merge 2 list | [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) | Easy |
| 3.19 Tự cài sort | [912. Sort an Array](https://leetcode.com/problems/sort-an-array/) | Medium |
| 3.21 lowerBound/upperBound | [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | Medium |
| 3.22 Cursor pagination | [704. Binary Search](https://leetcode.com/problems/binary-search/) | Easy |

**Luyện thêm (tùy chọn):** [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/) · [167. Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) · [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/) · [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) · [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/) · [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

> 🔒 = bài Premium.

---

## Checklist sau Phần 3
- [ ] Tự cài được HashMap, LinkedList, Stack, Queue
- [ ] Viết Merge Sort, Quick Sort, Binary Search không cần tra
- [ ] Phân tích Big-O cho mọi lời giải
