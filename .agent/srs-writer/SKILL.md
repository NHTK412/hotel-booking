---
name: srs-writer
description: Quy chuẩn và quy trình viết Tài liệu Đặc tả Yêu cầu Phần mềm (SRS - Software Requirement Specification) chuẩn Business Analyst (BA) chuyên nghiệp. Kích hoạt khi người dùng yêu cầu viết mới SRS, trích xuất/cập nhật SRS từ mã nguồn (Code-to-SRS), đặc tả bổ sung module/tính năng, hoặc review và chuẩn hóa tài liệu đặc tả yêu cầu phần mềm.
---

# Quy Chuẩn Viết Tài Liệu Đặc Tả Yêu Cầu Phần Mềm (`srs-writer`)

Tài liệu này định nghĩa phương pháp luận, quy trình phỏng vấn khai thác yêu cầu (Requirement Elicitation), cấu trúc chuẩn 5 chương, văn phong nghiệp vụ và bộ tiêu chuẩn chất lượng để Agent đóng vai trò một **Senior Business Analyst (BA) kết hợp Solution Architect** khi biên soạn, trích xuất hoặc chuẩn hóa **Tài liệu Đặc tả Yêu cầu Phần mềm (Software Requirement Specification - SRS)**.

Bộ khung mẫu hoàn chỉnh được đặt tại: [references/srs-template.md](./references/srs-template.md).

---

## 1. Quy Trình Phỏng Vấn Khai Thác Yêu Cầu (Smart BA Discovery & Interview Protocol)

> [!IMPORTANT]
> **NGUYÊN TẮC BẮT BUỘC TRƯỚC KHI VIẾT SRS:**
> Khi người dùng kích hoạt skill này, Agent BA **không được lập tức tự suy diễn và viết ngay toàn bộ SRS** nếu các thông số nghiệp vụ/kiến trúc chưa rõ ràng. Agent phải thực hiện quy trình **Phỏng vấn Khai thác Yêu cầu Thông minh (Smart BA Discovery)** với Developer (BE/FE) hoặc Product Owner để đạt được sự thống nhất hoàn toàn trước khi xuất tài liệu.

### 1.1. Ba Quy Tắc Phỏng Vấn Của Senior BA
1. **Khảo sát Codebase & Tài liệu trước khi hỏi (Explore Before Asking)**:
   - Nếu trong workspace đã có sẵn mã nguồn, `AGENTS.md`, `README.md`, `docker-compose.yml`, Entity/DTO, file cấu hình (`application.yml`, `.env.example`) hoặc tài liệu nháp, Agent **bắt buộc phải đọc lướt trước**.
   - Tuyệt đối không hỏi lại những thông tin đã có sẵn câu trả lời rõ ràng trong codebase. Thay vào đó, hãy dùng thông tin đọc được từ code làm cơ sở để đặt câu hỏi xác nhận hoặc hỏi sâu vào các góc khuất nghiệp vụ.
2. **Hỏi từng bước theo nhánh quyết định kèm Phương án Đề xuất (`(Recommended)`)**:
   - Ưu tiên sử dụng công cụ `ask_question` (hoặc đặt câu hỏi ngắn gọn, rõ ràng) để làm rõ từng nhánh quyết định từ tổng quan xuống chi tiết.
   - Trong mọi câu hỏi, Agent BA phải đóng vai chuyên gia tư vấn: luôn đưa ra **phương án khuyến nghị tốt nhất ở đầu tiên (gắn nhãn `(Recommended)`)** dựa trên chuẩn mực thiết kế hệ thống và dữ kiện đã biết từ dự án để BE/User dễ dàng chốt nhanh.
3. **Chốt thiết kế rồi mới xuất bản (Confirm Before Generating)**:
   - Chỉ khi đã làm rõ các điểm mấu chốt trên cây quyết định, Agent mới tiến hành viết tài liệu SRS hoàn chỉnh theo đúng khung chuẩn.

### 1.2. Cây Câu Hỏi Khai Thác Yêu Cầu 4 Nhánh (4-Branch BA Discovery Tree)

Tùy vào những gì còn thiếu trong yêu cầu của người dùng, Agent BA lần lượt đi qua 4 nhánh câu hỏi sau:

#### Nhánh 1: Mục tiêu, Phạm vi & Tác nhân (Scope, Actors & External Integrations — Chương 1 & Mục 2.1–2.3)
- **Phạm vi bàn giao (`In-Scope` vs. `Out-of-Scope`)**: Phiên bản SRS này viết cho toàn bộ nền tảng hay chỉ một phân hệ cụ thể? Những tính năng nào chủ động loại khỏi phạm vi phiên bản hiện tại (VD: ủy thác vận chuyển cho 3PL, chưa làm app mobile riêng cho Seller, chưa bao gồm ERP/Kế toán nội bộ)?
- **Tác nhân (`System Actors`) & Đối tác thứ ba (`External Systems`)**: Hệ thống có những vai trò người dùng nào (`Buyer`, `Seller`, `Admin`, `Support Staff`...) và tích hợp với những đối tác bên ngoài nào (Cổng thanh toán Stripe/VNPay/VietQR, Hãng vận chuyển GHN/GHTK, Dịch vụ Email/Push/SMS)?

#### Nhánh 2: Kiến trúc, Phân hệ & Giao tiếp (Architecture & Service Boundaries — Mục 2.4–2.5 & Chương 4)
- **Mô hình kiến trúc**: Hệ thống triển khai theo kiến trúc **Microservices + Event-Driven** (API Gateway, các service độc lập DB, Message Broker Kafka/RabbitMQ) hay **Monolith / Modular Monolith**?
- **Danh sách Phân hệ / Microservices**: Xác nhận danh sách các phân hệ cần đặc tả và ranh giới trách nhiệm giữa chúng (VD: dịch vụ nào sở hữu việc tạo đơn hàng khi đấu giá kết thúc? Dịch vụ nào quản lý tồn kho?).
- **Xác thực & Phân quyền**: Cơ chế truyền định danh (JWT tại Gateway + Header `X-User-Id`, `X-User-Roles` xuống downstream) và ma trận phân quyền RBAC (**Mục 2.7**) có quy định đặc biệt nào không?

