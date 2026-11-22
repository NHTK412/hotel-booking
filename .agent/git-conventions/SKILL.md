---
name: git-conventions
description: Quy tắc Git & Pull Request chuẩn hóa: tự động tra cứu Task ID và ngữ cảnh từ MCP Jira (hoặc fallback về SPRINT-<N> hiện tại), đặt tên nhánh, viết commit theo Conventional Commits, đặt tiêu đề PR và template mô tả PR chi tiết cho Backend (BE) và Frontend (FE). Kích hoạt khi người dùng cần tạo nhánh, viết commit, soạn PR hoặc chuẩn hóa quy trình làm việc nhóm.
---

# Quy Tắc Git & Pull Request (Git Workflow & Conventions)

Tài liệu này định nghĩa quy chuẩn làm việc với Git và Pull Request (PR) nhằm đảm bảo tính nhất quán, dễ theo dõi lịch sử và tối ưu quy trình review code trong dự án.

---

## 0. Quy tắc Xác định Task ID & Đồng bộ Ngữ cảnh từ MCP Jira

Trước khi đặt tên nhánh, viết commit message hoặc soạn mô tả Pull Request, Agent cần xác định mã định danh công việc (`<task-id>`) và ngữ cảnh nghiệp vụ theo thứ tự ưu tiên sau:

### 0.1. Thứ tự ưu tiên xác định `<task-id>`

1. **Ưu tiên 1 — Người dùng chỉ định trực tiếp hoặc trích xuất từ nhánh hiện tại**:
   - Nếu người dùng đã cung cấp sẵn mã task (VD: `SCRUM-148`) hoặc nhánh Git đang đứng đã mang mã task cụ thể (`<type>/SCRUM-148/...`), sử dụng mã task đó.
   - Nếu có kết nối **MCP Jira**, truy vấn chi tiết issue theo mã task này để lấy `Summary`, `Description` và `Acceptance Criteria` phục vụ cho việc sinh mô tả commit/PR.

2. **Ưu tiên 2 — Tự động tra cứu trên MCP Jira (khi có kết nối)**:
   - Khi người dùng chưa chỉ định mã task, Agent kiểm tra kết nối tới **MCP Jira**.
   - Truy vấn danh sách các task/issue trong **Active Sprint** của dự án trên Jira.
   - Đối chiếu yêu cầu của người dùng hoặc nội dung thay đổi mã nguồn (`git status`, `git diff`) với `Summary` và `Description` của các issue trong Active Sprint để chọn ra task khớp nhất (VD: `SCRUM-148`).

3. **Ưu tiên 3 — Fallback về Sprint hiện tại (`SPRINT-<N>`)**:
   - Áp dụng khi:
     - Đã tra cứu trên MCP Jira nhưng công việc mang tính chất chung (tích hợp, sửa lỗi tổng hợp, dọn dẹp tài liệu...) không khớp với bất kỳ task lẻ nào; **hoặc**
     - Không có kết nối tới MCP Jira / truy vấn Jira không khả dụng.
   - Định dạng chuẩn bắt buộc: `SPRINT-<N>` (viết hoa, nối bằng dấu gạch ngang, VD: `SPRINT-4`).
   - **Cách xác định số Sprint `<N>`**:
     1. Nếu kết nối được MCP Jira: Lấy số thứ tự từ **Active Sprint** trên Jira Board (VD: `"Sprint 4"` $\rightarrow$ `SPRINT-4`).
     2. Nếu không kết nối được MCP Jira: Suy luận từ tên nhánh hiện tại hoặc các nhánh/commit gần nhất trong lịch sử Git (`git branch -a`, `git log -n 10 --oneline`).

### 0.2. Cách khai thác dữ liệu Task từ MCP Jira cho từng tác vụ

| Tác vụ | Dữ liệu Jira sử dụng | Quy tắc áp dụng |
| :--- | :--- | :--- |
| **Tạo nhánh (Branch)** | `Key` (hoặc `SPRINT-<N>`) + `Summary` | Dùng `Key` làm `<task-id>`; dịch/tóm tắt `Summary` của Jira task sang tiếng Anh không dấu dạng `kebab-case` làm `<short-description-kebab-case>`. |
| **Viết Commit** | `Key` (hoặc `SPRINT-<N>`) + `Summary` / `Description` | Kết hợp ngữ cảnh nghiệp vụ từ Jira task với `git diff` thực tế để viết câu mô tả commit sát mục tiêu công việc, gắn `#<task-id>` ở cuối dòng. |
| **Soạn Pull Request** | `Key` + `Summary` + `Description` + `Acceptance Criteria` | Điền `#<task-id>` vào tiêu đề và mục *Task ID*; tóm tắt mục tiêu nghiệp vụ từ `Summary`/`Description` vào phần *Tổng quan mục đích*; ánh xạ `Acceptance Criteria` vào phần *Chi tiết tính năng* và *Self-Checklist*. |

