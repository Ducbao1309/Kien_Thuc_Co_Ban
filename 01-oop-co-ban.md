# Phần 1 – OOP Cơ bản (bối cảnh Backend)

## Ôn nhanh lý thuyết
- **Class / Object**: Class là bản thiết kế, Object là thực thể.
- **Encapsulation (Đóng gói)**: ẩn dữ liệu (private), chỉ lộ ra hành vi hợp lệ.
- **Inheritance (Kế thừa)**: lớp con tái sử dụng lớp cha ("is-a").
- **Polymorphism (Đa hình)**: cùng một lời gọi, nhiều cách thực thi.
- **Abstraction (Trừu tượng)**: chỉ định nghĩa "làm gì", không quan tâm "làm thế nào" (interface/abstract class).

---

## Bài 1.1 ⭐ – Class `User`
**Bối cảnh:** Mọi hệ thống backend đều có User.

**Yêu cầu:**
- Thuộc tính: `id`, `username`, `email`, `passwordHash`, `createdAt`, `isActive`.
- Constructor nhận `username`, `email`, `rawPassword`; tự sinh `id` (UUID) và `createdAt`.
- Method `checkPassword(raw) -> bool`, `deactivate()`, `toString()` (**không in password**).

**Test case:**
```
u = User("alice", "alice@mail.com", "123456")
u.checkPassword("123456")  -> true
u.checkPassword("wrong")   -> false
print(u)                   -> User(id=..., username=alice, email=alice@mail.com, active=true)
```
**Mở rộng:** Hash password bằng SHA-256 + salt thay vì lưu thô.

---

## Bài 1.2 ⭐ – Encapsulation với `BankAccount`
**Yêu cầu:**
- `balance` là **private**, không có setter.
- `deposit(amount)`, `withdraw(amount)`: ném exception nếu `amount <= 0` hoặc số dư không đủ.
- Lưu lịch sử giao dịch `List<Transaction>` (type, amount, timestamp). `getHistory()` trả về **bản copy / read-only**.

**Câu hỏi tự kiểm tra:** Vì sao không nên trả về trực tiếp list nội bộ?

**Mở rộng:** Làm cho `withdraw` an toàn khi nhiều thread gọi đồng thời (lock / synchronized).

---

## Bài 1.3 ⭐ – Validation trong Constructor
Tạo class `Email` (Value Object):
- Constructor nhận chuỗi, validate định dạng, sai thì ném `InvalidEmailException`.
- **Immutable** (không thể sửa sau khi tạo).
- Override `equals()` và `hashCode()` – hai `Email("a@b.com")` phải bằng nhau.

**Câu hỏi:** Tại sao khi override `equals` bắt buộc phải override `hashCode`? (Gợi ý: HashMap/HashSet.)

---

## Bài 1.4 ⭐⭐ – Inheritance: Hệ thống nhân viên
```
Employee (abstract): id, name, baseSalary, abstract calculateSalary()
 ├── FullTimeEmployee : salary = baseSalary + bonus
 ├── PartTimeEmployee : salary = hourlyRate * hoursWorked
 └── Intern           : salary = baseSalary * 0.5
```
- Viết hàm `totalPayroll(List<Employee>)` tính tổng lương **mà không dùng if/instanceof**.

**Câu hỏi:** Đây là ví dụ của tính chất nào? Abstract class khác Interface ở điểm nào?

---

## Bài 1.5 ⭐⭐ – Polymorphism: `NotificationService`
**Bối cảnh:** Backend cần gửi thông báo qua nhiều kênh.

```
interface Notifier { send(to, message) }
EmailNotifier, SmsNotifier, PushNotifier implements Notifier
```
- `NotificationService` nhận `List<Notifier>` và gửi qua tất cả kênh.
- Thêm kênh `TelegramNotifier` **mà không sửa** `NotificationService`.

**Mở rộng:** Nếu một kênh lỗi (throw), các kênh còn lại vẫn phải gửi được; ghi log lỗi.

---

## Bài 1.6 ⭐⭐ – Abstraction: Payment Gateway
```
interface PaymentGateway {
    PaymentResult charge(orderId, amount, currency)
    PaymentResult refund(transactionId)
}
```
- Implement `MomoGateway`, `VnPayGateway`, `StripeGateway` (giả lập, random thành công/thất bại).
- `PaymentResult`: `success`, `transactionId`, `errorMessage`.
- Class `CheckoutService` chỉ phụ thuộc vào `PaymentGateway` (interface).