#### Nhánh 3: Luật Nghiệp vụ Lõi, Máy Trạng thái & Cấu hình (Core Domain Rules, State Machines & Configs — Mục 2.6, 2.8 & Chương 3)
- **Các luồng nghiệp vụ & Máy trạng thái (State Lifecycle)**:
  - Chuỗi trạng thái của các thực thể chính (Đơn hàng, Phiên đấu giá, Kiện hàng vận chuyển, Hồ sơ người bán...).
  - Các công thức tính toán cốt lõi (VD: Công thức tồn kho khả dụng theo SKU + Kho, công thức tính phí vận chuyển tách riêng theo từng Shop, cách áp dụng Voucher tiền hàng & Voucher phí ship, bước giá đấu giá tối thiểu).
- **Tham số cấu hình & Kịch bản biên (Configurations & Edge Cases)**:
  - Các ngưỡng thời gian chờ (Timeout giữ kho, thời hạn checkout, hạn thanh toán sau thắng đấu giá 24h, gia hạn Anti-Sniping phút chót).
  - Các điều kiện ràng buộc dữ liệu: Khi nào cấm xóa bản ghi (đã có đơn hàng/sản phẩm liên kết)? Có áp dụng xóa mềm (Soft Delete) không? Hình phạt khi vi phạm (trừ điểm uy tín) là gì?
  - Yêu cầu xử lý đồng thời tải cao (Optimistic Locking, Redis Cache, chống Overselling) và cửa sổ kiểm tra tính lũy biến (Idempotency).

#### Nhánh 4: Chỉ tiêu Phi chức năng, SLA & Kiểm toán (NFRs, SLA & Auditing — Chương 5)
- **Hiệu năng & Luồng tối quan trọng (`Critical Path`)**: Tác vụ nào đòi hỏi độ trễ thấp nhất (VD: Đặt giá thầu $\le 300\text{ ms}$, Đăng nhập/Giỏ hàng $\le 500\text{ ms}$)? Chỉ tiêu thông lượng (Throughput) và cam kết Uptime SLA ($\ge 99.9\%$) là bao nhiêu?
- **Lưu trữ, Kiểm toán & Khôi phục**: Thời hạn lưu trữ tối thiểu cho Đơn hàng, Lịch sử giao dịch, System Logs; yêu cầu Sổ cái bất biến (Immutable Ledger / Audit Logs) và mục tiêu RTO/RPO.

---

## 2. Nguyên Tắc Cốt Lõi Của Một BA Chuyên Nghiệp (Core BA Invariants)

Khi biên soạn bất kỳ tài liệu SRS nào, Agent bắt buộc phải tuân thủ 5 nguyên tắc bất biến sau:

1. **Tham chiếu chéo 3 chiều chặt chẽ (3-Way Cross-Referencing)**:
   - Mọi thông số giới hạn, thời gian chờ (timeout), ngưỡng điểm, cờ bật/tắt tính năng được nhắc tới trong **Chương 3 (Yêu cầu Chức năng)** bắt buộc phải được khai báo tại **Mục 2.6 (Danh mục Cấu hình Hệ thống)** và trích dẫn trực tiếp mã cấu hình trong ngoặc đơn, ví dụ: `(`CHECKOUT_TIMEOUT_MINUTES = 15`)`, `(`MIN_REPUTATION_TO_BID >= 40`)`.
   - Mọi hành động bị giới hạn theo vai trò trong Chương 3 phải có mã quyền tương ứng trong **Mục 2.7 (Danh sách Quyền hạn - RBAC Matrix)**, ví dụ: `PRODUCT.CREATE`, `AUCTION.ADMIN_CANCEL`.
   - Mọi sự kiện bất đồng bộ phát sinh khi chuyển trạng thái trong Chương 3 phải được định nghĩa tại **Mục 2.8 (Bảng Thông điệp Sự kiện Domain)** và trích dẫn đúng tên sự kiện trong câu đặc tả, ví dụ: phát sự kiện `OrderCompleted`, `Outbid`, `AuctionCancelled`.
   - **Tuyệt đối không để xuất hiện "con số ma thuật" (magic numbers) hoặc "sự kiện mồ côi" (orphan events)** trong Chương 3 mà không có mặt ở Chương 2.

2. **Chuẩn hóa thuật ngữ song ngữ (`Tiếng Việt (English Term)`)**:
   - Sử dụng tiếng Việt kỹ thuật trang trọng, mạch lạc, chính xác.
   - Tại các tiêu đề chương/mục và các khái niệm nghiệp vụ/kiến trúc quan trọng, luôn ghi kèm thuật ngữ tiếng Anh chuẩn trong ngoặc đơn, ví dụ: *Tính lũy biến (Idempotency)*, *Cơ chế chống trả giá phút chót (Anti-Sniping)*, *Khóa lạc quan (Optimistic Locking)*, *Sổ cái tồn kho bất biến (Immutable Inventory Ledger)*.
   - Bọc tất cả tên biến cấu hình (`ALLOW_OVERSELL`), mã quyền (`ORDER.REFUND`), tên sự kiện (`BidPlaced`), trạng thái (`Created`, `Paid`, `Settled`) và đường dẫn API (`/api/v1`) trong cặp dấu backticks `` `...` ``.

3. **Đặc tả định lượng, có thể kiểm thử (Quantifiable & Testable)**:
   - Tuyệt đối không dùng từ ngữ cảm tính, mơ hồ như *"xử lý nhanh"*, *"chịu tải cao"*, *"số lượng vừa phải"*, *"đợi một lúc"*.
   - Mọi yêu cầu đều phải lượng hóa bằng con số, công thức toán học tường minh (VD: `Available = Total - Reserved - Unavailable`), chuỗi chuyển tiếp trạng thái (`Created` -> `Paid` -> `Processing`), hoặc chỉ tiêu SLA định lượng ($\le 300\text{ ms}$, $\ge 99.9\%$).