> **Nguyên tắc Xác nhận**: Trước khi chạy lệnh tạo nhánh hoặc `git commit`, Agent **luôn hiển thị mã `<task-id>` (hoặc `SPRINT-<N>`) đề xuất** kèm tên nhánh / nội dung commit dự kiến để người dùng xác nhận.

---

## 1. Quy tắc đặt tên Nhánh (Branch Naming)

### 1.1. Cú pháp chuẩn
```text
<type>/<task-id>/<short-description-kebab-case>
```

- `<type>`: Loại nhánh (chữ thường).
- `<task-id>`: Mã định danh công việc lấy từ MCP Jira (VD: `SCRUM-27`, `SCRUM-148`). Trường hợp không có task cụ thể trên Jira hoặc không kết nối được MCP Jira, bắt buộc fallback về mã Sprint hiện tại theo định dạng `SPRINT-<N>` (VD: `SPRINT-3`, `SPRINT-4`).
- `<short-description-kebab-case>`: Tóm tắt nội dung bằng tiếng Anh không dấu (ưu tiên chuyển ngữ từ `Summary` của Jira task nếu có), nối nhau bằng dấu gạch ngang `-`, độ dài ngắn gọn (dưới 40 ký tự).

### 1.2. Phân loại nhánh (`<type>`)

| Type | Ý nghĩa & Mục đích sử dụng | Nhánh base xuất phát | Nhánh merge vào |
| :--- | :--- | :--- | :--- |
| **`feature/`** | Phát triển tính năng mới | `develop` / `main` | `develop` |
| **`bugfix/`** | Sửa lỗi trong quá trình phát triển (sprint, dev, staging) | `develop` / `staging` | `develop` / `staging` |
| **`hotfix/`** | Sửa lỗi khẩn cấp trực tiếp từ môi trường production | `main` / `master` | `main` và `develop` |
| **`refactor/`** | Tái cấu trúc mã nguồn (không làm thay đổi logic/tính năng) | `develop` | `develop` |
| **`chore/`** | Cấu hình CI/CD, nâng cấp thư viện, cài đặt ban đầu | `develop` | `develop` |
| **`docs/`** | Cập nhật tài liệu, README, Swagger, API docs | `develop` | `develop` |
| **`test/`** | Bổ sung hoặc cập nhật bộ unit test, integration test | `develop` | `develop` |
| **`perf/`** | Tối ưu hóa hiệu năng, query database, caching | `develop` | `develop` |

### 1.3. Ví dụ hợp lệ:
- Khi khớp với task trên MCP Jira:
  - `feature/SCRUM-27/login-endpoint`
  - `bugfix/SCRUM-35/jwt-token-expiration-error`
  - `refactor/SCRUM-12/auth-middleware-structure`
  - `hotfix/SCRUM-99/fix-payment-callback`
- Khi không có task riêng lẻ phù hợp (fallback theo Sprint hiện tại):
  - `feature/SPRINT-4/integration-and-completion`
  - `docs/SPRINT-4/javadoc-vi`
  - `test/SPRINT-3/integration-and-completion`

---

## 2. Quy tắc viết Commit (Commit Message)

### 2.1. Cú pháp chuẩn (Conventional Commits)
```text
<type>(<scope>): <Mô tả ngắn gọn> #<task-id>
```

> **Lưu ý:**
> - `<type>` viết chữ thường (`feat`, `fix`, `refactor`...).
> - `<scope>` viết chữ thường, biểu thị module hoặc tầng kiến trúc bị ảnh hưởng (`auth`, `user`, `order`, `auction`, `dto`, `ui`...).
> - Phần mô tả kết hợp giữa `git diff` thực tế và ngữ cảnh nghiệp vụ của task trên MCP Jira (nếu có), không kết thúc bằng dấu chấm (`.`).
> - Mã `#<task-id>` bắt buộc đặt ở cuối dòng:
>   - Dùng mã issue từ MCP Jira (VD: `#SCRUM-27`, `#SCRUM-148`) nếu commit thuộc một task cụ thể để Jira/Git tự động liên kết.
>   - Nếu không có task nào trên Jira phù hợp (hoặc không có kết nối MCP Jira), bắt buộc đánh theo Sprint hiện tại: `#SPRINT-<N>` (VD: `#SPRINT-4`).

