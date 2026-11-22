---
name: javadoc-vi
description: Quy chuẩn viết và Việt hóa Javadoc cho các Java microservice trong Project Nexus (src/main/java). Kích hoạt khi cần bổ sung Javadoc cho hàm public/protected, Việt hóa Javadoc tiếng Anh sẵn có, hoặc chuẩn hóa tài liệu mã nguồn Java mà không làm thay đổi logic.
---

# Quy Chuẩn Viết & Việt Hóa Javadoc (`javadoc-vi`)

Tài liệu này quy định chuẩn mực viết mới và Việt hóa Javadoc trong mã nguồn Java của Project Nexus, giúp tài liệu mã nguồn đồng nhất, dễ đọc và an toàn tuyệt đối khi biên dịch.

---

## 1. Quy Tắc Bắt Buộc (Tuyệt Đối)

> **CẢNH BÁO AN TOÀN:**
> **Tuyệt đối không tự ý sửa đổi logic code, signature, annotation, import hay luồng xử lý** — chỉ thêm mới khối Javadoc (`/** ... */`) hoặc sửa/Việt hóa nội dung Javadoc hiện có.
>
> - Không thêm, xóa, sắp xếp lại `import` hoặc thay đổi tên biến, kiểu trả về, tham số, modifier.
> - Không sửa hoặc xóa các annotation (`@Override`, `@Transactional`, `@GetMapping`, `@PreAuthorize`, v.v.).
> - Không xóa hoặc làm sai lệch các comment nội bộ không phải Javadoc (`// ...`, `/* ... */`), đặc biệt là các comment giải thích nghiệp vụ hoặc `TODO`/`FIXME` quan trọng.

---

## 2. Phạm Vi Áp Dụng

- **Đường dẫn áp dụng**: Toàn bộ mã nguồn Java trong `src/main/java` của các Java microservice (`catalog-service`, `commerce-service`, `fulfilment-service`, `user-service`, `auction-service`, `api-gateway`, v.v.).
- **Đối tượng cần viết/Việt hóa**:
  - Tất cả các phương thức `public` và `protected` chưa có Javadoc hoặc đang có Javadoc bằng tiếng Anh / chưa đúng chuẩn.
  - Các `class`, `interface`, `enum`, `record` chưa có Javadoc mô tả tổng quan (bổ sung ngắn gọn 1–2 câu nêu rõ vai trò).
  - Các thuộc tính (field) của `class` (đặc biệt là `Entity`, `DTO`, `Config`/`Properties`, hằng số `enum`, hoặc thành phần của `record` qua `@param`) chưa có Javadoc giải thích ý nghĩa nghiệp vụ/dữ liệu.
- **Đối tượng bỏ qua**:
  - Các hàm `getter`/`setter` đơn giản, `equals`, `hashCode`, `toString`, hoặc constructor/boilerplate không chứa logic nghiệp vụ.
  - Các hàm `private` (trừ khi đã có sẵn Javadoc cần Việt hóa, hoặc hàm chứa thuật toán phức tạp cần giải thích).
  - Các field kỹ thuật hiển nhiên như `serialVersionUID`, `log`/`LOGGER`, hoặc dependency inject (`private final ...Service`) nếu tên biến đã tự giải thích rõ ràng.

---

## 3. Văn Phong & Thuật Ngữ

- **Văn phong**: Tiếng Việt kỹ thuật tự nhiên, rõ ràng, súc tích, đi thẳng vào hành vi của hàm/lớp. Tránh dịch word-by-word cứng nhắc từ tiếng Anh.
- **Thuật ngữ chuyên ngành**: Giữ nguyên tiếng Anh đối với các thuật ngữ phổ biến trong dự án và ngành phần mềm để tránh tối nghĩa:
  - Kiến trúc & Giao tiếp: `API`, `Endpoint`, `OpenFeign`, `FeignClient`, `Webhook`, `Kafka`, `Saga`, `Outbox`, `Gateway`, `Microservice`.
  - Bảo mật & Định danh: `JWT`, `Token`, `Bearer`, `Role`, `Privilege`, `Header`.
  - Dữ liệu & Java: `DTO`, `Entity`, `Repository`, `Service`, `Controller`, `Page`, `Pageable`, `Optional`, `Cache`, `Lock`, `Enum`, `Record`, `Payload`, `Request`, `Response`.
  - Nghiệp vụ hệ thống: `SKU`, `Shop`, `Variant`, `Voucher`, `Checkout`, `Cart`.

