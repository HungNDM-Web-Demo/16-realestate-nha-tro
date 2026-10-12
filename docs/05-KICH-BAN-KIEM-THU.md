# Kịch bản kiểm thử nghiệm thu — Quản lý Nhà trọ

> Test case theo use case trong `03-USE-CASE.md`. Phần nền (đăng nhập, phân quyền, đặt lại dữ liệu) dùng TC-SYS-01 đến TC-SYS-10 trong `ZDOCS/03-KIEN-TRUC/USE-CASE-NEN.md` §8.

---

## 1. Cách chạy

| Mục | Nội dung |
| :--- | :--- |
| Môi trường | Chạy cả hai chế độ: dữ liệu mẫu (`mock`) và BE thật (`api`) |
| Dữ liệu | Đặt lại dữ liệu demo trước mỗi lượt test, chạy test case theo thứ tự. Test case ghi "dữ liệu vừa đặt lại" thì chạy ngay sau khi đặt lại |
| Ngày chạy | Số liệu dưới đây tính cho ngày chạy 12/10/2026: kỳ hiện tại 10/2026 (31 ngày), kỳ trước 09/2026. Chạy ngày khác thì kỳ và ngày dời theo, số tiền tính lại theo BR-12, BR-13 |
| Tài khoản | Theo cột "Vai trò" của từng test case, dùng "Đăng nhập nhanh" |
| Thiết bị | Test case vai trò Quản lý tòa, Người thuê chạy ở cửa sổ 390×844 |
| Đạt | Kết quả thực tế khớp toàn bộ cột "Kết quả mong đợi" |

Dữ liệu mẫu dùng trong test:

- Tòa `BA` Nhà trọ Bình An: 20 phòng A101–A405 (4 tầng × 5 phòng), quản lý tòa là tài khoản `nhanvien`, hạn thanh toán ngày 05. Bảng giá: điện 3.500 ₫/kWh, nước 20.000 ₫/m³ theo đồng hồ, Internet 100.000 ₫/phòng, rác 20.000 ₫/phòng, gửi xe 100.000 ₫/xe.
- Tòa `HS` Căn hộ dịch vụ Hoa Sen: 20 phòng B101–B405, quản lý tòa khác (không có tài khoản demo). Bảng giá: điện 3.800 ₫/kWh, nước 100.000 ₫/người, dịch vụ chung 150.000 ₫/phòng, gửi xe 120.000 ₫/xe.
- Tổng 40 phòng: 33 đang thuê, 5 trống, 2 đang sửa chữa. Hợp đồng hết hạn trong 30 ngày: A105 (31/10/2026), B204, B302.
- A203: giá 3.200.000 ₫, tối đa 3 người; hợp đồng của Nguyễn Thị Lan (tài khoản `nguoithue`), 2 người, 1 xe, cọc 3.200.000 ₫; chỉ số kỳ 09: điện 1.392, nước 331; không nợ.
- A104: Trống, giá niêm yết 3.000.000 ₫, tối đa 2 người.
- A204: đang thuê, giá 3.200.000 ₫, 2 người, 1 xe; chỉ số kỳ 09: điện 1.645, nước 410; trung bình điện 3 kỳ 150 kWh.
- A105: hợp đồng hết hạn 31/10/2026, giá 2.800.000 ₫.
- A202: giá 2.800.000 ₫, cọc 2.800.000 ₫, 2 người, 2 xe, hợp đồng tới 31/12/2026; chỉ số kỳ 09: điện 3.210, nước 512; không nợ.
- A302: giá 3.000.000 ₫, cọc 3.000.000 ₫, 1 người, 1 xe, hợp đồng tới 31/03/2027; chỉ số kỳ 09: điện 1.880, nước 260; không nợ.
- A305 (Lý Văn Hậu): `HD2608-A305` còn nợ 1.500.000 ₫ (hạn 05/09/2026), `HD2609-A305` chưa thu 3.900.000 ₫ (hạn 05/10/2026).
- Kỳ 10/2026 chưa chốt chỉ số phòng nào, chưa có hóa đơn tháng.

---

## 2. Test case