### 2.2. Phân loại Commit (`<type>`)

| Type | Ý nghĩa | Ví dụ |
| :--- | :--- | :--- |
| **`feat`** | Thêm tính năng, API, màn hình, UI component mới | `feat(auth): Thêm API đăng nhập bằng Google #SCRUM-27` |
| **`fix`** | Sửa lỗi, fix bug phát sinh | `fix(jwt): Sửa lỗi không giải mã được token hết hạn #SCRUM-35` |
| **`refactor`**| Tái cấu trúc mã nguồn (không đổi logic bên ngoài) | `refactor(user): Tách tầng service xử lý validate dữ liệu #SCRUM-12` |
| **`perf`** | Cải thiện hiệu năng xử lý, query DB, caching | `perf(order): Tối ưu index query danh sách đơn hàng #SCRUM-44` |
| **`docs`** | Thêm hoặc sửa tài liệu, chú thích, Swagger | `docs(javadoc): Bổ sung Javadoc tiếng Việt cho catalog-service #SPRINT-4` |
| **`style`** | Định dạng code, dấu cách, xoá code thừa (không đổi logic) | `style(auth): Format code và chỉnh sửa ESLint warning #SPRINT-4` |
| **`test`** | Thêm, sửa unit test, integration test, mock data | `test(auth): Viết unit test cho logic verify mật khẩu #SCRUM-27` |
| **`chore`** | Cài đặt package, config build/webpack/docker | `chore(deps): Cập nhật thư viện bcrypt lên bản mới nhất #SPRINT-4` |
| **`revert`**| Revert lại commit trước đó do lỗi | `revert(auth): Revert commit 3a4f1bc do lỗi timeout #SCRUM-27` |

### 2.3. Ví dụ Commit chuẩn:
```bash
# Khi xác định được Task ID từ MCP Jira:
feat(auth): Thiết kế DTO gửi biểu mẫu đăng nhập #SCRUM-27
fix(jwt): Sửa lỗi không giải mã được token hết hạn #SCRUM-35
refactor(user): Tách riêng tầng service xử lý validate dữ liệu #SCRUM-12
test(auth): Viết unit test cho logic verify mật khẩu #SCRUM-27

# Khi không có Task Jira phù hợp (fallback về Sprint hiện tại):
chore(auction): Cập nhật cấu hình anti-sniping soft-close 30s cho bản demo #SPRINT-4
docs(catalog): Chuẩn hóa tài liệu Javadoc tiếng Việt cho tầng service #SPRINT-4
```

---

## 3. Quy tắc Pull Request (PR)

### 3.1. Tiêu đề Pull Request

#### Cú pháp:
```text
[<TAG>] <Mô tả tổng quan bằng Tiếng Việt> #<TASK-ID>
```

#### Bảng Tag PR:
| Tag PR | Ý nghĩa | Tương ứng nhánh / commit |
| :--- | :--- | :--- |
| **`[FEAT]`** | Triển khai tính năng mới | `feature/`, `feat` |
| **`[FIX]`** | Sửa lỗi (Bugfix / Hotfix) | `bugfix/`, `hotfix/`, `fix` |
| **`[REFACTOR]`** | Tái cấu trúc mã nguồn | `refactor/`, `refactor` |
| **`[PERF]`** | Tối ưu hóa hiệu năng | `perf/`, `perf` |
| **`[DOCS]`** | Cập nhật tài liệu kỹ thuật | `docs/`, `docs` |
| **`[CHORE]`** | Cấu hình, cài đặt phụ thuộc, CI/CD | `chore/`, `chore` |

#### Ví dụ tiêu đề PR:
- `[FEAT] Triển khai API đăng nhập và cấp phát JWT token #SCRUM-27`
- `[FIX] Sửa lỗi văng app khi token hết hạn #SCRUM-35`
- `[REFACTOR] Tái cấu trúc middleware xác thực người dùng #SCRUM-12`
- `[PERF] Thêm bộ nhớ đệm Redis cho trang chủ #SCRUM-88`

---

### 3.2. Tiêu chí kiểm tra trước khi gửi PR (Self-Checklist)
- [ ] Code tuân thủ coding style và convention của dự án.
- [ ] Đã chạy test local và pass 100%.
- [ ] Không có conflict với nhánh đích (`main`/`develop`).
- [ ] Đã loại bỏ code debug thừa (`console.log`, `print`, `dump`, `debugger`).
- [ ] Không commit file cấu hình nhạy cảm (`.env`, credentials, secrets).

