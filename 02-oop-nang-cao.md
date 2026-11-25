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

# ➕ Bài tập bổ sung

## D. Design Patterns (tiếp)

### Bài 2.16 ⭐⭐⭐ – Template Method: Import dữ liệu
Abstract class `DataImporter` có `importFile(path)` cố định các bước: `read → parse → validate → save → report`.
- Lớp con `CsvImporter`, `JsonImporter`, `ExcelImporter` chỉ override `parse()` (và tùy chọn `validate()`).
- **Câu hỏi:** Template Method khác Strategy ở đâu (kế thừa vs composition)?

### Bài 2.17 ⭐⭐⭐ – Command: Job có Undo
- Interface `Command { execute(); undo(); }`. Các lệnh: `CreateUserCommand`, `ChangePriceCommand`, `BulkDeleteCommand`.
- `CommandInvoker` lưu lịch sử, hỗ trợ `undo()` / `redo()` và **ghi audit log** mỗi lệnh.
- Liên quan LeetCode **2296** (Design a Text Editor).

### Bài 2.18 ⭐⭐⭐ – Proxy: Caching & Access Control
- `ProductService` thật gọi DB (giả lập `sleep 200ms`).
- `CachingProductServiceProxy` cache kết quả `getById` với TTL; `SecuredProductServiceProxy` kiểm tra quyền trước khi `delete`.
- **Câu hỏi:** Proxy khác Decorator thế nào? (`@Transactional`, `@Cacheable` của Spring là Proxy.)
- Liên quan LeetCode **2622**, **2623**.

### Bài 2.19 ⭐⭐⭐ – Facade: Checkout
`CheckoutFacade.placeOrder(cartId, paymentInfo)` gọi bên trong: `InventoryService.reserve`, `PricingService.calculate`, `PaymentService.charge`, `OrderService.create`, `NotificationService.send`.
- Nếu thanh toán lỗi → **hoàn lại kho đã reserve** (compensation).

### Bài 2.20 ⭐⭐⭐ – Composite: Cây quyền / Menu
- `PermissionComponent` → `Permission` (lá) và `PermissionGroup` (chứa nhiều component).
- `group.has("order:delete")` duyệt đệ quy. Group có thể lồng group.
- **Mở rộng:** Tính tổng dung lượng thư mục (File/Folder) theo cùng pattern.

### Bài 2.21 ⭐⭐⭐ – Iterator: Duyệt API phân trang
- `PagedIterator<T>` bọc một hàm `fetchPage(cursor) -> (items, nextCursor)`.
- Client dùng `for (item : iterator)` mà **không biết** gì về phân trang; chỉ gọi trang tiếp khi cần (lazy).
- Liên quan LeetCode **341**, **284**, **173**.