---

## Bài 1.7 ⭐⭐ – Composition vs Inheritance
**Đề:** `Order` gồm nhiều `OrderItem` (product, quantity, unitPrice), có `ShippingAddress`.
- `Order.getTotal()`, `addItem()`, `removeItem()`.
- Trạng thái: `PENDING → PAID → SHIPPED → DELIVERED` hoặc `CANCELLED`. Chuyển trạng thái sai (VD: `DELIVERED → PAID`) phải ném exception.

**Câu hỏi:** Vì sao `Order` **chứa** `OrderItem` (has-a) chứ không **kế thừa**? Khi nào nên ưu tiên composition?

---

## Bài 1.8 ⭐⭐ – Custom Exception Hierarchy
Thiết kế cây exception cho một REST API:
```
AppException (abstract): httpStatus, errorCode, message
 ├── NotFoundException       (404)
 ├── ValidationException     (400) + danh sách field lỗi
 ├── UnauthorizedException   (401)
 └── ConflictException       (409)
```
- Viết hàm `handle(AppException e) -> JSON` trả về:
```json
{ "status": 404, "code": "USER_NOT_FOUND", "message": "User 42 not found" }
```

---

# ➕ Bài tập bổ sung

## Bài 1.9 ⭐ – Class `Product` & `Inventory`
- `Product`: `sku`, `name`, `price`, `stock`. Không cho `price < 0`, `stock < 0`.
- `Inventory`: `addProduct`, `increaseStock(sku, qty)`, `decreaseStock(sku, qty)` (không đủ hàng → `OutOfStockException`), `findBySku`, `lowStock(threshold)`.

## Bài 1.10 ⭐ – Value Object `Money`
- `Money(amount, currency)` lưu bằng **số nguyên nhỏ nhất** (VD: cents / đồng), **không dùng float/double**.
- `add`, `subtract`, `multiply(factor)`, `equals`. Cộng 2 loại tiền khác nhau → exception.
- **Câu hỏi:** Vì sao `0.1 + 0.2 != 0.3` và nó nguy hiểm thế nào với hệ thống thanh toán?

## Bài 1.11 ⭐ – Static member: ID Generator
- Class `Ticket` tự tăng mã `TCK-0001`, `TCK-0002`... dùng biến **static**.
- `Ticket.count()` trả về số ticket đã tạo.
- **Câu hỏi:** Biến static có vấn đề gì khi chạy nhiều thread? Khi chạy nhiều server?

## Bài 1.12 ⭐ – Overloading vs Overriding
- `PriceCalculator.calculate(price)`, `calculate(price, quantity)`, `calculate(price, quantity, discountPercent)` (overloading).
- `Shape` → `Circle`, `Rectangle`, `Triangle` override `area()`, `perimeter()` (overriding).
- **Câu hỏi:** Cái nào quyết định lúc **compile-time**, cái nào lúc **runtime**?

## Bài 1.13 ⭐ – Enum có hành vi: `Role` & `Permission`
- `enum Permission { READ, WRITE, DELETE, MANAGE_USERS }`
- `enum Role { GUEST, USER, ADMIN }`, mỗi role có tập permission riêng; `role.can(Permission.DELETE)`.
- `User.hasPermission(p)`. Thêm role `MODERATOR` không phải sửa logic kiểm tra.

## Bài 1.14 ⭐ – Interface `Comparable`: Sắp xếp sản phẩm
- `Product implements Comparable<Product>` sort theo giá.
- Viết thêm các `Comparator`: theo tên, theo rating giảm dần, kết hợp nhiều tiêu chí.
- **Câu hỏi:** `Comparable` khác `Comparator` thế nào? Khi nào dùng cái nào?

## Bài 1.15 ⭐⭐ – Generic `ApiResponse<T>`
```
ApiResponse.ok(data)            -> { success: true,  data: ..., error: null }
ApiResponse.fail(code, message) -> { success: false, data: null, error: {code, message} }
```
- Dùng cho `ApiResponse<User>`, `ApiResponse<List<Product>>`, `ApiResponse<Page<Order>>`.
- **Mở rộng:** `Page<T>`: `items`, `page`, `size`, `totalItems`, `totalPages`, `hasNext`.