### 2.1. Tòa nhà và phòng

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-01 | UC-01 | Tạo bảng giá mới từ kỳ sau | Chủ nhà | 1. Tòa nhà → Bình An → Bảng giá và dịch vụ 2. Tạo bảng giá mới, kỳ hiệu lực 11/2026, điện 3.800 ₫/kWh, giữ nguyên các mục khác 3. Lưu | Có 2 bảng giá: hiện tại (điện 3.500 ₫) và "Áp dụng từ kỳ 11/2026" (điện 3.800 ₫); hóa đơn kỳ 10/2026 ở TC-22 vẫn dùng 3.500 ₫ |
| TC-02 | UC-01 | Đơn giá không hợp lệ | Chủ nhà | 1. Tạo bảng giá, nhập điện 3.550 ₫ 2. Lưu | Báo lỗi tại ô đơn giá điện (phải là bội số 100 ₫), không lưu |
| TC-03 | UC-02 | Thêm phòng | Chủ nhà | 1. Dữ liệu vừa đặt lại 2. Sơ đồ phòng → Thêm phòng: A406, tòa Bình An, tầng 4, 22 m², giá 3.300.000 ₫, tối đa 3 người, có máy lạnh 3. Lưu | A406 hiện ở tầng 4, màu Trống; đầu trang: 6 phòng trống, tỉ lệ lấp đầy 84,6% (33 ÷ 39) |
| TC-04 | UC-02 | Thêm phòng trùng mã | Chủ nhà | 1. Thêm phòng mã A203 2. Lưu | Báo lỗi tại ô mã, không lưu |
| TC-05 | UC-02 | Chuyển phòng đang thuê sang sửa chữa | Chủ nhà | 1. Mở chi tiết A203 | Nút "Bắt đầu sửa chữa" bị vô hiệu, chú thích "Phòng đang có hợp đồng" |
| TC-06 | UC-02 | Quản lý tòa xem sơ đồ phòng | Quản lý tòa | 1. Mở Sơ đồ phòng 2. Gõ thẳng URL chi tiết phòng B101 | Bước 1 chỉ có tòa Bình An, không có nút "Thêm phòng"; bước 2 chuyển `/forbidden` |

