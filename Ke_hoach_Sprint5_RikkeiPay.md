# LẬP KẾ HOẠCH SPRINT 5 CHO CHỨC NĂNG VÍ RIKKEIPAY (RikkeiGo)

**Vai trò:** Vy (Developer), cùng Lan (Scrum Master) chuẩn bị đề xuất cho Sprint Planning
**Sprint 5:** 10 ngày | **Velocity 4 Sprint gần nhất:** 18, 20, 19, 15 SP | **Buffer:** không trừ

---

## Phần 1 – Phân tích

### 1.1. Planning Capacity sau khi Hà báo nghỉ 2 ngày

**Capacity cá nhân** = Ngày khả dụng × Giờ tập trung/ngày

| Developer | Ngày khả dụng | Phép tính | Capacity cá nhân | Việc cố định | Planning Capacity |
|-----------|---------------|-----------|------------------|--------------|--------------------|
| Tú | 10 | 10 × 6 | 60 giờ | 6 giờ (hỗ trợ vận hành) | 60 − 6 = **54 giờ** |
| Vy | 10 | 10 × 6 | 60 giờ | 0 giờ | **60 giờ** |
| Khoa | 9 | 9 × 6 | 54 giờ | 4 giờ (họp liên nhóm) | 54 − 4 = **50 giờ** |
| Hà | 10 − 2 = 8 | 8 × 6 | 48 giờ | 0 giờ | **48 giờ** |
| **Cả đội** | | | 60 + 60 + 54 + 48 = **222 giờ** | 6 + 4 = **10 giờ** | 54 + 60 + 50 + 48 = **212 giờ** |

**Planning Capacity của Sprint 5 = 222 − 10 = 212 giờ** (không trừ Buffer).

So sánh: nếu Hà không nghỉ thì Planning Capacity là 234 − 10 = 224 giờ. Việc Hà nghỉ làm giảm khoảng 5% (212 / 224 ≈ 94,6%).

### 1.2. Khoảng Velocity dùng để dự báo

- Velocity 4 Sprint gần nhất: thấp nhất **15**, cao nhất **20**, trung bình (18 + 20 + 19 + 15) / 4 = **18 SP**.
- Velocity chỉ là dữ liệu tham khảo, không phải chỉ tiêu. Vì Capacity Sprint này giảm khoảng 5%, đội không nên nhắm vào mức cao nhất (20).
- **Khoảng dự báo dùng cho Sprint 5: khoảng 15–18 SP** (từ mức thấp nhất đến mức trung bình, trung tâm khoảng 17 SP sau khi điều chỉnh theo Capacity giảm).

---

## Phần 2 – Thiết kế

### 2.1. Sprint Goal

> **"Khách hàng thanh toán chuyến đi bằng ví RikkeiPay ổn định và biết chắc tiền đã được hoàn vào ví khi chuyến bị hủy."**

Sprint Goal nêu giá trị cho người dùng, trực tiếp giải quyết hai phàn nàn của khách (thanh toán ví còn lỗi, không biết tiền hoàn về ví chưa).

### 2.2. PBI chọn vào Sprint 5

| # | PBI | SP | Lý do chọn |
|---|-----|----|------------|
| 1 | Thanh toán chuyến đi bằng ví RikkeiPay | 8 | Là lõi của Sprint Goal và đang xếp đầu Product Backlog; cần hoàn tất Integration Test để đạt Done. |
| 2 | Hoàn tiền vào ví khi chuyến bị hủy | 5 | Giải quyết phàn nàn "không biết tiền đã về ví chưa"; làm sau khi PBI 1 chạy được. |
| 3 | Xem số dư và lịch sử giao dịch ví | 3 | Cho khách tự kiểm tra tiền đã về ví hay chưa, phục vụ trực tiếp Sprint Goal; Acceptance Criteria đầy đủ. |

**Tổng: 8 + 5 + 3 = 16 SP**, nằm trong khoảng dự báo 15–18 SP và phù hợp với Capacity đã giảm.

### 2.3. PBI không chọn

| # | PBI | SP | Lý do không chọn |
|---|-----|----|------------------|
| 4 | Gửi SMS khi tài xế sắp đến | 3 | Không phục vụ Sprint Goal và phụ thuộc bên ngoài chưa sẵn sàng (nhà cung cấp SMS chưa ký hợp đồng), nên chưa thể Done. |
| 5 | Giao diện chủ đề Tết | 2 | Dù vừa khít chỗ trống 2 SP so với Velocity trung bình (18 − 16), PBI này không phục vụ Sprint Goal, và không được lấp đầy Sprint bằng việc ngoài Goal. |
| 6 | Chat với tài xế | 5 | Không phục vụ Sprint Goal; thêm vào sẽ đưa tổng lên 21 SP, vượt khoảng dự báo khi Capacity còn giảm. |

---

## Phần 3 – Phân rã PBI "Hoàn tiền vào ví khi chuyến bị hủy" (5 SP)

| Task | Nội dung | Đầu ra kiểm chứng được |
|------|----------|--------------------------|
| **Task 1** | Xây logic hoàn tiền: khi chuyến đã thanh toán bằng ví bị hủy, tạo giao dịch hoàn và cộng đúng số tiền vào ví; mỗi chuyến chỉ hoàn một lần. | Unit test pass cho 3 trường hợp: hoàn đúng số tiền vào ví, hủy lặp lại không hoàn lần hai, chuyến không thanh toán bằng ví không phát sinh hoàn tiền. |
| **Task 2** | Hiển thị trạng thái hoàn tiền cho khách: trên chi tiết chuyến bị hủy hiện "Đã hoàn tiền vào ví" kèm số tiền và gửi thông báo cho khách. | Demo trên môi trường test: sau khi hủy chuyến, màn hình chuyến bị hủy hiển thị đúng trạng thái và số tiền; thông báo được gửi tới tài khoản test. |
| **Task 3** | Kiểm thử tích hợp end-to-end với PBI 1: thanh toán bằng ví → hủy chuyến → hoàn tiền; kiểm tra cả trường hợp hoàn tiền lỗi phải ghi nhận để xử lý lại. | Integration Test pass, báo cáo kết quả kèm số dư ví trước/sau khớp; tất cả Acceptance Criteria và DoD của PBI được xác nhận. |

**Lưu ý phối hợp:** PBI này phụ thuộc PBI 1 chạy được, nên đội ưu tiên hoàn thiện PBI 1 sớm trong Sprint để Task 1–3 không bị chặn.