4. **Thích ứng linh hoạt theo Kiến trúc (Architecture-Adaptive)**:
   - Giữ vững bộ khung 5 chương làm tiêu chuẩn cốt lõi cho mọi dự án, đồng thời tự động điều chỉnh nội dung kỹ thuật phù hợp với mô hình kiến trúc của hệ thống:

   | Thành phần SRS | Hệ thống Microservices / Event-Driven | Hệ thống Monolith / Modular Monolith |
   | :--- | :--- | :--- |
   | **Mục 2.4 & 2.5 (Kiến trúc & Component)** | Vẽ API Gateway, các Microservice độc lập, Message Broker (Kafka/RabbitMQ) và Database riêng cho từng service. | Vẽ tầng Presentation/API, các Domain Modules nội bộ, In-memory Event Bus / Background Queue và Shared Database. |
   | **Mục 2.8 (Bảng Sự kiện Domain)** | Các sự kiện phát qua Message Broker để đồng bộ bất đồng bộ liên service. | Các Internal Domain Events / Application Hooks kích hoạt tác vụ phụ trợ (gửi mail, ghi audit log, cập nhật cache). |
   | **Mục 4.1 (Nguyên tắc Thiết kế)** | Bounded Context & **Cô lập dữ liệu bắt buộc (Data Isolation)**: cấm gọi trực tiếp DB hoặc dùng chung Schema giữa các service. | **Phân tách Module (Modular Boundaries)**: tách biệt tầng Controller - Service - Repository, giao tiếp qua Public Interface của module. |
   | **Mục 5.3 & 5.5 (Dữ liệu & Giám sát)** | Strong vs. Eventual Consistency, Saga Pattern, Distributed Tracing (`Trace ID`) xuyên suốt các service. | ACID Database Transactions, Centralized Structured Logging (`Request ID`), Database Connection Pool & Query Tuning. |

5. **Trực quan hóa Sơ đồ ngay trong Markdown**:
   - Đối với 3 sơ đồ ở Chương 2 (**2.2 Sơ đồ ngữ cảnh**, **2.4 Kiến trúc logic mức cao**, **2.5 Sơ đồ thành phần**):
     - Nếu người dùng đã cung cấp sẵn file ảnh sơ đồ: Giữ nguyên cú pháp nhúng ảnh `![Tên sơ đồ](đường-dẫn-ảnh.png)` kèm đoạn văn mô tả ngữ cảnh phía trên.
     - Nếu chưa có file ảnh: Chủ động vẽ trực tiếp bằng **Mermaid.js** (`flowchart TB` hoặc `flowchart LR`) rõ ràng, phân nhóm `subgraph` theo tầng/miền nghiệp vụ và chú thích nhãn kết nối (REST API, JWT, Kafka Event, Webhook).

---

## 3. Bốn Chế Độ Hoạt Động (Operating Workflows)

Khi nhận yêu cầu từ người dùng, sau khi thực hiện bước **Phỏng vấn Khai thác Yêu cầu (Mục 1)**, Agent triển khai theo chế độ phù hợp trong 4 chế độ sau:

### Chế độ 1: Viết mới SRS toàn diện từ Ý tưởng / BRD (Greenfield Full SRS)
1. **Phân tích & Phỏng vấn**: Thực hiện phỏng vấn theo 4 nhánh ở Mục 1.2 để chốt tên dự án, mô hình kinh doanh, danh sách Actors, các phân hệ nghiệp vụ chính (Domains/Services) và các hệ thống đối tác bên thứ ba (Payment, Shipping, OAuth, Notification...).
2. **Thiết lập nền tảng Chương 1 & Chương 2**:
   - Xác định rõ `In-Scope` và `Out-of-Scope` ở mục 1.2 để khóa phạm vi.
   - Thiết kế 3 sơ đồ kiến trúc (2.2, 2.4, 2.5) bằng Mermaid.js.
   - Lập trước 3 bảng xương sống: **Configurations List (2.6)**, **Privileges List (2.7)** và **Event Message Table (2.8)**.
3. **Triển khai Chương 3 (Functional Requirements)**: Viết đặc tả chi tiết cho từng phân hệ theo quy chuẩn 2 lớp (xem Mục 5 của skill này), liên tục đối chiếu ngược lên bảng 2.6, 2.7, 2.8.
4. **Hoàn thiện Chương 4 & Chương 5**: Thiết lập nguyên tắc kỹ thuật (Data Isolation, OpenAPI, Secrets) và các bảng SLA hiệu năng, thông lượng, độ sẵn sàng, bảo mật, lưu trữ.

### Chế độ 2: Trích xuất & Cập nhật SRS từ Mã nguồn (Code-to-SRS / Reverse Engineering)
Khi người dùng yêu cầu viết hoặc cập nhật SRS dựa trên source code có sẵn trong workspace:
1. **Quét cấu trúc dự án & quy tắc kiến trúc**:
   - Đọc `AGENTS.md`, `README.md`, `docker-compose.yml`, `pom.xml` / `package.json` để nắm danh sách services, cổng giao tiếp và nguyên tắc bất biến của dự án.
   - Đặt câu hỏi ngắn gọn với BE/User (theo Mục 1) chỉ cho những quy tắc nghiệp vụ hoặc chỉ số SLA không thể suy ra trực tiếp từ code.
2. **Trích xuất tự động Chương 2 từ Code**:
   - Quét `application.yml`, `application.properties`, `.env.example`, các class `@ConfigurationProperties` -> Tổng hợp thành **Mục 2.6 (Danh mục Cấu hình)**.
   - Quét các enum `Role`, `Privilege`, annotation `@PreAuthorize`, `SecurityConfig` và header `X-User-Roles` -> Lập ma trận **Mục 2.7 (Danh sách Quyền hạn)**.
   - Quét các class Event/Command, Kafka `@KafkaListener`, `KafkaTemplate.send(...)`, topic names -> Lập bảng **Mục 2.8 (Bảng Thông điệp Sự kiện Domain)**.