### 2.2. Người thuê và hợp đồng

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-07 | UC-03 | Thêm người thuê | Chủ nhà | 1. Người thuê → Thêm 2. Trần Văn Bình, CCCD 079096001234, số điện thoại 0905123456, biển số 59X1-123.45 3. Lưu | Người thuê có trong danh sách, CCCD hiện `0790****1234`, tạm trú "Chưa đăng ký" |
| TC-08 | UC-03 | Trùng CCCD | Chủ nhà | 1. Sau TC-07, thêm người khác với CCCD 079096001234 | Báo lỗi tại ô CCCD, không lưu |
| TC-09 | UC-04 | Lập hợp đồng phòng trống, thu cọc | Chủ nhà | 1. Sơ đồ phòng → A104 → Lập hợp đồng 2. Người thuê Trần Văn Bình, ngày bắt đầu 15/10/2026, 12 tháng, giá 3.000.000 ₫, cọc 3.000.000 ₫, dịch vụ Internet, rác, gửi xe 1 xe, chỉ số bàn giao điện 2.040, nước 415, thu cọc tiền mặt 3. Lưu hợp đồng | Hợp đồng `HDT26nnnn` "Đang hiệu lực", ngày kết thúc 14/10/2027; A104 Đang thuê; phiếu thu `PT261012nnn` loại "Tiền cọc" 3.000.000 ₫; bản in hợp đồng điền đủ thông tin |
| TC-10 | UC-04 | Vượt số người tối đa | Chủ nhà | 1. Dữ liệu vừa đặt lại, lập hợp đồng A104 với người đại diện và 2 người ở cùng | Báo "Phòng tối đa 2 người", không lưu |
| TC-11 | UC-04 | Tiền cọc quá 3 tháng | Chủ nhà | 1. Lập hợp đồng A104, giá 3.000.000 ₫, cọc 10.000.000 ₫ | Báo "Tiền cọc tối đa 9.000.000 ₫", không lưu |
| TC-12 | UC-04 | Lập hợp đồng cho phòng đang thuê | Chủ nhà | 1. Mở chi tiết A203 2. Hợp đồng → Lập hợp đồng, mở danh sách chọn phòng | A203 không có nút "Lập hợp đồng"; danh sách chọn phòng chỉ gồm phòng Trống, không có A203 |
| TC-13 | UC-05 | Gia hạn hợp đồng có giá mới | Chủ nhà | 1. Mở hợp đồng A105 → Gia hạn 2. 12 tháng, giá mới 2.900.000 ₫ từ kỳ 11/2026 3. Lưu | Ngày kết thúc 31/10/2027; hợp đồng "Đang hiệu lực", không còn trong danh sách sắp hết hạn; lịch sử gia hạn có 1 dòng; hóa đơn kỳ 10/2026 của A105 vẫn tính 2.800.000 ₫ |
| TC-14 | UC-06 | Thanh lý, hoàn cọc | Chủ nhà | 1. Mở hợp đồng A202 → Thanh lý 2. Ngày báo trả phòng 15/09/2026, ngày trả phòng 20/10/2026 3. Chỉ số cuối điện 3.290, nước 519; khấu trừ "Vỡ kính cửa sổ" 150.000 ₫ 4. Xem trước, xác nhận | Không mất cọc (báo trước 35 ngày). Hóa đơn `HD2610-A202-TL`: tiền phòng 20 ngày 1.806.000 ₫; điện 80 kWh 280.000 ₫; nước 7 m³ 140.000 ₫; Internet 65.000 ₫; rác 13.000 ₫; gửi xe 2 xe 129.000 ₫; khấu trừ 150.000 ₫; tổng 2.583.000 ₫. Cấn trừ cọc 2.583.000 ₫, hóa đơn "Đã thanh toán"; phiếu chi hoàn cọc 217.000 ₫; hợp đồng "Đã thanh lý"; A202 Trống |
| TC-15 | UC-06 | Thanh lý trước hạn, mất cọc | Chủ nhà | 1. Mở hợp đồng A302 → Thanh lý 2. Ngày báo 12/10/2026, ngày trả 20/10/2026 3. Chỉ số cuối điện 1.930, nước 264 4. Xác nhận | Báo "Trả phòng trước hạn, báo trước 8 ngày: mất cọc 3.000.000 ₫". Hóa đơn `HD2610-A302-TL` tổng 2.333.000 ₫ (tiền phòng 1.935.000 ₫, điện 175.000 ₫, nước 80.000 ₫, Internet 65.000 ₫, rác 13.000 ₫, gửi xe 65.000 ₫) "Chưa thanh toán", hạn 20/10/2026; hoàn cọc 0 ₫; khoản mất cọc ghi "Thu nhập khác – phạt cọc"; A302 Trống |
| TC-16 | UC-06 | Thanh lý với dữ liệu sai | Chủ nhà | 1. Thanh lý A204, ngày trả phòng 20/12/2026 2. Sửa ngày trả 20/10/2026, chỉ số điện cuối 1.600 | Bước 1 báo lỗi tại ô ngày trả phòng (quá hôm nay + 30 ngày); bước 2 báo lỗi tại ô chỉ số (nhỏ hơn 1.645) |

### 2.3. Điện nước

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-17 | UC-07 | Chốt chỉ số một phòng | Quản lý tòa | 1. Chốt điện nước → tòa Bình An, kỳ 10/2026 2. Chọn A203, nhập điện 1.530, nước 342 3. Lưu và tiếp | Chỉ số cũ hiện 1.392 và 331; tiêu thụ 138 kWh, 11 m³; A203 "Đã chốt"; tiến độ tăng 1; màn chuyển sang phòng kế |
| TC-18 | UC-07 | Chỉ số cũ của hợp đồng mới | Quản lý tòa | 1. Sau TC-09, chọn A104 2. Nhập điện 2.100, nước 418, lưu | Chỉ số cũ là chỉ số bàn giao 2.040 và 415; tiêu thụ 60 kWh, 3 m³ |
| TC-19 | UC-07 | Chỉ số mới nhỏ hơn chỉ số cũ | Quản lý tòa | 1. Chọn A204, nhập điện 1.000 | Báo "Chỉ số mới phải từ 1.645 trở lên", không lưu |
| TC-20 | UC-07 | Tiêu thụ bất thường | Quản lý tòa | 1. A204 nhập điện 2.100, nước 421 2. Bấm "Vẫn lưu" 3. Mở lại A204, sửa điện 1.790, lưu | Bước 1 cảnh báo 455 kWh, gấp 3,0 lần trung bình 150 kWh; bước 2 lưu, gắn cờ "Bất thường"; bước 3 tiêu thụ 145 kWh, bỏ cờ |