---

### 3.3. Mẫu Mô tả Pull Request (PR Descriptions)

#### 3.3.1. Template dành cho Backend (BE)

````markdown
## 1. Tổng quan mục đích
* **Task ID**: #SCRUM-XX
* **Loại thay đổi**: [Feature | Bugfix | Refactor | Perf | Chore]
* **Tóm tắt**: Mô tả ngắn gọn mục tiêu của PR, nghiệp vụ được giải quyết và phạm vi thay đổi chính.

---

## 2. Chi tiết các tính năng và thay đổi chính

### 2.1. [Tên chức năng / nghiệp vụ]
* Mô tả các chức năng hoặc luồng xử lý được triển khai.
* Các thành phần liên quan: Controller, Service, Repository, Entity/Model, DTO, Event/Listener.
* Quy tắc nghiệp vụ, điều kiện xử lý dữ liệu và các trạng thái liên quan.

### 2.2. [Tên chức năng / nghiệp vụ]
* Liệt kê các thay đổi kỹ thuật hoặc tích hợp dịch vụ bên thứ ba (nếu có).
* Xử lý dữ liệu nền (Queue, Cronjob, Cache).
* Xử lý các trường hợp ngoại lệ (Exceptions & Error Codes).

---

## 3. Danh sách API thay đổi