3. **Trích xuất Chương 3 từ Code**:
   - Quét `Controller` (endpoints), `DTO` (các annotation `@NotNull`, `@NotBlank`, `@Min`, `@Pattern` để lấy danh sách trường bắt buộc và rule validate), `Entity` & Flyway migrations (`V1__...sql` để lấy trạng thái, ràng buộc khóa ngoại, soft delete), và `ServiceImpl` (để lấy luồng nghiệp vụ, công thức tính toán, Saga, Lock, Idempotency).
   - Chuyển hóa toàn bộ logic code sang văn phong đặc tả nghiệp vụ BA chuẩn mực (không paste code Java/TS thô vào SRS).

### Chế độ 3: Đặc tả Bổ sung Module / Tính năng lẻ (Incremental Feature SRS)
Khi người dùng yêu cầu viết đặc tả cho một tính năng hoặc microservice mới để ghép vào SRS hiện có:
1. Đọc tài liệu SRS hiện tại (nếu có) để giữ đúng số thứ tự phân cấp (`3.x.y.z`) và phong cách đặt tên; hỏi làm rõ các ràng buộc nghiệp vụ của tính năng mới với BE/User.
2. Viết phần đặc tả chức năng chi tiết cho module mới ở Chương 3.
3. **Bắt buộc xuất kèm phần cập nhật bổ sung (Delta Update)** cho:
   - Các tham số cấu hình mới cần thêm vào **Mục 2.6**.
   - Các mã quyền hạn mới cần thêm vào ma trận **Mục 2.7**.
   - Các sự kiện Domain mới cần thêm vào bảng **Mục 2.8**.
   - Các chỉ số SLA / Throughput mới cần thêm vào **Mục 5.1 / 5.8** (nếu có).

### Chế độ 4: Review, Chuẩn hóa & Việt hóa SRS (SRS Standardization & Audit)
Khi người dùng đưa một bản nháp SRS hoặc tài liệu tiếng Anh yêu cầu chuẩn hóa:
1. Chuẩn hóa toàn bộ bố cục về đúng khung 5 chương của [references/srs-template.md](./references/srs-template.md).
2. Dịch và chuẩn hóa thuật ngữ chuyên ngành Kỹ thuật Phần mềm sang tiếng Việt theo định dạng `Tiếng Việt (English Term)`.
3. Phát hiện các lỗ hổng nghiệp vụ (thiếu trạng thái `"No Data"`, thiếu điều kiện chặn xóa, thiếu Idempotency/Timeout/Concurrency, thiếu Audit Logs) và hỏi xác nhận với BE/User để bổ sung hoàn chỉnh.

---

## 4. Cấu Trúc Chuẩn 5 Chương Của Tài Liệu SRS

Mọi tài liệu SRS hoàn chỉnh phải tuân thủ cấu trúc gồm **Phần Đầu Tài Liệu (Front Matter)** và **5 Chương chính** dưới đây (chi tiết bộ khung Markdown xem tại [references/srs-template.md](./references/srs-template.md)):

### 4.1. Phần Đầu Tài Liệu (Front Matter)
1. **Tiêu đề & Phụ đề dự án**: Tên dự án in hoa và mô tả loại hình nền tảng (kèm tiếng Anh).
2. **Khối Metadata**: Tên dự án, Tài liệu, Phiên bản, Người lập tài liệu (`<Tên> (Business Analyst)`), Địa điểm & Thời gian, Lần sửa đổi gần nhất.
3. **Trang Phê Duyệt (Approval Page)**: Bảng 4 cột (`Vai trò`, `Chức danh`, `Chữ ký`, `Ngày`) cho 4 vị trí: *Người lập (Prepared by)*, *Người kiểm tra (Reviewed by)*, *Người hỗ trợ (Supported by - Tech Lead / Solution Architect)*, *Người phê duyệt (Approved by - PM / PO)*.
4. **Lịch Sử Thay Đổi (Revision History)**: Bảng 4 cột (`Ngày`, `Phiên bản`, `Tác giả`, `Mô tả thay đổi`).
5. **Mục Lục (Table of Contents)**: Danh sách liên kết neo (anchor links) phân cấp tới từng mục lớn và mục con.

### 4.2. Chương 1: Giới thiệu (Introduction)
- **1.1 Mục đích (Purpose)**: Nêu rõ tài liệu định nghĩa toàn bộ Functional & Non-functional Requirements, đóng vai trò là *nguồn thông tin chuẩn mực duy nhất (single source of truth)* cho đội ngũ Dev, QA và Stakeholders.
- **1.2 Phạm vi dự án (Scope of Project)**:
  - Mô tả ngắn gọn sứ mệnh kết nối các tác nhân của nền tảng.
  - **Các tính năng thuộc phạm vi phiên bản (In-Scope Features)**: Liệt kê các nhóm tính năng cốt lõi được cam kết bàn giao.
  - **Các tính năng nằm ngoài phạm vi phiên bản (Out-of-Scope Features)**: Nêu rõ những phần chưa làm hoặc tích hợp qua đối tác thứ ba (VD: tự vận hành đội xe 3PL, ERP/HR nội bộ, ứng dụng mobile riêng cho Seller ở Phase 1) để quản lý kỳ vọng.

