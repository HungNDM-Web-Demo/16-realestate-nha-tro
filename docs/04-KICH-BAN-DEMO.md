# Kịch bản demo — Quản lý Nhà trọ

> Dùng khi trình bày cho chủ nhà trọ, chủ căn hộ dịch vụ, ký túc xá. Thời lượng 12–15 phút. Đi theo một tháng của nhà trọ: có khách mới thuê, cuối tháng chốt điện nước, lập hóa đơn, thu tiền, đòi nợ, người cũ trả phòng.

---

## 1. Chuẩn bị trước buổi demo

- [ ] Mở sẵn link demo, thử "Đăng nhập nhanh" cả 4 vai trò
- [ ] Đặt lại dữ liệu demo (thanh công cụ demo → Đặt lại dữ liệu) để có 6 kỳ hóa đơn tính tới tháng trước; kỳ hiện tại chưa chốt điện nước; phòng A305 nợ 2 kỳ
- [ ] Đổi tên, logo, màu theo thương hiệu khách (Cài đặt → Thương hiệu) nếu đã biết
- [ ] Mở hai cửa sổ: một cửa sổ thu nhỏ 390×844 (hoặc điện thoại thật) cho Quản lý tòa và Người thuê, một cửa sổ desktop cho Chủ nhà
- [ ] Chuẩn bị điện thoại có app ngân hàng để quét thử mã QR (chỉ để xem, không chuyển tiền) và một ảnh chụp bất kỳ làm "biên lai"
- [ ] Ghi nhớ số liệu mẫu: phòng A203 kỳ này 4.123.000 ₫; phòng A104 vào ở ngày 15 tính 17/31 ngày; phòng A202 trả phòng hoàn cọc 217.000 ₫
- [ ] Hỏi trước khách đang thu tiền phòng trước hay sau, tính nước theo khối hay theo người, để nhấn đúng chỗ

| Link | Ghi chú |
| :--- | :--- |
| FE | `http://localhost:3116` hoặc link online |
| Swagger | `https://localhost:7116/swagger` (khi khách hỏi về tích hợp) |

| Tài khoản | Vai trò | Dùng để cho xem |
| :--- | :--- | :--- |
| `admin` | Quản trị viên | Phân quyền, thương hiệu |
| `quanly` | Chủ nhà | Tổng quan, sơ đồ phòng, hợp đồng, công nợ, nhắc nợ, thanh lý, báo cáo |
| `nhanvien` | Quản lý tòa Bình An | Chốt điện nước, lập hóa đơn, thu tiền, xác nhận chuyển khoản, xử lý sự cố |
| `nguoithue` | Người thuê phòng A203 (Nguyễn Thị Lan) | Xem hóa đơn, quét QR, báo đã chuyển khoản, báo sự cố |

Mật khẩu: giá trị `DEMO_PASSWORD` trong `.env`, hoặc bấm "Đăng nhập nhanh".

---

## 2. Câu chuyện demo

> Mở đầu: "Anh/chị hình dung mình có hai khu trọ, 40 phòng, mỗi khu có một người trông. Em đi đúng một tháng của nhà mình: có khách mới, cuối tháng ghi điện nước, gửi tiền nhà, thu tiền, đòi nợ, có người trả phòng."