### Bài 2.22 ⭐⭐⭐ – Abstract Factory: Đa database
- `DatabaseFactory` → `MySqlFactory`, `PostgresFactory`; mỗi factory tạo bộ đôi `Connection` + `QueryDialect` tương thích (VD: `LIMIT/OFFSET` vs `FETCH FIRST`, quote `` ` `` vs `"`).
- **Câu hỏi:** Abstract Factory khác Factory Method thế nào?

### Bài 2.23 ⭐⭐⭐ – Prototype: Template email
- `EmailTemplate` (subject, body, attachments, headers) có `clone()` deep copy.
- `TemplateRegistry` lưu template gốc; mỗi lần gửi clone rồi điền biến `{{name}}`, `{{orderId}}`.

### Bài 2.24 ⭐⭐⭐ – Null Object & Optional
- Thay vì `if (logger != null)`, `if (discount != null)` khắp nơi → `NoOpLogger`, `NoDiscount`.
- Refactor 1 đoạn code có ≥ 5 lần kiểm tra null.

### Bài 2.25 ⭐⭐⭐⭐ – Object Pool: Connection Pool
- `ConnectionPool(maxSize)`: `acquire(timeout)`, `release(conn)`. Hết connection → chờ tối đa `timeout` rồi exception.
- Thread-safe (semaphore / blocking queue); kiểm tra connection còn “sống” trước khi trả ra.
- Liên quan LeetCode **1188** (premium) – Design Bounded Blocking Queue.

### Bài 2.26 ⭐⭐⭐⭐ – Circuit Breaker (State pattern)
- 3 trạng thái: `CLOSED → OPEN` (lỗi ≥ N lần liên tiếp) `→ HALF_OPEN` (sau T giây) `→ CLOSED/OPEN`.
- `breaker.call(supplier)`; khi `OPEN` trả fallback ngay không gọi service.
- Kết hợp **Retry với exponential backoff + jitter**. Inject `Clock` để test.

### Bài 2.27 ⭐⭐⭐⭐ – Memento: Lưu nháp bài viết
- `Article.save() -> Memento`, `Article.restore(memento)`; `History` giữ tối đa 20 phiên bản.
- Memento không cho bên ngoài đọc/sửa trạng thái bên trong (đóng gói).

### Bài 2.28 ⭐⭐⭐⭐ – Mediator: Chat room
- `ChatRoom` (mediator) điều phối `User.send(msg)`; user không giữ tham chiếu trực tiếp tới nhau.
- Hỗ trợ tin nhắn riêng, mute user, bot tự động trả lời khi có từ khóa.

### Bài 2.29 ⭐⭐⭐⭐ – Visitor: Xuất báo cáo nhiều định dạng
- Cây đối tượng `Report → Section → Table / Paragraph / Chart`.
- `JsonExportVisitor`, `HtmlExportVisitor`, `WordCountVisitor` – thêm định dạng mới không sửa các class node.
- Liên quan LeetCode **1628** (premium) – Expression Tree.

### Bài 2.30 ⭐⭐⭐⭐ – Specification Pattern: Bộ lọc sản phẩm
```
spec = priceBetween(100, 500).and(inCategory("phone")).and(not(outOfStock()))
repo.findAll(spec)
```
- Mỗi spec có `isSatisfiedBy(product)` và **có thể dịch sang SQL** `toSql() -> (where, params)`.

## E. Kiến trúc & Thiết kế hệ thống nhỏ

### Bài 2.31 ⭐⭐⭐⭐ – Unit of Work + Repository
- `UnitOfWork` theo dõi entity `new / dirty / removed`; `commit()` ghi tất cả hoặc không gì cả.
- Mô phỏng chuyển tiền giữa 2 tài khoản: lỗi giữa chừng → rollback toàn bộ.

### Bài 2.32 ⭐⭐⭐⭐ – Hexagonal Architecture (Ports & Adapters)
- Domain `Order` không import bất kỳ thứ gì từ DB/HTTP.
- Port: `OrderRepositoryPort`, `PaymentPort`, `NotificationPort`. Adapter: `InMemory`, `FakeStripe`, `ConsoleEmail`.
- Viết **2 driving adapter**: CLI và HTTP giả lập – dùng chung một use case `PlaceOrderUseCase`.

### Bài 2.33 ⭐⭐⭐⭐ – CQRS mini
- Command side: `CreateProduct`, `UpdatePrice` ghi vào `EventStore` (danh sách event).
- Query side: `ProductReadModel` được cập nhật từ event, tối ưu cho `search` và `topCheapest`.
- **Mở rộng:** Event Sourcing – dựng lại trạng thái sản phẩm từ đầu bằng cách replay event.

### Bài 2.34 ⭐⭐⭐⭐ – Plugin Architecture
- Interface `PaymentPlugin { name(); supports(currency); pay(...) }`.
- `PluginRegistry` tự đăng ký plugin (annotation / decorator / ServiceLoader / entry points).
- Thêm plugin mới chỉ bằng **thêm 1 file** – không sửa registry (Open/Closed).

### Bài 2.35 ⭐⭐⭐⭐ – Refactor Code Smells
Cho đoạn code ~150 dòng (tự viết hoặc lấy từ project cũ) có: method dài, `switch` theo type, magic number, duplicate code, data class nghèo nàn (anemic), feature envy.
- Liệt kê từng smell → pattern/kỹ thuật refactor tương ứng → refactor từng bước, **giữ test xanh** sau mỗi bước.

### Bài 2.36 ⭐⭐⭐⭐ – Multi-tenant Service
- Mỗi request có `tenantId`; `TenantContext` (ThreadLocal / context var) giữ tenant hiện tại.
- Repository tự lọc dữ liệu theo tenant; middleware (Decorator Bài 2.11) gán tenant từ header.
- **Câu hỏi:** Ưu/nhược của: chung DB chung bảng / chung DB khác schema / mỗi tenant một DB.

---

# 🧩 Luyện tập LeetCode (Design – mức Medium → Hard)

> Với mỗi bài: vẽ sơ đồ class trước, chọn cấu trúc dữ liệu cho từng method, ghi Big-O, rồi mới code.

| # | Bài LeetCode | Độ khó | Pattern / Kỹ năng | Liên quan |
|---|--------------|--------|-------------------|-----------|
| 1 | [146. LRU Cache](https://leetcode.com/problems/lru-cache/) | Medium | Proxy cache, HashMap + DLL | 2.18 |
| 2 | [460. LFU Cache](https://leetcode.com/problems/lfu-cache/) | Hard | Thiết kế nhiều cấu trúc phối hợp | 2.18 |
| 3 | [355. Design Twitter](https://leetcode.com/problems/design-twitter/) | Medium | Observer / feed, Heap | 2.10 |
| 4 | [1146. Snapshot Array](https://leetcode.com/problems/snapshot-array/) | Medium | Memento, versioning | 2.27 |
| 5 | [981. Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/) | Medium | Versioned storage | 2.33 |
| 6 | [2296. Design a Text Editor](https://leetcode.com/problems/design-a-text-editor/) | Hard | Command / Undo | 2.17 |
| 7 | [341. Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/) | Medium | Iterator + Composite | 2.20, 2.21 |
| 8 | [284. Peeking Iterator](https://leetcode.com/problems/peeking-iterator/) | Medium | Decorator trên Iterator | 2.11, 2.21 |
| 9 | [173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/) | Medium | Lazy Iterator | 2.21 |
| 10 | [1286. Iterator for Combination](https://leetcode.com/problems/iterator-for-combination/) | Medium | Iterator | 2.21 |
| 11 | [1628. Design an Expression Tree With Evaluate Function](https://leetcode.com/problems/design-an-expression-tree-with-evaluate-function/) 🔒 | Medium | Composite / Polymorphism / Factory | 2.29 |
| 12 | [2353. Design a Food Rating System](https://leetcode.com/problems/design-a-food-rating-system/) | Medium | Read model / index | 2.33 |
| 13 | [1912. Design Movie Rental System](https://leetcode.com/problems/design-movie-rental-system/) | Hard | Nhiều index sorted | 2.30 |
| 14 | [380. Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/) | Medium | Kết hợp cấu trúc dữ liệu | – |
| 15 | [432. All O'one Data Structure](https://leetcode.com/problems/all-oone-data-structure/) | Hard | Bucket + DLL | – |
| 16 | [2502. Design Memory Allocator](https://leetcode.com/problems/design-memory-allocator/) | Medium | Object Pool / cấp phát tài nguyên | 2.25 |
| 17 | [2349. Design a Number Container System](https://leetcode.com/problems/design-a-number-container-system/) | Medium | Index + lazy deletion | – |
| 18 | [1348. Tweet Counts Per Frequency](https://leetcode.com/problems/tweet-counts-per-frequency/) | Medium | Thống kê theo cửa sổ thời gian | – |
| 19 | [1114. Print in Order](https://leetcode.com/problems/print-in-order/) | Easy | Đồng bộ thread (nền tảng Singleton thread-safe) | 2.6 |
| 20 | [1117. Building H2O](https://leetcode.com/problems/building-h2o/) | Medium | Semaphore / barrier | 2.25 |
| 21 | [1226. The Dining Philosophers](https://leetcode.com/problems/the-dining-philosophers/) | Medium | Deadlock, lock ordering | 2.25 |
| 22 | [2622. Cache With Time Limit](https://leetcode.com/problems/cache-with-time-limit/) | Medium | Cache có TTL (Proxy) | 2.18 |
| 23 | [2623. Memoize](https://leetcode.com/problems/memoize/) | Medium | Decorator / Proxy cho hàm | 2.11, 2.18 |
| 24 | [2627. Debounce](https://leetcode.com/problems/debounce/) | Medium | Decorator hàm | 2.11 |
| 25 | [2666. Allow One Function Call](https://leetcode.com/problems/allow-one-function-call/) | Easy | Decorator / Singleton init | 2.6, 2.11 |
| 26 | [2637. Promise Time Limit](https://leetcode.com/problems/promise-time-limit/) | Medium | Timeout wrapper (tiền đề Circuit Breaker) | 2.26 |

> 🔒 = bài Premium.

---

## Checklist sau Phần 2
- [ ] Nhìn code và chỉ ra vi phạm SOLID nào
- [ ] Code thuộc lòng: Singleton, Factory, Builder, Strategy, Observer, Decorator
- [ ] Hiểu DI và lý do nó giúp test dễ hơn
- [ ] Tách được Controller / Service / Repository