### 4.3. Chương 2: Mô tả Tổng quan Hệ thống (Overall Description)
- **2.1 Bối cảnh nghiệp vụ (Business Context)**: Nêu bài toán thị trường, nỗi đau của người dùng và giá trị cốt lõi hệ thống mang lại.
- **2.2 Sơ đồ ngữ cảnh hệ thống (System Context Diagram)**: Đoạn văn dẫn nhập + Sơ đồ Mermaid (hoặc ảnh) thể hiện ranh giới giữa Hệ thống trung tâm với các Actors và External Systems (Payment Gateways, Shipping Carriers...).
- **2.3 Các tác nhân hệ thống (System Actors)**: Bảng 2 cột (`Tác nhân (Actor)`, `Mô tả vai trò`), chuẩn hóa tên song ngữ: **Buyer (Người mua)**, **Seller (Người bán)**, **Administrator (Quản trị viên - Admin)**, **Support Staff (Nhân viên Hỗ trợ)**, **External System (Hệ thống bên ngoài)**.
- **2.4 Kiến trúc Logic Mức cao (High-Level Logical Architecture)**: Đoạn mô tả phân tầng + Sơ đồ Mermaid thể hiện luồng từ Client (Web/Mobile) -> API Gateway -> Microservices/Modules -> Message Broker & Databases.
- **2.5 Sơ đồ Thành phần (Component Diagram)**: Đoạn mô tả + Sơ đồ Mermaid phân rã các module dịch vụ nghiệp vụ.
- **2.6 Danh mục Cấu hình Hệ thống (Configurations List)**:
  - Bảng 3 cột: `Tên cấu hình (Configuration Name)` | `Mô tả chi tiết` | `Giá trị mặc định`.
  - Phân nhóm ngầm theo mạch logic: Hệ thống & Bảo mật (`SYSTEM_ENV`, `SESSION_TIMEOUT_MINUTES`, `MAX_LOGIN_ATTEMPTS`, `ACCESS_TOKEN_EXPIRY_MINUTES`...) -> Danh mục & Sản phẩm -> Giỏ hàng, Checkout & Thanh toán -> Nghiệp vụ đặc thù (Đấu giá/Khuyến mãi) -> Kho & Vận chuyển -> Thông báo & Uy tín -> Cờ tính năng (`FEATURE_*_ENABLED`).
- **2.7 Danh sách Quyền hạn (Privileges List)**:
  - Bảng ma trận RBAC: `Mã quyền (Privilege)` | `Mô tả nghiệp vụ` | `Admin` | `Seller` | `Buyer` | `Support Staff` (đánh dấu `X` ở các cột được cấp quyền).
  - Quy ước đặt mã quyền: `<DOMAIN>.<ACTION>` hoặc `<DOMAIN>.<SUBDOMAIN>.<ACTION>` viết hoa toàn bộ (VD: `AUTH.LOGIN`, `USER.REPUTATION.ADJUST`, `ORDER.REFUND`, `SHIPPING.MANUAL_OVERRIDE`).
- **2.8 Bảng Thông điệp Sự kiện Domain (Event Message Table)**:
  - Bảng 2 cột: `Tên sự kiện (Event ID)` | `Mô tả ý nghĩa sự kiện`.
  - Quy ước đặt tên sự kiện: `PascalCase` ở dạng danh từ + động từ quá khứ (`UserRegistered`, `ProductStatusChanged`, `OrderPaid`, `BidPlaced`, `Outbid`, `InventoryReserved`, `ShipmentDelivered`, `MessageBrokerUnavailable`).

---

## 5. Quy Chuẩn Viết Chương 3: Đặc Tả Yêu Cầu Chức Năng (2 Lớp Nghiệp Vụ)

Chương 3 là trái tim của tài liệu SRS. Cấu trúc đánh số phân cấp chặt chẽ theo 3 đến 4 mức:
- **Mức 2 (`## 3.x`)**: Tên Dịch vụ / Phân hệ (VD: `## 3.1 Dịch vụ Người dùng (User Service)`).
- **Mức 3 (`### 3.x.y`)**: Tên Module / Nhóm chức năng (VD: `### 3.1.1 Quản lý Định danh & Truy cập (IAM)`).
- **Mức 4 (`#### 3.x.y.z` hoặc bullet `- **3.x.y.z ...**`)**: Tác vụ hoặc Quy tắc nghiệp vụ cụ thể.

Agent bắt buộc áp dụng **Phương pháp Đặc tả 2 Lớp** tùy theo bản chất của từng nhóm chức năng:

### 5.1. Lớp 1: Khung Đặc tả cho Tác vụ CRUD, Quản trị & Danh mục (Management & CRUD Specs)
Áp dụng cho các chức năng quản lý thực thể (Người dùng, Vai trò, Sản phẩm, Danh mục, Nhật ký sự kiện...). Mỗi nhóm CRUD phải bao phủ đủ 5 khía cạnh sau:

1. **Tạo mới (`Create <Entity>`)**:
   - Nêu rõ ai có quyền thực hiện.
   - Liệt kê **đầy đủ các trường thông tin bắt buộc** người dùng phải nhập.
   - Quy tắc kiểm tra tính hợp lệ (validate) và giới hạn cấu hình (VD: tối đa 5 ảnh theo `MAX_PRODUCT_IMAGES = 5`, độ sâu cây danh mục `CATEGORY_MAX_LEVEL = 3`, kiểm tra trùng mã).
   - Hành vi khi lỗi: Từ chối lưu và hiển thị thông báo lỗi chi tiết.
   - Hành vi khi hợp lệ: Lưu vào CSDL với trạng thái khởi tạo ban đầu (`DRAFT`/`ACTIVE`) và ghi nhận vào nhật ký kiểm toán (Audit Log).

2. **Cập nhật (`Update <Entity>`)**:
   - Liệt kê rõ **các trường được phép chỉnh sửa** (và các trường bị khóa bất biến nếu có).
   - Kiểm tra dữ liệu cập nhật; nếu thiếu trường bắt buộc hoặc sai định dạng thì hủy cập nhật và cảnh báo.
   - Khi thành công: Cập nhật CSDL, phản hồi cho người dùng và ghi Audit Log (ghi rõ trường bị thay đổi và định danh người thực hiện).

3. **Lấy danh sách & Tra cứu (`Get list of <Entities>`)**:
   - Nêu rõ đối tượng có thẩm quyền xem.
   - Khả năng hỗ trợ: Tìm kiếm theo từ khóa (Search), Lọc theo tiêu chí (Filter) và Sắp xếp (Sort).
   - **Thứ tự sắp xếp mặc định**: Bắt buộc chỉ rõ (VD: *Mặc định sắp xếp theo Họ và tên (A-Z)* hoặc *Mặc định hiển thị sự kiện mới nhất lên đầu theo thứ tự thời gian giảm dần*).
   - **Danh sách các trường hiển thị trên mỗi dòng bản ghi**.
   - **Xử lý trạng thái rỗng (Empty State)**: Bắt buộc ghi rõ: *"Nếu không có dữ liệu nào phù hợp, hệ thống hiển thị thông báo `"No Data"` thay vì để trống giao diện."*