| # | Vai trò | Thao tác | Điểm cần nhấn mạnh |
| :---: | :--- | :--- | :--- |
| 1 | Chủ nhà | Đăng nhập, mở Tổng quan: 33/38 phòng đang thuê (86,8%), tiền kỳ trước đã thu và còn nợ, việc cần làm: 3 hợp đồng sắp hết hạn, phòng A305 nợ quá 30 ngày | Mở máy là biết phòng nào trống, ai còn nợ, hợp đồng nào sắp hết, không phải lật sổ |
| 2 | Chủ nhà | Mở Sơ đồ phòng tòa Bình An: màu theo trạng thái, A305 cờ đỏ; bấm A104 đang trống xem giá, diện tích, tiện nghi | Khách gọi hỏi phòng là trả lời được ngay phòng nào trống, giá bao nhiêu |
| 3 | Chủ nhà | Khách mới Trần Văn Bình thuê A104: thêm người thuê (CCCD, số điện thoại, biển số xe), lập hợp đồng 12 tháng từ ngày 15, cọc 3.000.000 ₫, ghi chỉ số bàn giao; in hợp đồng | Hợp đồng điền sẵn, ngày hết hạn tự tính; tiền cọc ghi riêng, không lẫn vào doanh thu; nhắc người thuê đăng ký tạm trú |
| 4 | Quản lý tòa | Đổi sang Quản lý tòa trên cửa sổ điện thoại; Chốt điện nước tòa Bình An: phòng A203 nhập điện 1.530, nước 342, thấy ngay 138 kWh, 11 m³; bấm "Lưu và tiếp"; phòng A104 mới vào có chỉ số cũ là chỉ số bàn giao, nhập điện 2.100, nước 418; thử nhập phòng A204 điện 2.100, hệ thống cảnh báo gấp 3 lần bình thường, sửa lại 1.790 | Đi tới đâu nhập tới đó, không cần sổ; nhập nhầm số là hệ thống báo ngay, tránh tính sai cho khách |
| 5 | Quản lý tòa | Lập hóa đơn kỳ này cho tòa Bình An: A203 tổng 4.123.000 ₫; A104 vào ở ngày 15 nên tiền phòng tính 17/31 ngày, tổng 2.036.000 ₫; phát hành | Tiền phòng, điện, nước, rác, wifi, xe cộng tự động; vào giữa tháng cũng tính đúng, không cãi nhau |
| 6 | Người thuê | Đổi sang Người thuê (A203): trang của tôi có hóa đơn kỳ này, mở xem từng dòng điện nước; bấm "Chuyển khoản" hiện QR đủ số tiền, nội dung `HD2610-A203`; quét thử bằng app ngân hàng; bấm "Tôi đã chuyển khoản" kèm ảnh | Người thuê tự xem, không phải nhắn hỏi; QR có sẵn số tiền và nội dung, không chuyển nhầm |
| 7 | Quản lý tòa | Đổi lại Quản lý tòa: thông báo có báo chuyển khoản; đối chiếu rồi bấm "Xác nhận"; hóa đơn A203 "Đã thanh toán", có phiếu thu | Chủ nhà biết ngay phòng nào đã đóng, không phải dò sao kê từng người |
| 8 | Chủ nhà | Đổi sang Chủ nhà, mở Công nợ: A305 nợ 2 kỳ 5.400.000 ₫, quá hạn 37 ngày; bấm "Nhắc nợ", nội dung tự điền, gửi Zalo (giả lập) | Đòi nợ lịch sự, đúng số, có lịch sử đã nhắc lúc nào; không phải tự soạn tin |
| 9 | Chủ nhà | Mở Nhắc việc: hợp đồng A105 hết hạn cuối tháng; bấm "Gia hạn" 12 tháng, giá mới 2.900.000 ₫ từ kỳ sau | Không quên hợp đồng hết hạn; tăng giá có ngày áp dụng rõ ràng |
| 10 | Chủ nhà | Người thuê A202 báo trả phòng từ tháng trước, trả ngày 20: thanh lý, nhập chỉ số cuối, trừ 150.000 ₫ vỡ kính; hệ thống tính hóa đơn cuối 2.583.000 ₫, cấn trừ cọc, hoàn 217.000 ₫; nhắc trường hợp báo không đủ 30 ngày thì mất cọc | Trả phòng tính tiền minh bạch tới từng ngày, cọc hoàn bao nhiêu có giấy tờ rõ ràng |
| 11 | Người thuê, Quản lý tòa | Người thuê báo sự cố "Vòi nước bồn rửa bị rò" kèm ảnh; Quản lý tòa tiếp nhận, sửa xong ghi chi phí 80.000 ₫ người thuê chịu; khoản này tự vào hóa đơn kỳ sau | Sự cố có người nhận, có hạn xử lý; chi phí không bị quên khi tính tiền tháng sau |
| 12 | Chủ nhà, Quản trị viên | Chủ nhà mở Báo cáo: doanh thu 6 tháng theo khoản, tỉ lệ lấp đầy; đổi sang Quản trị viên, cho thấy Quản lý tòa không có quyền xem báo cáo doanh thu, chỉ thấy tòa mình | Chủ nhà nắm toàn bộ, người trông nhà chỉ thấy phần được giao |

> Kết: "Số liệu anh/chị vừa thấy là dữ liệu mẫu. Bản triển khai sẽ nhập đúng phòng, giá, đơn giá điện nước và người thuê hiện tại của nhà mình."

---

## 3. Câu hỏi khách hay hỏi

| Câu hỏi | Trả lời gợi ý |
| :--- | :--- |
| Có gửi hóa đơn qua Zalo tự động không? | Demo giả lập, cho sao chép tin nhắn để dán vào Zalo. Bản triển khai gửi tự động qua Zalo OA (ZNS), cần đăng ký Zalo OA và có phí theo tin. |
| Người thuê chuyển khoản có tự gạch nợ không? | Demo để người thuê báo, quản lý xác nhận. Bản triển khai có thể nối dịch vụ đọc biến động số dư tài khoản để tự gạch nợ theo nội dung chuyển khoản. |
| Tôi thu tiền phòng trước, điện nước sau thì sao? | Demo thu sau theo tháng. Bản triển khai cấu hình được thu trước tiền phòng, điện nước tháng trước cộng vào cùng hóa đơn. |
| Nước tính theo người, không theo khối được không? | Được, mỗi tòa chọn cách tính nước riêng; tòa Hoa Sen trong demo đang tính 100.000 ₫/người. |
| Giá điện tôi tự đặt được không? | Được, mỗi tòa một đơn giá, đổi giá có kỳ áp dụng. Khi triển khai nên đối chiếu quy định về giá điện cho người thuê trọ để tránh khiếu nại. |
| Có khai báo tạm trú cho người thuê không? | Demo chỉ ghi nhận đã đăng ký hay chưa và nhắc chủ nhà. Khai báo thực hiện trên VNeID hoặc với công an phường; hệ thống xuất được danh sách người thuê để khai. |
| Có tính phạt trả chậm không? | Demo chưa tính. Bản triển khai thêm được khoản phạt chậm trả theo điều khoản hợp đồng. |
| Có công tơ điện tử tự đọc số không? | Ngoài phạm vi demo. Nếu công tơ có kết nối và API, bản triển khai có thể lấy số tự động. |
| Mất mạng có ghi điện nước được không? | Bản demo cần mạng. Bản triển khai có thể cho nhập khi mất mạng rồi đồng bộ, cần khảo sát thêm. |

---

## 4. Sau buổi demo

- [ ] Ghi lại yêu cầu khách nêu thêm vào `01-TONG-QUAN-NGHIEP-VU.md`
- [ ] Ghi lại số tòa, số phòng, cách thu tiền (trước hay sau), cách tính nước, các dịch vụ thu, mức cọc, ngày chốt và hạn đóng tiền
- [ ] Ghi lại khách có dùng Zalo OA, tài khoản ngân hàng nhận tiền, có người trông nhà hay không
- [ ] Gửi link demo và tài khoản `quanly`, `nhanvien`, `nguoithue` cho khách tự trải nghiệm (nếu đồng ý)