---

## 4. Cấu Trúc Javadoc Chuẩn

### 4.1. Cấu trúc tổng quát cho phương thức

```java
/**
 * <Câu mở đầu mô tả ngắn gọn mục đích/hành vi của hàm, kết thúc bằng dấu chấm.>
 *
 * <p><Đoạn giải thích bổ sung quy tắc nghiệp vụ, luồng xử lý hoặc lưu ý đặc biệt nếu cần.></p>
 *
 * @param <tênThamSố> <mô tả ngắn gọn ý nghĩa tham số>
 * @return <mô tả giá trị trả về>
 * @throws <TênNgoạiLệ> <điều kiện phát sinh ngoại lệ>
 */
```

### 4.2. Quy tắc chi tiết từng phần

1. **Câu mở đầu (Summary sentence)**:
   - Bắt đầu bằng động từ hoặc cụm động từ rõ nghĩa (VD: *Tạo mới...*, *Cập nhật...*, *Tra cứu...*, *Tính toán...*, *Xác thực...*, *Phát sự kiện...*).
   - Ngắn gọn, súc tích (thường 1 câu) và **bắt buộc kết thúc bằng dấu chấm (`.`)**.
2. **Phần mô tả chi tiết (tùy chọn)**:
   - Chỉ thêm khi hàm có logic phức tạp, nhiều bước (Saga, Cache, Lock, tính cước song song, v.v.).
   - Bọc trong thẻ `<p>...</p>` hoặc danh sách `<ul><li>...</li></ul>` nếu liệt kê nhiều ý.
3. **Các tag Javadoc**:
   - `@param`: Khai báo đầy đủ cho **tất cả** tham số nếu hàm có tham số (không viết nếu hàm không có tham số). Tên tham số phải khớp chính xác 100% với signature.
   - `@return`: Chỉ viết khi hàm có kiểu trả về khác `void`. Mô tả rõ đối tượng/trạng thái trả về.
   - `@throws`: Chỉ viết khi hàm có khai báo `throws` ở signature hoặc chủ động `throw` ngoại lệ nghiệp vụ quan trọng trong thân hàm (VD: `ResponseStatusException`, `IllegalArgumentException`, custom domain exception). Không bịa ra ngoại lệ mà hàm không ném.

### 4.3. Quy tắc cho Class và Thuộc tính (Field)

1. **Javadoc ở mức Class / Interface / Enum / Record**:
   - Nêu ngắn gọn 1–2 câu về vai trò của lớp trong hệ thống (kết thúc bằng dấu chấm).
   - Với `record`, có thể dùng `@param <tênThànhPhần>` ngay trong Javadoc của `record` để giải thích từng trường dữ liệu.
2. **Javadoc cho Thuộc tính (Field)**:
   - Mỗi thuộc tính dữ liệu/nghiệp vụ trong `Entity`, `DTO`, `Properties`, hoặc hằng số `enum` nên có khối Javadoc ngắn gọn ngay phía trên (1 dòng nếu ngắn):
     ```java
     /** Mã định danh duy nhất của kho hàng theo định dạng {@code WH_S{shopId}_{6 hex}}. */
     private String warehouseCode;
     ```
   - Nêu rõ ý nghĩa nghiệp vụ, đơn vị tính (nếu có, VD: VNĐ, gram, giây), giá trị mặc định hoặc ràng buộc quan trọng.
   - Đặt Javadoc phía trên toàn bộ annotation của field đó (VD: trên `@Column`, `@NotBlank`, `@JsonProperty`).