## Bài 1.16 ⭐⭐ – Generic `InMemoryRepository<T, ID>`
- Interface `Entity<ID> { getId() }`.
- `save`, `findById -> Optional<T>`, `findAll`, `deleteById`, `existsById`, `count`.
- Dùng lại cho `User`, `Product`, `Order` mà **không viết lại code**.

## Bài 1.17 ⭐⭐ – Immutable `AppConfig` với “wither”
- Các field `final`: `host`, `port`, `dbUrl`, `maxConnections`.
- Không có setter; muốn đổi thì `config.withPort(9090)` trả về **object mới**.
- **Câu hỏi:** Vì sao object immutable an toàn khi chia sẻ giữa nhiều thread?

## Bài 1.18 ⭐⭐ – Shallow copy vs Deep copy
- `Order` chứa `List<OrderItem>` và `Address`. Viết `shallowCopy()` và `deepCopy()`.
- Viết test chứng minh: sửa item trong bản shallow copy làm thay đổi bản gốc.

## Bài 1.19 ⭐⭐ – Giỏ hàng `ShoppingCart`
- `addItem(product, qty)` (thêm trùng sản phẩm thì cộng số lượng), `updateQuantity`, `removeItem`, `clear`.
- `getSubtotal()`, `getTax(rate)`, `getTotal()` dùng `Money` ở Bài 1.10.
- Không cho thêm quá số lượng tồn kho (dùng `Inventory` Bài 1.9).

## Bài 1.20 ⭐⭐ – Session / Token có hạn dùng
- `Session(userId, ttlSeconds)`: `token` ngẫu nhiên, `isExpired()`, `refresh()`.
- **Không gọi `now()` trực tiếp** – inject interface `Clock` để test được (VD: `FakeClock.advance(60)`).
- `SessionManager`: `create`, `validate(token)`, `revoke(token)`, `cleanupExpired()`.

## Bài 1.21 ⭐⭐ – Logger nhiều level & nhiều đích
- `enum Level { DEBUG, INFO, WARN, ERROR }`; logger chỉ in log ≥ level cấu hình.
- Interface `LogAppender`: `ConsoleAppender`, `FileAppender`, `JsonAppender`.
- Format: `2026-11-25T10:00:00 [ERROR] OrderService - Payment failed orderId=42`.

## Bài 1.22 ⭐⭐ – Quản lý tài nguyên (try-with-resources / context manager)
- `FakeDbConnection implements AutoCloseable` (Python: `__enter__/__exit__`).
- In ra `open` / `close`; đảm bảo **luôn close** kể cả khi có exception.
- **Mở rộng:** `Transaction` tự `rollback()` nếu có exception, `commit()` nếu thành công.

## Bài 1.23 ⭐⭐ – Interface mặc định / Mixin: `Auditable`, `SoftDeletable`
- `Auditable`: `createdAt`, `updatedAt`, `createdBy`, `touch(user)`.
- `SoftDeletable`: `deletedAt`, `softDelete()`, `isDeleted()`.
- `Product` dùng cả 2; repository `findAll()` tự bỏ qua bản ghi đã soft-delete.

## Bài 1.24 ⭐⭐ – Bãi đỗ xe đơn giản (Parking Lot)
- `Vehicle` (abstract) → `Motorbike`, `Car`, `Truck`; `ParkingSpot` có size `SMALL/MEDIUM/LARGE`.
- `park(vehicle) -> Ticket`, `leave(ticket) -> fee` (tính theo giờ, mỗi loại xe giá khác nhau).
- Liên quan LeetCode **1603**.

## Bài 1.25 ⭐⭐ – Máy ATM
- Các mệnh giá 20k, 50k, 100k, 200k, 500k. `deposit(counts)`, `withdraw(amount)` ưu tiên tờ lớn.
- Không rút được đúng số tiền → **không thay đổi** trạng thái máy (rollback).
- Liên quan LeetCode **2241**.

## Bài 1.26 ⭐⭐ – Lịch sử trình duyệt / Điều hướng Admin
- `visit(url)`, `back(steps)`, `forward(steps)`. `visit` mới xóa toàn bộ lịch sử forward.
- Liên quan LeetCode **1472**.