4. **Xem chi tiết (`View <Entity>`)**:
   - Điều kiện hiển thị: Bản ghi phải đang tồn tại và chưa bị đánh dấu xóa.
   - Trường hợp không tìm thấy hoặc ở trạng thái không khả dụng: Thông báo lỗi tương ứng.

5. **Xóa (`Delete <Entity>`)**:
   - Nêu rõ ai có quyền xóa (Admin / Seller...).
   - **Kiểm tra điều kiện ràng buộc trước khi xóa**:
     - Bản ghi có tồn tại và chưa bị xóa trước đó không? (Nếu không tồn tại hoặc đã xóa -> cảnh báo và ngăn chặn).
     - Bản ghi có đang gắn với dữ liệu giao dịch khác không? Tham chiếu cấu hình chặn xóa (VD: `ALLOW_PRODUCT_DELETE_WITH_ORDER = false`, `ALLOW_CATEGORY_DELETE_WITH_PRODUCT = false`).
   - Cơ chế xóa: Xóa mềm (Soft Delete theo `USER_SOFT_DELETE = true`) hay xóa vật lý.
   - Ghi vết kiểm toán (Audit Log) ghi rõ danh tính người thực hiện.

---

### 5.2. Lớp 2: Khung Đặc tả cho Luồng Nghiệp vụ Phức tạp, Giao dịch & Máy Trạng thái (Transactional & State-Driven Specs)
Áp dụng cho các luồng nghiệp vụ lõi (Giỏ hàng, Checkout & Thanh toán, Đơn hàng, Đấu giá, Đặt giá thầu, Tất toán, Kho & Tồn kho, Vận chuyển, Điểm uy tín, Thông báo). Khi viết các mục này, Agent phải đặc tả sâu 6 yếu tố kỹ thuật - nghiệp vụ sau:

1. **Vòng đời & Máy trạng thái (Lifecycle & State Machine)**:
   - Liệt kê chuỗi trạng thái hợp lệ tuần tự, ví dụ:
     - Đơn hàng: `Created` -> `Paid` -> `Processing` -> `Shipped` -> `Delivered` -> `Completed` (hoặc `Cancelled` / `Refunded`).
     - Vận đơn chuẩn hóa: `Created`, `PickedUp`, `InTransit`, `Delivered`, `Failed`, `Returned`.
     - Phiên đấu giá: `Scheduled` -> `Active` -> `Ended` -> `Settled` (hoặc `Cancelled` / `Failed`).
   - Khẳng định quy tắc: Ngăn chặn tuyệt đối các bước nhảy trạng thái trái quy tắc nghiệp vụ; khóa bất biến cấu hình ngay khi phiên/giao dịch đã bắt đầu chạy.

2. **Công thức Tính toán & Bất biến Dữ liệu (Formulas & Data Invariants)**:
   - Viết rõ biểu thức toán học cho các quy tắc tính tồn kho, giá thầu, giá giỏ hàng:
     - Tồn kho theo cặp `SKU + Warehouse`: `Available = Total - Reserved - Unavailable`.
     - Giá thầu hợp lệ tối thiểu: `Bid Amount >= Current Price + Bid Increment` (`DEFAULT_BID_INCREMENT = 10`).
     - Snapshot dữ liệu: Đơn hàng phải lưu bản sao chép tức thời (snapshot) của thông tin sản phẩm và giá tại thời điểm mua, không bị biến đổi khi sản phẩm gốc cập nhật sau này.

3. **Ràng buộc Cấu hình, Thời hạn & Hình phạt Tự động (Timeouts, Thresholds & Penalties)**:
   - Gắn chặt mọi quy tắc kiểm tra với mã cấu hình ở Mục 2.6:
     - Ngưỡng điểm uy tín (`MIN_REPUTATION_TO_BID >= 40`, `MIN_REPUTATION_TO_SELL >= 50`).
     - Giới hạn số lượng (`MAX_ACTIVE_CART_PER_USER = 1`, `MAX_ACTIVE_AUCTIONS_PER_SELLER = 5`, `MAX_WAREHOUSES_PER_ORDER = 2`).
     - Cơ chế hết hạn tự động (Expiration/Timeout) và hành động bù trừ (Compensation/Saga):
       - Giữ tồn kho quá 30 phút (`INVENTORY_RESERVATION_TIMEOUT_MINUTES = 30`) -> Tự động hết hạn và giải phóng hàng về kho khả dụng.
       - Thắng đấu giá không thanh toán sau 24 giờ (`AUCTION_PAYMENT_DEADLINE_HOURS = 24`) -> Phát sự kiện `AuctionPaymentTimeout`, hủy tư cách thắng thầu và phạt trừ điểm uy tín (`PAYMENT_TIMEOUT_PENALTY_SCORE = 10`).
       - Đặt giá vào cửa sổ phút chót (`ANTI_SNIPING_ENABLED = true`) -> Tự động cộng thêm thời gian gia hạn (`ANTI_SNIPING_EXTENSION_MINUTES = 5`).

4. **Phân định Ranh giới Dịch vụ & Sự kiện Domain (Service Boundaries & Event Emission)**:
   - Nêu rõ dịch vụ nào sở hữu hành động và dịch vụ nào nhận ủy quyền qua sự kiện/API (VD: *Auction Service không tự tạo đơn hàng trực tiếp mà gửi dữ liệu tất toán sang Commerce Service để khởi tạo đơn hàng*; *Shipping Orchestration sử dụng mẫu thiết kế Carrier Adapter để đóng gói logic riêng của GHN/GHTK*).
   - Chỉ rõ thời điểm phát sự kiện Domain lên Message Bus (`AuctionCancelled`, `Outbid`, `AuctionWon`, `AuctionFailed`, `ShipmentPickedUp`...) và phản ứng của các dịch vụ lắng nghe.