### 2.4. Hóa đơn

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-21 | UC-08 | Lập hóa đơn hàng loạt | Quản lý tòa | 1. Sau TC-14, TC-15, TC-17, TC-18, TC-20: Hóa đơn → Lập hóa đơn, tòa Bình An, kỳ 10/2026 | Báo đã lập 3 hóa đơn nháp (A104, A203, A204); các phòng đang thuê còn lại bị bỏ qua với lý do "Chưa chốt chỉ số kỳ 10/2026"; A202, A302 không có hóa đơn tháng |
| TC-22 | UC-08 | Kiểm tra hóa đơn đủ tháng | Quản lý tòa | 1. Mở `HD2610-A203` | Tiền phòng 3.200.000 ₫; điện 1.392 → 1.530, 138 kWh × 3.500 = 483.000 ₫; nước 331 → 342, 11 m³ × 20.000 = 220.000 ₫; Internet 100.000 ₫; rác 20.000 ₫; gửi xe 1 xe 100.000 ₫; tổng 4.123.000 ₫; nợ kỳ trước 0 ₫ |
| TC-23 | UC-08 | Hóa đơn vào ở giữa tháng | Quản lý tòa | 1. Mở `HD2610-A104` | Tiền phòng 17/31 ngày 1.645.000 ₫; điện 60 kWh 210.000 ₫; nước 3 m³ 60.000 ₫; Internet 55.000 ₫; rác 11.000 ₫; gửi xe 55.000 ₫; tổng 2.036.000 ₫ |
| TC-24 | UC-08 | Khoản phát sinh trên hóa đơn nháp | Quản lý tòa | 1. `HD2610-A203` → thêm khoản phát sinh "Thay bóng đèn" 50.000 ₫ 2. Xóa khoản đó | Bước 1 tổng 4.173.000 ₫; bước 2 tổng 4.123.000 ₫ |
| TC-25 | UC-08 | Phát hành hóa đơn | Quản lý tòa | 1. Tick `HD2610-A203`, `HD2610-A204` → Phát hành | Hai hóa đơn "Chưa thanh toán", hạn 05/11/2026; người thuê A203 nhận thông báo hóa đơn mới |
| TC-26 | UC-08 | Lập trùng kỳ | Quản lý tòa | 1. Lập hóa đơn tòa Bình An kỳ 10/2026 lần nữa | Không tạo thêm hóa đơn cho A203; lý do bỏ qua "Đã có hóa đơn HD2610-A203" |
| TC-27 | UC-07 | Sửa chỉ số sau khi phát hành | Quản lý tòa | 1. Chốt điện nước → A203 → sửa điện | Ô chỉ số bị khóa, chú thích "Hủy hóa đơn trước khi sửa" |
| TC-28 | UC-06 | Thanh lý khi hóa đơn kỳ đã phát hành | Chủ nhà | 1. Thanh lý hợp đồng A203, ngày trả phòng 25/10/2026 | Từ chối, báo "Hủy hóa đơn HD2610-A203 trước khi thanh lý" |
| TC-29 | UC-08 | Hủy hóa đơn đã phát hành | Quản lý tòa, Chủ nhà | 1. Quản lý tòa mở `HD2610-A204` 2. Đổi sang Chủ nhà, bấm Hủy, để trống lý do 3. Nhập lý do "Nhập nhầm chỉ số", xác nhận 4. Lập hóa đơn tòa Bình An kỳ 10/2026 | Bước 1 không có nút "Hủy"; bước 2 nút xác nhận bị khóa; bước 3 hóa đơn "Đã hủy"; bước 4 tạo lại hóa đơn nháp cho A204 |