---

## 5. Lưu Ý An Toàn Khi Parse Javadoc

Trình biên dịch Javadoc (`maven-javadoc-plugin` / IDE) rất nhạy cảm với ký tự HTML chưa escape và reference sai:

1. **Ký tự `<`, `>`, `&`**:
   - **Không** viết trực tiếp `<`, `>`, `&` trong phần văn bản mô tả (ví dụ `List<String>`, `a > 0 && b < 10`, `A & B`) vì parser sẽ hiểu nhầm là thẻ HTML hoặc entity lỗi.
   - **Cách xử lý an toàn**: Bọc biểu thức/kiểu generic trong `{@code ...}` (ví dụ: `{@code List<OrderItem>}`, `{@code amount > 0 && stock >= quantity}`, `{@code commerce-service}`) hoặc dùng HTML entity (`&lt;`, `&gt;`, `&amp;`) khi không dùng `{@code ...}`.
2. **Thẻ `{@link ...}`**:
   - Chỉ dùng `{@link TenClass}` hoặc `{@link TenClass#tenHam()}` đối với các class đã có sẵn trong cùng package hoặc đã được `import` trong file. Nếu class chưa được import, dùng `{@code TenClass}` để tránh làm thay đổi phần `import`.
3. **Thẻ HTML đóng mở**:
   - Nếu dùng `<ul>`, `<li>`, `<p>`, phải đảm bảo đóng thẻ đầy đủ (`</ul>`, `</li>`, `</p>`), tránh lồng thẻ sai cấu trúc.

---

## 6. Ví Dụ Minh Họa (Before / After)

### 6.1. Controller

**Before:**
```java
@GetMapping
public ResponseEntity<ApiResponse<Page<SellerProductItemResponse>>> getSellerProducts(
        @ModelAttribute SellerProductFilterRequest filter,
        @AuthenticationPrincipal UserPrincipal userPrincipal
) {
    Page<SellerProductItemResponse> response = productService.getSellerProducts(filter, userPrincipal.getUserId());
    return ResponseEntity.ok(ApiResponse.ok(200, "Thành công", response));
}
```

**After:**
```java
/**
 * Tra cứu danh sách sản phẩm thuộc quyền sở hữu của gian hàng người bán đang đăng nhập.
 *
 * @param filter        DTO chứa điều kiện lọc, tìm kiếm và thông tin phân trang
 * @param userPrincipal thông tin ngữ cảnh người bán đang đăng nhập
 * @return {@link ResponseEntity} chứa {@link ApiResponse} với trang dữ liệu {@link SellerProductItemResponse}
 */
@GetMapping
public ResponseEntity<ApiResponse<Page<SellerProductItemResponse>>> getSellerProducts(
        @ModelAttribute SellerProductFilterRequest filter,
        @AuthenticationPrincipal UserPrincipal userPrincipal
) {
    Page<SellerProductItemResponse> response = productService.getSellerProducts(filter, userPrincipal.getUserId());
    return ResponseEntity.ok(ApiResponse.ok(200, "Thành công", response));
}
```

### 6.2. Service

**Before:**
```java
/**
 * Calculates checkout summary and shipping fee per shop.
 *
 * @param userId buyer ID
 * @param request checkout payload
 * @return calculation result
 */
@Override
public CheckoutCalculationResponse calculateCheckout(Long userId, CheckoutCalculateRequest request) {
    if (request.getQuantity() != null && request.getQuantity() <= 0) {
        throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "Số lượng phải lớn hơn 0");
    }
    // ...
}
```