## Bài 1.27 ⭐⭐ – Event Emitter (tiền đề cho Observer)
- `on(event, callback) -> subscription`, `emit(event, ...args) -> List<results>`, `subscription.unsubscribe()`.
- Liên quan LeetCode **2694**.

---

# 🧩 Luyện tập LeetCode (OOP / Design – mức Easy → Medium)

> Các bài “Design” trên LeetCode chính là bài OOP: thiết kế class với các method và trạng thái nội bộ. Hãy chú ý **đóng gói** (field private) và **độ phức tạp** từng method.

| # | Bài LeetCode | Độ khó | Kiến thức OOP luyện được | Liên quan |
|---|--------------|--------|--------------------------|-----------|
| 1 | [1603. Design Parking System](https://leetcode.com/problems/design-parking-system/) | Easy | Class, constructor, trạng thái | 1.24 |
| 2 | [2043. Simple Bank System](https://leetcode.com/problems/simple-bank-system/) | Medium | Encapsulation, validate giao dịch | 1.2 |
| 3 | [2241. Design an ATM Machine](https://leetcode.com/problems/design-an-atm-machine/) | Medium | Trạng thái + rollback | 1.25 |
| 4 | [705. Design HashSet](https://leetcode.com/problems/design-hashset/) | Easy | Ẩn cài đặt sau interface | 1.3 |
| 5 | [706. Design HashMap](https://leetcode.com/problems/design-hashmap/) | Easy | Ẩn cài đặt sau interface | 1.16 |
| 6 | [707. Design Linked List](https://leetcode.com/problems/design-linked-list/) | Medium | Class lồng nhau (Node) | 1.7 |
| 7 | [155. Min Stack](https://leetcode.com/problems/min-stack/) | Medium | Bất biến (invariant) nội bộ | – |
| 8 | [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/) | Easy | Composition | 1.7 |
| 9 | [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/) | Easy | Composition | 1.7 |
| 10 | [622. Design Circular Queue](https://leetcode.com/problems/design-circular-queue/) | Medium | Đóng gói con trỏ head/tail | – |
| 11 | [1472. Design Browser History](https://leetcode.com/problems/design-browser-history/) | Medium | Quản lý trạng thái | 1.26 |
| 12 | [1656. Design an Ordered Stream](https://leetcode.com/problems/design-an-ordered-stream/) | Easy | Trạng thái con trỏ | – |
| 13 | [1797. Design Authentication Manager](https://leetcode.com/problems/design-authentication-manager/) | Medium | Token có TTL | 1.20 |
| 14 | [1357. Apply Discount Every n Orders](https://leetcode.com/problems/apply-discount-every-n-orders/) | Medium | Business logic trong class | 1.19 |
| 15 | [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/) | Easy | Class + Queue nội bộ | – |
| 16 | [1396. Design Underground System](https://leetcode.com/problems/design-underground-system/) | Medium | Nhiều HashMap, thiết kế dữ liệu | – |
| 17 | [1845. Seat Reservation Manager](https://leetcode.com/problems/seat-reservation-manager/) | Medium | Class + Heap | 1.24 |
| 18 | [1381. Design a Stack With Increment Operation](https://leetcode.com/problems/design-a-stack-with-increment-operation/) | Medium | Tối ưu trạng thái | – |
| 19 | [1600. Throne Inheritance](https://leetcode.com/problems/throne-inheritance/) | Medium | Mô hình hóa quan hệ cha–con | 1.4 |
| 20 | [2694. Event Emitter](https://leetcode.com/problems/event-emitter/) | Medium | Callback, đa hình qua hàm | 1.5, 1.27 |
| 21 | [2726. Calculator with Method Chaining](https://leetcode.com/problems/calculator-with-method-chaining/) | Easy | Method chaining (`return this`) | – |
| 22 | [2618. Check if Object Instance of Class](https://leetcode.com/problems/check-if-object-instance-of-class/) | Medium | Prototype chain / kế thừa | 1.4 |

---

## Checklist sau Phần 1
- [ ] Giải thích được 4 tính chất OOP bằng ví dụ backend của chính mình
- [ ] Phân biệt abstract class vs interface
- [ ] Hiểu equals/hashCode, immutable object
- [ ] Biết khi nào dùng composition thay vì inheritance
