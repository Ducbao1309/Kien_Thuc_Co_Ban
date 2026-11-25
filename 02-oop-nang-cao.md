# Phần 2 – OOP Nâng cao: SOLID, Design Patterns, Kiến trúc

## A. SOLID

### Bài 2.1 ⭐⭐⭐ – S: Single Responsibility (Refactor)
Code "God class" dưới đây vi phạm SRP. Hãy tách thành nhiều class.
```java
class UserService {
    void register(String email, String password) {
        if (!email.contains("@")) throw new RuntimeException("Invalid");   // validate
        String hash = md5(password);                                        // hashing
        db.execute("INSERT INTO users ...");                                // persistence
        smtp.send(email, "Welcome!");                                       // email
        System.out.println("User registered " + email);                    // logging
    }
}
```
**Kết quả mong đợi:** `UserValidator`, `PasswordHasher`, `UserRepository`, `EmailService`, `Logger`, và `UserService` chỉ điều phối.

---

### Bài 2.2 ⭐⭐⭐ – O: Open/Closed – Tính phí vận chuyển
Code ban đầu:
```
if (type == "STANDARD") fee = 20000
else if (type == "EXPRESS") fee = 50000
else if (type == "SAME_DAY") fee = 80000
```
- Refactor để thêm loại mới (`INTERNATIONAL`, tính theo cân nặng) **không phải sửa** code cũ.
- Gợi ý: interface `ShippingStrategy` + `Map<String, ShippingStrategy>` registry.

---

### Bài 2.3 ⭐⭐⭐ – L: Liskov Substitution
```
class FileStorage { save(file); read(id); delete(id) }
class ReadOnlyStorage extends FileStorage { delete(id) { throw UnsupportedOperation } }
```
- Giải thích vì sao vi phạm LSP.
- Thiết kế lại bằng interface `Readable`, `Writable`, `Deletable`.

---

### Bài 2.4 ⭐⭐⭐ – I: Interface Segregation
Interface `Repository` có 15 method (`findAll`, `findById`, `save`, `bulkInsert`, `exportCsv`, `search`...). Một số repo chỉ cần đọc.
- Chia nhỏ thành `ReadRepository<T, ID>`, `WriteRepository<T, ID>`, `SearchableRepository<T>`.
- Dùng **Generics**.

---

### Bài 2.5 ⭐⭐⭐ – D: Dependency Inversion + Dependency Injection
- `OrderService` hiện `new MySqlOrderRepository()` bên trong. Refactor để nhận `OrderRepository` qua constructor.
- Viết `InMemoryOrderRepository` để **unit test** `OrderService` không cần DB.
- **Mở rộng:** Tự viết một DI container mini: `container.register(Interface, Impl)`, `container.resolve(Interface)`.

---

## B. Design Patterns thường gặp ở Backend

### Bài 2.6 ⭐⭐⭐ – Singleton: Connection Pool / Config
- `AppConfig` đọc file `.env` 1 lần, dùng chung toàn app.
- Phải **thread-safe** (double-checked locking hoặc enum/module-level).
- **Câu hỏi:** Vì sao Singleton bị coi là anti-pattern trong nhiều trường hợp? DI giải quyết thế nào?

### Bài 2.7 ⭐⭐⭐ – Factory: Tạo Notifier từ config
`NotifierFactory.create("email" | "sms" | "push")` trả về đúng `Notifier`. Loại không hỗ trợ → exception.

### Bài 2.8 ⭐⭐⭐ – Builder: Xây dựng HTTP Request / Query
```
HttpRequest req = HttpRequest.builder()
    .url("https://api.x.com/users")
    .method("POST")
    .header("Authorization", "Bearer abc")
    .body("{...}")
    .timeout(5000)
    .build();
```
- `build()` validate: url bắt buộc, GET không được có body.
- **Mở rộng:** `QueryBuilder.select("id","name").from("users").where("age > ?", 18).orderBy("name").limit(10).build()` → chuỗi SQL + params.

### Bài 2.9 ⭐⭐⭐ – Strategy: Discount Engine
Các loại giảm giá: phần trăm, số tiền cố định, mua X tặng Y, giảm cho thành viên VIP. Cho phép **kết hợp nhiều mã** (chọn mức có lợi nhất hoặc cộng dồn tùy config).

### Bài 2.10 ⭐⭐⭐⭐ – Observer / Event Bus
- `EventBus.subscribe(eventType, handler)`, `EventBus.publish(event)`.
- Khi `OrderPlacedEvent` được publish: gửi email, trừ kho, cộng điểm thưởng – mỗi việc là 1 handler độc lập.
- **Mở rộng:** handler chạy bất đồng bộ (thread pool), có retry khi lỗi.

### Bài 2.11 ⭐⭐⭐⭐ – Decorator: Middleware
```
interface Handler { Response handle(Request req) }
```
- Viết các decorator: `LoggingHandler`, `AuthHandler`, `TimingHandler`, `CachingHandler` bọc quanh handler thật.
- Ghép chuỗi: `new Logging(new Auth(new Timing(new UserController())))`.
- **Câu hỏi:** Đây chính là cơ chế middleware trong Express / Spring Filter. Giải thích.

### Bài 2.12 ⭐⭐⭐⭐ – Chain of Responsibility: Xử lý đơn hàng
Pipeline validate đơn: `StockCheck → FraudCheck → PaymentCheck → AddressCheck`. Bước nào fail thì dừng và trả lý do.

### Bài 2.13 ⭐⭐⭐⭐ – State Pattern: Vòng đời đơn hàng
Làm lại Bài 1.7 nhưng mỗi trạng thái là một class (`PendingState`, `PaidState`...) với các method `pay()`, `ship()`, `cancel()`. Loại bỏ toàn bộ `switch/if` theo trạng thái.

### Bài 2.14 ⭐⭐⭐⭐ – Adapter: Tích hợp bên thứ 3
Hệ thống dùng interface `SmsSender.send(phone, text)`. Thư viện mới của Twilio có API `TwilioClient.messages.create({to, body, from})`. Viết `TwilioAdapter`.

---

## C. Kiến trúc

### Bài 2.15 ⭐⭐⭐⭐ – Layered Architecture (không dùng framework)
Xây dựng module **Quản lý sản phẩm** gồm các tầng:
```
Controller  -> nhận request (giả lập bằng hàm), trả DTO
Service     -> business logic
Repository  -> interface + InMemory implementation
Entity / DTO / Mapper
```
- Chức năng: CRUD, tìm kiếm theo tên, phân trang (`page`, `size`), sort theo giá.
- Exception dùng cây ở Bài 1.8.
- Viết unit test cho Service (mock Repository).

---

## Checklist sau Phần 2
- [ ] Nhìn code và chỉ ra vi phạm SOLID nào
- [ ] Code thuộc lòng: Singleton, Factory, Builder, Strategy, Observer, Decorator
- [ ] Hiểu DI và lý do nó giúp test dễ hơn
- [ ] Tách được Controller / Service / Repository