### 2.5. Thu tiền và công nợ

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-30 | UC-09 | Thu một phần | Quản lý tòa | 1. Sau TC-25, Thu tiền → A203 2. Tiền mặt 2.000.000 ₫ 3. Lưu | Phiếu thu `PT261012nnn` 2.000.000 ₫, phân bổ vào `HD2610-A203`; hóa đơn "Thanh toán một phần", còn nợ 2.123.000 ₫; mở bản in phiếu thu |
| TC-31 | UC-09 | Thu vượt số nợ | Quản lý tòa | 1. Thu tiền A203, nhập 3.000.000 ₫ | Báo "Vượt số còn nợ 2.123.000 ₫", không lưu |
| TC-32 | UC-09 | Phân bổ hóa đơn cũ trước | Quản lý tòa | 1. Thu tiền A305, chuyển khoản 3.000.000 ₫ 2. Lưu | Xem trước và kết quả: `HD2608-A305` nhận 1.500.000 ₫ → "Đã thanh toán"; `HD2609-A305` nhận 1.500.000 ₫ → "Thanh toán một phần", còn 2.400.000 ₫; tổng nợ A305 2.400.000 ₫ |
| TC-33 | UC-09 | Hủy phiếu thu | Quản lý tòa, Chủ nhà | 1. Quản lý tòa mở phiếu thu của TC-32 2. Đổi sang Chủ nhà, Hủy phiếu thu, lý do "Ghi nhầm phòng" | Bước 1 không có nút "Hủy"; bước 2 phiếu "Đã hủy"; `HD2608-A305` còn nợ 1.500.000 ₫ ("Thanh toán một phần"), `HD2609-A305` còn 3.900.000 ₫ ("Chưa thanh toán") |
| TC-34 | UC-10 | Danh sách công nợ | Chủ nhà | 1. Dữ liệu vừa đặt lại 2. Thu tiền → Công nợ 3. Mở Sơ đồ phòng | Dòng A305: 2 hóa đơn, tổng nợ 5.400.000 ₫, quá hạn lâu nhất 37 ngày, nhóm "Trên 30 ngày"; A305 có cờ đỏ trên sơ đồ phòng |
| TC-35 | UC-10 | Quản lý tòa xem công nợ | Quản lý tòa | 1. Thu tiền → Công nợ | Chỉ có phòng tòa Bình An; không có tab Tiền cọc |
| TC-36 | UC-10 | Theo dõi tiền cọc | Chủ nhà | 1. Sau TC-09, TC-14, TC-15: Thu tiền → Tiền cọc | A104: thu cọc 3.000.000 ₫, đang giữ 3.000.000 ₫; A202: thu 2.800.000 ₫, cấn trừ 2.583.000 ₫, hoàn 217.000 ₫, đang giữ 0 ₫; A302: mất cọc 3.000.000 ₫, đang giữ 0 ₫ |

### 2.6. Nhắc nợ

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-37 | UC-11 | Gửi nhắc nợ | Chủ nhà | 1. Dữ liệu vừa đặt lại 2. Nhắc việc → Nhắc nợ → tick A305 → Gửi nhắc, kênh Zalo 3. Mở lịch sử nhắc của A305 ở tab Công nợ | Nội dung tự điền có A305, Nhà trọ Bình An, `HD2608-A305` 1.500.000 ₫, `HD2609-A305` 3.900.000 ₫, tổng 5.400.000 ₫, thông tin chuyển khoản; lịch sử ghi "Đã gửi (giả lập Zalo)" kèm giờ và người gửi |
| TC-38 | UC-11 | Nhắc lần hai trong ngày | Chủ nhà | 1. Sau TC-37, gửi nhắc A305 lần nữa | Báo "Đã nhắc hôm nay lúc hh:mm", không gửi |