5. **Xử lý Đồng thời, Nguyên tử & Lũy biến (Concurrency, Atomicity & Idempotency)**:
   - **Tính nguyên tử (Atomicity) & Chống xung đột (Concurrency)**: Đặt giá thầu và đặt giữ tồn kho phải xử lý nguyên tử, kết hợp In-Memory Caching (Redis) và Khóa lạc quan (Optimistic Locking) để chống bán vượt tồn kho (`ALLOW_OVERSELL = false`) và không làm mất lượt đặt giá ở tải cao.
   - **Tính lũy biến (Idempotency)**: Luôn có mục riêng khẳng định các tác vụ tạo đơn, callback thanh toán (`PAYMENT_IDEMPOTENCY_WINDOW_MINUTES = 60`), tất toán đấu giá, trừ/nhả kho và webhook vận chuyển đều xử lý lũy biến — gọi lặp nhiều lần không gây trừ tiền hoặc trừ kho trùng lặp.

6. **Khả năng Chịu lỗi & Sổ cái Bất biến (Fault Tolerance & Immutable Ledgers)**:
   - Chính sách Retry & Circuit Breaker khi gọi đối tác ngoài (`PAYMENT_RETRY_LIMIT = 3`, `SHIPPING_RETRY_LIMIT = 3`, `SHIPPING_API_TIMEOUT_MS = 3000`), phân biệt lỗi tạm thời (cho phép retry) và lỗi vĩnh viễn (không retry).
   - Yêu cầu lưu trữ bất biến (Immutable) đối với Sổ cái tồn kho (Inventory Ledger) và Nhật ký kiểm toán (Audit Logs) — tuyệt đối không cho phép sửa hoặc xóa bản ghi lịch sử.

---

## 6. Quy Chuẩn Chương 4 & Chương 5 (Yêu Cầu Triển Khai & Phi Chức Năng)

### 6.1. Chương 4: Các Yêu cầu Triển khai Khác (Other Requirements)
Bắt buộc bao gồm 3 mục chuẩn:
- **4.1 Nguyên tắc Thiết kế & Hiện thực Dịch vụ (General implementation principles)**:
  - Định nghĩa Bounded Context (DDD), chủ sở hữu dịch vụ (Service Ownership) và **Quy tắc cô lập dữ liệu bắt buộc (Data Isolation)** (không truy cập trực tiếp DB của service khác, không dùng chung Schema/Table, chỉ giao tiếp qua REST API hoặc Message Events).
- **4.2 Quản lý API & Giao ước Dữ liệu (API & Contract Management)**:
  - Đánh phiên bản trên URL (`/api/v1`, `/api/v2`), duy trì tương thích ngược (Backward compatibility), tài liệu hóa bằng **OpenAPI (Swagger)** (Endpoints, JSON Schema, Error Codes) và quy trình thẩm định trước khi thay đổi Contract (API schema & Event schema).
- **4.3 Quản lý Cấu hình & Thông tin Bảo mật (Configuration & Secrets Management)**:
  - Cấm tuyệt đối hardcode nhạy cảm (API keys, DB credentials, private keys) vào source code.
  - Phân tách biến môi trường (Environment Variables) cho cấu hình thường và **HashiCorp Vault / Cloud Secrets Manager** cho thông tin bí mật.

### 6.2. Chương 5: Yêu cầu Phi chức năng (Non-functional Requirements)
Bắt buộc bao gồm đầy đủ 8 mục chuẩn với các bảng chỉ số định lượng (sử dụng ký hiệu toán học LaTeX `$\le$`, `$\ge$`, `$p95, p99$`):
- **5.1 Hiệu năng & Khả năng Mở rộng (Performance & Scalability)**:
  - Bảng **Tiêu chuẩn Thời gian Phản hồi (SLA Response Time)** theo từng nghiệp vụ chính.
  - Khối cảnh báo `> [!IMPORTANT]` chỉ định rõ **Luồng nghiệp vụ tối quan trọng (Critical Path)** có yêu cầu độ trễ thấp nhất.
  - Bảng **Chỉ tiêu Thông lượng Tối thiểu (Minimum Throughput Targets)** theo từng Service (`requests/giây` hoặc `lượt/phút`).
  - Yêu cầu mở rộng chiều ngang (Horizontal Scaling), Stateless APIs và kiểm soát truy cập đồng thời (In-memory Caching, Optimistic Locking).
- **5.2 Độ Sẵn sàng & Độ Tin cậy (Availability & Reliability)**:
  - Bảng **Cam kết Độ sẵn sàng (Uptime SLA)** ($\ge 99.9\%$ cho Core Platform, $\ge 99.5\%$ cho dịch vụ đặc thù...).
  - Cô lập lỗi dịch vụ phụ trợ (Notification lỗi không làm sập luồng Order), cơ chế retry & đối soát thủ công khi Payment timeout, cam kết không mất dữ liệu giao dịch khi khởi động lại.
- **5.3 Tính Nhất quán & Toàn vẹn Dữ liệu (Data Consistency & Integrity)**:
  - Phân định rõ dữ liệu nào áp dụng **Tính nhất quán mạnh (Strong Consistency)** và dữ liệu nào áp dụng **Nhất quán sau (Eventual Consistency)**, cùng yêu cầu bắt buộc về tính **Lũy biến (Idempotent)**.
- **5.4 Bảo mật & Tuân thủ Tiêu chuẩn (Security & Compliance)**:
  - OAuth 2.0 & JWT (Access/Refresh Token), RBAC, mã hóa đường truyền HTTPS/TLS 1.3 & mTLS nội bộ, mã hóa lưu trữ (Encryption at Rest, băm mật khẩu BCrypt/Argon2), tuân thủ **PCI-DSS** (cấm lưu số thẻ đầy đủ & CVV/CVC, che mờ dữ liệu nhạy cảm trong log - masked data).
- **5.5 Khả năng Quan sát & Giám sát Hệ thống (Observability & Monitoring)**:
  - Ghi log tập trung (Centralized Logging) chuẩn JSON gồm `Timestamp`, `Trace ID`, masked `User ID`; các mức `INFO`, `WARN`, `ERROR`.
  - Thu thập Metrics (Request Rate, Error Rate, độ trễ phân vị $p95, p99$), ngưỡng kích hoạt cảnh báo tự động (Alerts khi Error Rate $> 1\%$ hoặc vi phạm SLA) và Truy vết phân tán (**Distributed Tracing**) trên các luồng trọng yếu.
