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

## Checklist sau Phần 1
- [ ] Giải thích được 4 tính chất OOP bằng ví dụ backend của chính mình
- [ ] Phân biệt abstract class vs interface
- [ ] Hiểu equals/hashCode, immutable object
- [ ] Biết khi nào dùng composition thay vì inheritance