### 2.7. Người thuê và sự cố

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-39 | UC-12 | Trang người thuê và QR | Người thuê | 1. Sau TC-25 (chưa chạy TC-30), đăng nhập nhanh vai trò Người thuê 2. Mở hóa đơn cần thanh toán 3. Bấm "Chuyển khoản" | Trang có phòng A203, tòa Bình An, ngày hết hạn hợp đồng, tiền cọc 3.200.000 ₫; hóa đơn `HD2610-A203` 4.123.000 ₫, hạn 05/11/2026, đủ dòng như TC-22; QR có số tiền 4.123.000 ₫, nội dung `HD2610-A203` |
| TC-40 | UC-12 | Báo đã chuyển khoản, quản lý xác nhận | Người thuê, Quản lý tòa | 1. Người thuê bấm "Tôi đã chuyển khoản", số tiền 4.123.000 ₫, đính ảnh, gửi 2. Đổi sang Quản lý tòa → Thu tiền → Chờ xác nhận chuyển khoản → Xác nhận | Bước 1 hóa đơn nhãn "Chờ xác nhận", nút báo chuyển khoản bị vô hiệu, công nợ chưa đổi; bước 2 tạo phiếu thu chuyển khoản 4.123.000 ₫, hóa đơn "Đã thanh toán" |
| TC-41 | UC-12 | Báo chuyển khoản bị từ chối | Người thuê, Quản lý tòa | 1. Người thuê báo chuyển khoản cho `HD2610-A203` 2. Quản lý tòa bấm Từ chối, lý do "Chưa thấy tiền về" 3. Người thuê mở lại hóa đơn | Bước 3 thấy thông báo từ chối kèm lý do; hóa đơn vẫn "Chưa thanh toán"; nút "Tôi đã chuyển khoản" dùng lại được |
| TC-42 | UC-12 | Người thuê truy cập dữ liệu khác | Người thuê | 1. Gõ URL `/rooms` 2. Gõ URL chi tiết hóa đơn `HD2609-A305` | Cả hai chuyển `/forbidden` |
| TC-43 | UC-13 | Sự cố có chi phí người thuê chịu | Người thuê, Quản lý tòa | 1. Người thuê báo sự cố loại Nước, mức Thường, "Vòi nước bồn rửa bị rò", kèm ảnh 2. Quản lý tòa Tiếp nhận 3. Ghi kết quả "Thay vòi", chi phí 80.000 ₫, bên chịu Người thuê 4. Người thuê bấm "Đã xong" | Bước 1 sự cố `SC261012nn` "Mới"; bước 2 "Đang xử lý"; bước 3 "Đã xử lý", hợp đồng A203 có khoản phát sinh chờ 80.000 ₫ (sẽ vào hóa đơn kỳ 11/2026); bước 4 "Đã đóng" |

### 2.8. Tổng quan và báo cáo

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-44 | UC-14 | Tổng quan chủ nhà | Chủ nhà | 1. Dữ liệu vừa đặt lại, đăng nhập nhanh vai trò Chủ nhà | 33 đang thuê, 5 trống, 2 đang sửa chữa, tỉ lệ lấp đầy 86,8%; việc cần làm có 3 hợp đồng sắp hết hạn (A105, B204, B302), A305 trong hóa đơn quá hạn trên 30 ngày, 33 phòng chưa chốt điện nước kỳ 10/2026 |
| TC-45 | UC-14 | Tổng quan cập nhật sau hợp đồng mới và thu tiền | Chủ nhà | 1. Sau TC-09, mở Tổng quan 2. Ghi lại số "Đã thu kỳ này", chạy TC-30, mở lại | Bước 1: 34 đang thuê, 4 trống, tỉ lệ 89,5%; bước 2: "Đã thu kỳ này" tăng đúng 2.000.000 ₫ (tiền cọc của TC-09 không được tính) |
| TC-46 | UC-14 | Tổng quan quản lý tòa | Quản lý tòa | 1. Đăng nhập nhanh vai trò Quản lý tòa | Số liệu, việc cần làm chỉ của tòa Bình An; không có ô chọn tòa Hoa Sen |
| TC-47 | UC-15 | Báo cáo doanh thu không gồm tiền cọc | Chủ nhà | 1. Sau TC-09, TC-15, mở Báo cáo → Doanh thu, kỳ 10/2026 | Cột mất cọc có 3.000.000 ₫ của A302; tiền cọc 3.000.000 ₫ của A104 không nằm trong doanh thu; tổng bảng bằng tổng biểu đồ |

---

## 3. Biên bản chạy thử

| Ngày | Người test | Chế độ | Số TC đạt / tổng | Lỗi ghi nhận |
| :--- | :--- | :--- | :---: | :--- |
| | | | | |
