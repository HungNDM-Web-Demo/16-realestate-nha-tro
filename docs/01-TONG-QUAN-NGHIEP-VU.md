# Tổng quan nghiệp vụ — Quản lý Nhà trọ

> Mô tả demo này cho ai, giải quyết vấn đề gì. Cập nhật khi phạm vi thay đổi.

---

## 1. Thông tin chung

| Mục | Nội dung |
| :--- | :--- |
| Tên demo | Quản lý Nhà trọ |
| Mã dự án | `realestate-nha-tro` |
| Ngành / loại khách hàng | Chủ nhà trọ, căn hộ dịch vụ, ký túc xá |
| Loại sản phẩm | Web quản lý + giao diện điện thoại cho người thuê |
| Ý tưởng gốc | 6.1 · Quản lý nhà trọ / căn hộ cho thuê (`ZDOCS/02-KE-HOACH/DANH-SACH-Y-TUONG.md`) |
| Người phụ trách | |
| Trạng thái | Có tài liệu nghiệp vụ (SRS, use case, kịch bản demo, kịch bản kiểm thử), chưa code |

## 2. Vấn đề của khách hàng

- Ghi chỉ số điện nước bằng sổ
- Tính tiền hằng tháng thủ công, dễ nhầm
- Quên ngày hết hạn hợp đồng
- Khó theo dõi ai còn nợ

## 3. Giải pháp demo thể hiện

- Tòa nhà, phòng, trạng thái trống hoặc đang thuê
- Người thuê, hợp đồng, tiền cọc
- Chốt chỉ số điện nước
- Tạo hóa đơn tháng tự động: tiền phòng, điện, nước, dịch vụ
- Thu tiền, công nợ, nhắc nợ; người thuê xem hóa đơn, báo sự cố

## 4. Vai trò người dùng

| Vai trò | Mã | Dùng trên | Làm được gì |
| :--- | :--- | :--- | :--- |
| Quản trị viên | ADMIN | Desktop | Toàn quyền |
| Chủ nhà | MANAGER | Desktop | Phòng, hợp đồng, báo cáo |
| Quản lý tòa | STAFF | Điện thoại | Chốt điện nước, thu tiền, xử lý sự cố |
| Người thuê | TENANT | Điện thoại | Xem hóa đơn, báo sự cố |

## 5. Phân hệ và chức năng

| Area | Chức năng | Màn hình (route FE dự kiến) | Ưu tiên |
| :--- | :--- | :--- | :---: |
| `System` | Đăng nhập, tài khoản, vai trò, cài đặt | Có sẵn | — |
| `Rental` | Tổng quan: phòng trống, thu tiền kỳ này, việc cần làm; trang người thuê | `/dashboard` | P1 |
| `Rental` | Tòa nhà, phòng | `/buildings`, `/rooms` | P1 |
| `Rental` | Người thuê, hợp đồng | `/tenants`, `/contracts` | P1 |
| `Rental` | Chốt điện nước | `/meter-readings` | P1 |
| `Rental` | Hóa đơn tháng | `/invoices` | P1 |
| `Rental` | Thu tiền, công nợ | `/payments` | P1 |
| `Rental` | Nhắc nợ, hợp đồng sắp hết hạn | `/reminders` | P2 |
| `Rental` | Báo sự cố | `/issues` | P2 |
| `Rental` | Báo cáo doanh thu, tỉ lệ lấp đầy | `/reports` | P2 |

P1 = bắt buộc có để demo · P2 = nên có · P3 = làm thêm nếu còn thời gian.

## 6. Luồng nghiệp vụ chính

```mermaid
flowchart LR
    S0["Ký hợp đồng"] --> S1["Chốt điện nước cuối tháng"] --> S2["Tạo hóa đơn"] --> S3["Gửi người thuê"] --> S4["Thu tiền"] --> S5["Nhắc nợ"]
```

## 7. Phạm vi

**Có trong demo:**

- Phòng, hợp đồng, điện nước, hóa đơn, công nợ

**Không làm (nói rõ với khách nếu được hỏi):**

- Thanh toán online thật
- Gửi Zalo thật
- Các kết nối bên ngoài khác chỉ giả lập

## 8. Dữ liệu mẫu cần chuẩn bị

| Dữ liệu | Số lượng | Ghi chú |
| :--- | :---: | :--- |
| Tòa nhà | 2 |  |
| Phòng | 40 |  |
| Người thuê | 35 |  |
| Hợp đồng | 35 | Có hợp đồng sắp hết hạn |
| Hóa đơn | 6 tháng | Có hóa đơn còn nợ |