- **5.6 Khả năng Bảo trì & Mở rộng Mã nguồn (Maintainability & Extensibility)**:
  - Clean Architecture & DDD, tỷ lệ bao phủ kiểm thử tự động (**Unit Test Coverage $\ge 70\%$**), chuẩn RESTful versioning và tách biệt cấu hình theo môi trường (`DEV`, `STAGING`, `PROD`).
- **5.7 Quy trình Triển khai & Vận hành (Deployment & Operations)**:
  - Đường ống CI/CD tự động (Build, Test, Deploy), chiến lược phát hành **Zero-Downtime Deployment** (Rolling / Blue-Green), đóng gói **Docker** & điều phối **Kubernetes (K8s)**, sao lưu CSDL hàng ngày (Daily Backup) và cam kết **RTO** ($\le 4\text{ giờ}$) / **RPO** ($\le 15\text{ phút}$).
- **5.8 Lưu trữ Dữ liệu & Kiểm toán (Data Retention & Auditing)**:
  - Bảng **Thời gian Lưu trữ Tối thiểu (Retention Period)** cho từng loại dữ liệu (Orders $\ge 5\text{ năm}$, History $\ge 3\text{ năm}$, System Logs $\ge 90\text{ ngày}$...).
  - Quy định sinh tự động và bảo vệ bất biến (**Immutable Audit Logs**) cho mọi thao tác nhạy cảm.

---

## 7. Ví Dụ Minh Họa Phong Cách Viết Đặc Tả (Standard Examples)

### 7.1. Ví dụ Đặc tả Lớp 1 (Tác vụ CRUD & Danh sách)
```markdown
- **3.1.1.1.3 Lấy danh sách Người dùng (Get list of Users):**
  - Cho phép người dùng có thẩm quyền (`USER.LIST`) xem danh sách người dùng trong hệ thống.
  - Cung cấp các tính năng tìm kiếm theo từ khóa, lọc (filter) theo tiêu chí lựa chọn và sắp xếp (sort) theo các cột dữ liệu khả dụng.
  - Mặc định, danh sách được sắp xếp theo Họ và tên (A-Z).
  - Thông tin hiển thị trên mỗi dòng gồm: Họ tên, Email, Số điện thoại, Số CCCD, Giới tính, Địa chỉ và Ngày sinh.
  - Nếu không có dữ liệu người dùng nào phù hợp, hệ thống hiển thị thông báo "No Data" thay vì để trống giao diện.
```

### 7.2. Ví dụ Đặc tả Lớp 2 (Luồng Giao dịch, Sự kiện & Ràng buộc Cấu hình)
```markdown
- **3.5.3.5 Thực thi Thời hạn Thanh toán Đấu giá (Payment Deadline Enforcement):**
  - Người thắng đấu giá bắt buộc phải thanh toán đơn hàng trong vòng 24 giờ (`AUCTION_PAYMENT_DEADLINE_HOURS = 24`).
  - Nếu quá thời hạn mà chưa thanh toán, hệ thống phát sự kiện `AuctionPaymentTimeout`, hủy tư cách thắng thầu, giải phóng hàng tồn kho đang giữ (`InventoryReleased`) và tự động phạt trừ 10 điểm uy tín của người mua này (`PAYMENT_TIMEOUT_PENALTY_SCORE = 10`).
```

---

## 8. Checklist Kiểm Tra Chất Lượng Trước Khi Bàn Giao (BA Quality Gate)

Trước khi hoàn tất tài liệu SRS, Agent phải tự rà soát qua danh sách kiểm tra sau:

- [ ] **Đã thực hiện Phỏng vấn Khai thác Yêu cầu (Smart BA Discovery)**: Đã khảo sát trước codebase/tài liệu hiện có và đặt câu hỏi làm rõ (kèm phương án `(Recommended)`) cho các quyết định phạm vi, kiến trúc, quy tắc nghiệp vụ và SLA chưa tường minh.
- [ ] **Đầy đủ cấu trúc 5 chương & Front Matter**: Có Trang phê duyệt, Lịch sử thay đổi, Mục lục liên kết neo, Chương 1 -> Chương 5 đúng thứ tự mẫu tại [references/srs-template.md](./references/srs-template.md).
- [ ] **Đồng bộ Tham chiếu Chéo 100%**:
  - Mọi hằng số cấu hình trong Chương 3 đều đã được khai báo trong bảng **Mục 2.6 (Configurations List)**.
  - Mọi tác vụ phân quyền trong Chương 3 đều có mã quyền tương ứng trong ma trận **Mục 2.7 (Privileges List)**.
  - Mọi sự kiện phát ra/lắng nghe trong Chương 3 đều có tên trong bảng **Mục 2.8 (Event Message Table)**.
- [ ] **Chuẩn hóa Sơ đồ Chương 2**: Các mục 2.2, 2.4, 2.5 đã được vẽ bằng sơ đồ Mermaid.js hợp lệ (hoặc giữ nguyên link ảnh nếu có sẵn) kèm đoạn văn mô tả ngữ cảnh.
- [ ] **Độ sâu Chương 3 (Functional Requirements)**:
  - Các chức năng CRUD/Danh sách đã nêu đủ: trường bắt buộc, validate, sort mặc định, các cột hiển thị, trạng thái `"No Data"`, điều kiện chặn xóa/xóa mềm và ghi Audit Log.
  - Các luồng giao dịch phức tạp đã nêu đủ: vòng đời trạng thái tuần tự, công thức tính toán, timeout/hết hạn, xử lý đồng thời (Concurrency), tính lũy biến (Idempotency) và ranh giới dịch vụ.
- [ ] **Định lượng Chương 5 (Non-Functional Requirements)**: Đầy đủ các bảng SLA Response Time, Callout `[!IMPORTANT]` cho Critical Path, Throughput Targets, Uptime SLA, RTO/RPO và Data Retention Period.
- [ ] **Thuật ngữ & Định dạng**: Song ngữ `Tiếng Việt (English Term)` nhất quán; các mã kỹ thuật bọc trong backticks; công thức và ký hiệu so sánh dùng chuẩn LaTeX (`$\le$`, `$\ge$`).