### 3.1. API mới
| Method | Endpoint | Mô tả | Quyền truy cập (Auth/Role) |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/v1/auth/login` | Đăng nhập tài khoản | Public |
| `GET` | `/api/v1/users/me` | Lấy thông tin cá nhân | Bearer Token |

### 3.2. API cập nhật
| Method | Endpoint | Nội dung thay đổi (Param/Body/Response) | Breaking Change? |
| :--- | :--- | :--- | :--- |
| `PATCH` | `/api/v1/orders/{id}` | Bổ sung trường `cancel_reason` | Không |

### 3.3. API bị xóa / Deprecated
* Liệt kê API không còn sử dụng hoặc đánh dấu deprecated kèm lý do.

---

## 4. Thay đổi Database & Migration (nếu có)
* **Bảng mới / Cột mới**: Liệt kê các table/column/index thêm mới.
* **Lưu ý triển khai**: Có cần chạy migration script, seed data hay backfill dữ liệu cũ không?

---

## 5. Danh sách kiểm tra (Self-Checklist)
- [ ] Đã test thành công các kịch bản chính (Happy cases).
- [ ] Đã kiểm tra các trường hợp biên và ngoại lệ (Edge cases / Validation fail).
- [ ] Đã kiểm tra phân quyền & xác thực API (401, 403).
- [ ] Không có log debug thừa hoặc lộ dữ liệu nhạy cảm.
````

---

#### 3.3.2. Template dành cho Frontend (FE)

````markdown
## 1. Tổng quan mục đích
* **Task ID**: #SCRUM-XX
* **Loại thay đổi**: [Feature | Bugfix | UI/UX | Refactor]
* **Tóm tắt**: Mô tả mục tiêu của PR, màn hình/chức năng được triển khai và vấn đề được giải quyết.

---

## 2. Chi tiết các tính năng và thay đổi chính

### 2.1. Giao diện người dùng (UI/UX)
* Xây dựng hoặc cập nhật màn hình, layout, component mới.
* Bổ sung form, bảng dữ liệu, modal, notification, dialog.
* Hỗ trợ Responsive (Mobile / Tablet / Desktop).
* Xử lý đầy đủ các trạng thái UI: **Loading state**, **Empty state**, **Error state**, **Success feedback**.

### 2.2. Tích hợp API (API Integration)
* Các API đã tích hợp từ Backend (kèm endpoint).
* Xử lý request payload, response data mapping.
* Xử lý bắt lỗi mạng, timeout, validate server-side và toast message hiển thị tương ứng.

### 2.3. Xử lý nghiệp vụ & Validate (Business Logic)
* Triển khai các luồng thao tác của người dùng (User flows).
* Validate dữ liệu form đầu vào (Regex, required, length, format...).
* Đồng bộ dữ liệu giao diện ngay sau khi thao tác thành công (Optimistic update hoặc Refetch).
* Xử lý hiển thị component và action theo phân quyền người dùng (Role-based UI).

### 2.4. Điều hướng & Quản lý trạng thái (State & Routing)
* Cập nhật Route, Guard / Protected Route nếu có.
* Quản lý State: Form state, Filter/Pagination state, Global state (Redux/Zustand/Pinia).

---

## 3. Ảnh chụp / Video minh họa (Screenshots / Demo)
*(Đính kèm ảnh chụp màn hình, GIF hoặc video kiểm thử thực tế)*

| Trạng thái | Ảnh chụp màn hình / Video |
| :--- | :--- |
| **Giao diện Desktop** | *[Chèn ảnh/link]* |
| **Giao diện Mobile** | *[Chèn ảnh/link]* |
| **Trạng thái Loading / Error** | *[Chèn ảnh/link]* |

---

## 4. Danh sách kiểm tra (Self-Checklist)
- [ ] Giao diện hiển thị đúng thiết kế (Figma/UI specs).
- [ ] Đã kiểm tra responsive trên cả Desktop và Mobile.
- [ ] Form validate chính xác và hiển thị lỗi thân thiện.
- [ ] Đã xử lý đầy đủ loading indicator và không bị giật layout.
- [ ] Đã xóa bỏ các đoạn `console.log` và test code thừa.
````

---

## 4. Hướng dẫn thực thi cho Agent (Agent Guidelines)

Khi người dùng yêu cầu tạo nhánh, viết commit hoặc soạn Pull Request, Agent thực hiện theo quy trình chuẩn sau:

1. **Bước 1 — Tra cứu & Xác định `<task-id>` cùng Ngữ cảnh (MCP Jira $\rightarrow$ Fallback `SPRINT-<N>`)**:
   - Kiểm tra xem người dùng đã chỉ định mã task hay tên nhánh hiện tại đã có mã task (VD: `SCRUM-148`) chưa.
   - Nếu chưa có và môi trường có kết nối **MCP Jira**:
     - Truy vấn danh sách các issue trong **Active Sprint** trên Jira.
     - Đối chiếu yêu cầu của người dùng hoặc thay đổi trong code (`git status`, `git diff`) với `Summary` và `Description` của các issue để tìm task khớp nhất.
     - Lấy thêm thông tin chi tiết của task (`Summary`, `Description`, `Acceptance Criteria`).
   - Nếu không có task nào trên Jira phù hợp (hoặc không kết nối được MCP Jira):
     - Fallback sang mã Sprint hiện tại theo chuẩn `SPRINT-<N>` (VD: `SPRINT-4`).
     - Ưu tiên lấy số `<N>` từ Active Sprint trên MCP Jira; nếu không kết nối được Jira thì suy luận từ nhánh hoặc commit gần nhất trong lịch sử Git (`git branch -a`, `git log -n 10 --oneline`).

2. **Bước 2 — Đặt tên nhánh (Branch Naming)**:
   - Sinh tên nhánh theo cú pháp `<type>/<task-id>/<short-description-kebab-case>`.
   - Nếu lấy được task từ MCP Jira: chuyển ngữ/tóm tắt `Summary` của Jira task sang tiếng Anh không dấu dạng `kebab-case`.
   - Nếu fallback về `SPRINT-<N>`: tóm tắt mục tiêu công việc sang tiếng Anh `kebab-case` (VD: `feature/SPRINT-4/order-checkout-flow`).
   - Hiển thị tên nhánh và mã `<task-id>` đề xuất để người dùng xác nhận.

3. **Bước 3 — Viết Commit Message**:
   - Phân tích `git status` / `git diff` kết hợp với `Summary` và `Description` của task trên MCP Jira (nếu có) để xác định `<type>`, `<scope>` và viết mô tả sát nghiệp vụ.
   - Luôn gắn `#<task-id>` (VD: `#SCRUM-148` hoặc `#SPRINT-4`) ở cuối câu commit theo mục 2.
   - Liệt kê các file thay đổi, trình bày commit message kèm mã task dự kiến và chờ người dùng xác nhận trước khi chạy lệnh `git commit`.

4. **Bước 4 — Tạo Pull Request (PR Title & Description)**:
   - Đặt tiêu đề PR theo cú pháp `[<TAG>] <Mô tả tổng quan bằng Tiếng Việt> #<TASK-ID>` (trong đó `<TASK-ID>` là mã Jira task hoặc `SPRINT-<N>`).
   - Xác định phạm vi là Backend hay Frontend để áp dụng đúng template mục 3.3.
   - Nếu có thông tin từ MCP Jira: tự động điền mã task, tóm tắt mục tiêu nghiệp vụ từ `Summary`/`Description` của Jira task vào mục *1. Tổng quan mục đích*, và đối chiếu `Acceptance Criteria` của Jira task vào mục *2. Chi tiết các tính năng* cùng *Self-Checklist*.