**After:**
```java
/**
 * Tính toán tổng quan đơn hàng và phí vận chuyển riêng biệt theo từng Shop khi Checkout.
 *
 * <p>Hỗ trợ cả hai luồng từ giỏ hàng và mua ngay, đồng thời áp dụng chiết khấu Voucher tiền hàng và vận chuyển.</p>
 *
 * @param userId  ID tài khoản người mua
 * @param request DTO chứa thông tin sản phẩm, địa chỉ nhận hàng và mã giảm giá
 * @return {@link CheckoutCalculationResponse} chứa chi tiết tiền hàng, phí vận chuyển và tổng thanh toán
 * @throws ResponseStatusException nếu số lượng sản phẩm không hợp lệ hoặc dữ liệu đầu vào sai quy định
 */
@Override
public CheckoutCalculationResponse calculateCheckout(Long userId, CheckoutCalculateRequest request) {
    if (request.getQuantity() != null && request.getQuantity() <= 0) {
        throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "Số lượng phải lớn hơn 0");
    }
    // ...
}
```

### 6.3. OpenFeign Client / Helper

**Before:**
```java
@FeignClient(name = "commerce-service")
public interface CommerceOrderClient {

    @GetMapping("/api/v1/orders")
    CommerceApiResponse<List<CommerceOrderDto>> getBuyerOrders(
            @RequestHeader("X-User-Id") Long userId,
            @RequestHeader("X-User-Roles") String roles,
            @RequestParam(value = "status", required = false) String status
    );
}
```

**After:**
```java
/**
 * Feign client giao tiếp nội bộ với {@code commerce-service} để truy vấn thông tin đơn hàng của người mua.
 */
@FeignClient(name = "commerce-service")
public interface CommerceOrderClient {

    /**
     * Lấy danh sách đơn hàng của người mua theo trạng thái tùy chọn.
     *
     * @param userId ID người dùng truyền qua header {@code X-User-Id}
     * @param roles  danh sách role của người dùng truyền qua header {@code X-User-Roles}
     * @param status trạng thái đơn hàng cần lọc (có thể {@code null} để lấy toàn bộ)
     * @return phản hồi chuẩn chứa danh sách {@code List<CommerceOrderDto>}
     */
    @GetMapping("/api/v1/orders")
    CommerceApiResponse<List<CommerceOrderDto>> getBuyerOrders(
            @RequestHeader("X-User-Id") Long userId,
            @RequestHeader("X-User-Roles") String roles,
            @RequestParam(value = "status", required = false) String status
    );
}
```

### 6.4. Entity / DTO (Class & Thuộc tính)

**Before:**
```java
@Getter
@Setter
public class CreateWarehouseRequest {
    @NotBlank
    private String provinceCity;

    @NotBlank
    private String ward;
}
```

**After:**
```java
/**
 * DTO chứa thông tin yêu cầu tạo mới kho hàng cho Shop theo mô hình địa chỉ 2 cấp.
 */
@Getter
@Setter
public class CreateWarehouseRequest {

    /** Tên Tỉnh / Thành phố nơi đặt kho hàng. */
    @NotBlank
    private String provinceCity;

    /** Tên Phường / Xã nơi đặt kho hàng (không sử dụng cấp Quận / Huyện). */
    @NotBlank
    private String ward;
}
```

---

## 7. Checklist Tự Kiểm Tra Trước Khi Hoàn Thành

- [ ] Chỉ thêm hoặc sửa Javadoc; **không** thay đổi bất kỳ dòng code logic, signature, annotation hay `import` nào.
- [ ] Mọi `class`/`interface`/`enum`/`record` và các thuộc tính nghiệp vụ/dữ liệu quan trọng đều có Javadoc tiếng Việt giải thích rõ vai trò.
- [ ] Mọi hàm `public`/`protected` (trừ getter/setter/boilerplate đơn giản) đều có Javadoc tiếng Việt rõ ràng.
- [ ] Câu mở đầu kết thúc bằng dấu chấm (`.`).
- [ ] Đủ `@param` (khớp tên tham số), `@return` (khi khác `void`), `@throws` (khi thực sự ném/khai báo ngoại lệ).
- [ ] Không chứa `<`, `>`, `&` trần ngoài thẻ HTML hợp lệ; các kiểu generic và biểu thức đều bọc trong `{@code ...}`.
- [ ] Các comment nghiệp vụ nội bộ (`// ...`) trước đó vẫn được giữ nguyên vẹn.
