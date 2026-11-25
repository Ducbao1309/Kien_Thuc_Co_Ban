# Lộ trình bài tập OOP & DSA định hướng Backend

Bộ bài tập này dành cho bạn đã từng học OOP/DSA nhưng bị "rơi rụng" kiến thức. Mọi bài đều đặt trong **bối cảnh Backend thực tế** (user, order, cache, queue, API, log...) để bạn vừa ôn nền tảng vừa thấy nó dùng vào đâu.

## Cấu trúc

| File | Nội dung | Số bài | Cấp độ |
|------|----------|--------|--------|
| [01-oop-co-ban.md](01-oop-co-ban.md) | Class, Object, 4 tính chất OOP, Generics, Value Object | 27 bài + 22 LeetCode | ⭐ – ⭐⭐ |
| [02-oop-nang-cao.md](02-oop-nang-cao.md) | SOLID, Design Patterns, Layered / Hexagonal / CQRS | 36 bài + 26 LeetCode | ⭐⭐⭐ – ⭐⭐⭐⭐ |
| [03-dsa-co-ban.md](03-dsa-co-ban.md) | Array, String, HashMap, Stack, Queue, Linked List, Sort, Binary Search | 46 bài (kèm LeetCode) | ⭐ – ⭐⭐ |
| [04-dsa-nang-cao.md](04-dsa-nang-cao.md) | Tree, Heap, Graph, Trie, Backtracking, Sliding Window, DP | 53 bài (kèm LeetCode) | ⭐⭐⭐ – ⭐⭐⭐⭐ |
| [05-du-an-tong-hop.md](05-du-an-tong-hop.md) | Mini-project / Low-Level Design kết hợp OOP + DSA | 20 dự án | ⭐⭐⭐ – ⭐⭐⭐⭐⭐ |

## Về phần LeetCode
- Mỗi file có mục **🧩 LeetCode**: bài mới có link ngay dưới đề, bài cũ có bảng đối chiếu sang LeetCode.
- Nên làm **bài bối cảnh Backend trước**, sau đó nộp bài LeetCode tương ứng để kiểm tra edge case & hiệu năng.
- Bài có ký hiệu 🔒 là Premium – có thể bỏ qua hoặc tìm bản free tương đương được ghi kèm.

## Ngôn ngữ
Đề bài viết **không phụ thuộc ngôn ngữ**. Khuyến nghị:
- **Java / C#**: tốt nhất để luyện OOP (interface, abstract class, access modifier rõ ràng).
- **Python / TypeScript**: viết nhanh, phù hợp luyện DSA.

## Lịch học gợi ý (6 tuần, ~1–2h/ngày)

| Tuần | Nội dung |
|------|----------|
| 1 | File 01 (OOP cơ bản) + File 03 phần 1–3 |
| 2 | File 03 phần 4–6 |
| 3 | File 02 phần SOLID + File 04 phần Tree/Heap |
| 4 | File 02 phần Design Patterns + File 04 phần Graph/Trie |
| 5 | File 04 phần Sliding Window/DP |
| 6 | File 05 – chọn 1–2 dự án tổng hợp |

## Cách làm mỗi bài (quan trọng)
1. Đọc đề, **tự viết ra test case** trước khi code.
2. Code phiên bản đơn giản nhất chạy được.
3. Phân tích **độ phức tạp thời gian/bộ nhớ** (Big-O).
4. Đọc phần *Gợi ý* chỉ khi bí > 20 phút.
5. Làm phần *Mở rộng* để lên level.
6. Viết unit test (JUnit / pytest / Jest).

> Mẹo: Tạo 1 repo Git, mỗi bài 1 commit. Đây cũng là portfolio khi đi phỏng vấn.
