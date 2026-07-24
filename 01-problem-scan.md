# Lab 02 — AI Product Scoping: Problem Scan & Quick Cards
## Vin Smart Future | Nguyễn Quốc Việt — 2A202601737

---

## 🏛️ Bối cảnh: Tôi là ai?

Tôi là **Việt**, AI Engineer tại **Vin Smart Future** — đơn vị công nghệ thống nhất mới thành lập của Vingroup. Nhiệm vụ của tôi là quét qua hoạt động vận hành của các công ty thành viên (VinFast, Xanh SM, Vinhomes, Vinmec, Vinpearl) để tìm ra những bài toán thực tế có thể tối ưu bằng AI.

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của các công ty thành viên Vingroup.

### 📝 Danh sách bài toán:

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Điều phối viên xử lý thủ công sự cố hết pin/sạc khẩn cấp của tài xế taxi điện giữa đường — mỗi lượt mất 15 phút tra cứu trạm sạc trống, soạn tin chỉ dẫn. |
| 2 | **VinFast** | Lặp lại | Đối chiếu thủ công hóa đơn sạc điện hằng tuần với hàng nghìn trạm sạc đối tác — team tài chính mất 2 ngày/tuần cho việc so khớp. |
| 3 | **Vinhomes** | AI có thể tốt hơn | Phân loại & điều hướng phản ánh/khiếu nại của cư dân qua App Vinhomes Resident — hiện CSKH xử lý thủ công, phản hồi rập khuôn, mất 12 tiếng. |
| 4 | **Vinmec** | Tốn thời gian | Bác sĩ mất 20-30 phút/bệnh nhân để viết tóm tắt hồ sơ xuất viện (Discharge Summary) từ bệnh án điện tử và ghi chú lâm sàng. |
| 5 | **Vinpearl** | Pain từ người khác | Quét phân tích review khách sạn trên Booking.com, Agoda, Google Maps — lọc ra các phàn nàn khẩn cấp gửi Manager để xử lý kịp thời. |
| 6 | **Xanh SM** | Lặp lại | Tổng hợp lý do khách hủy chuyến từ ghi âm cuộc gọi & ghi chú tài xế — hiện đội vận hành phải nghe lại từng cuộc gọi thủ công (3-5 phút/cuộc). |
| 7 | **VinFast** | AI có thể tốt hơn | Trợ lý chẩn đoán lỗi xe từ mô tả tiếng Việt của khách hàng — khách tả "xe kêu cụp cụp bánh trước", hệ thống phân loại mã lỗi kỹ thuật ban đầu. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN: **#1 (Xanh SM Sự cố pin), #3 (Vinhomes Phản ánh cư dân), #4 (Vinmec Tóm tắt xuất viện).**

---

## Quick Problem Card #1 — Xanh SM: Xử lý sự cố hết pin thực địa

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo cáo sự cố hết pin / sạc pin   │
│ khẩn cấp giữa đường cần điều phối trạm sạc gần nhất hoặc   │
│ xe cứu hộ pin di động.                                      │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau (Actor)? Tài xế (chờ đợi, mất doanh thu),      │
│ Điều phối viên (quá tải giờ cao điểm).                      │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài điều vận báo hết pin              │
│   → 2. Điều phối viên tra cứu thủ công vị trí xe trên bản đồ│
│   → 3. Tra cứu thủ công trạm sạc VinFast còn trụ trống     │
│   → 4. Viết tin nhắn chỉ dẫn/đường đi gửi qua App tài xế   │
│   → 5. Liên hệ đội xe cứu hộ nếu xe đã cạn kiệt pin        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 12 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (Tự động tra cứu vị trí → Trạm sạc trống → Draft tin nhắn) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian xử lý sự cố từ 15 phút → dưới 3 phút.       │
│ Tỉ lệ hướng dẫn đúng trạm sạc phù hợp đạt 98%.             │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (Tự động tra cứu API + soạn draft, dispatcher duyệt & gửi) │
└─────────────────────────────────────────────────────────────┘
```

---

## Quick Problem Card #2 — Vinhomes: Phân loại & Điều hướng phản ánh cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Phân loại tự động các phản ánh/khiếu nại của cư   │
│ dân gửi qua App Vinhomes Resident để điều hướng đến đúng    │
│ ban quản lý từng tòa nhà.                                   │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH Vinhomes (quá tải),     │
│ Cư dân (chờ phản hồi lâu, 12 tiếng).                        │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh qua App (mất nước, hỏng đèn, ồn…)│
│   → 2. CSKH đọc & phân loại thủ công từng phản ánh          │
│   → 3. Chuyển tiếp đến ban quản lý tòa nhà tương ứng        │
│   → 4. Ban quản lý xử lý & phản hồi lại cư dân             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (⏱ 5-10 phút/       │
│ phản ánh, ~200 phản ánh/ngày tại khu đô thị lớn).          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2               │
│ (Tự động phân loại: khẩn cấp/thường, loại sự cố, tòa nhà)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian phân loại từ 10 phút → dưới 30 giây/phản ánh│
│ Tỉ lệ phân loại đúng ban quản lý đạt 95%.                  │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (LLM phân loại + route tự động, CSKH xác nhận trước gửi)   │
└─────────────────────────────────────────────────────────────┘
```

---

## Quick Problem Card #3 — Vinmec: Soạn thảo tóm tắt hồ sơ xuất viện

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Trích xuất thông tin lâm sàng từ bệnh án điện tử, │
│ xét nghiệm và ghi chú của bác sĩ để soạn thảo bản tóm tắt  │
│ xuất viện (Discharge Summary) bằng ngôn ngữ dễ hiểu.        │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ (quá tải, 20-30 phút/bệnh nhân),│
│ Bệnh nhân (chờ đợi lâu, khó hiểu thuật ngữ chuyên môn).    │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bác sĩ xem lại toàn bộ bệnh án điện tử của bệnh nhân   │
│   → 2. Đọc & tổng hợp kết quả xét nghiệm, chẩn đoán hình ảnh│
│   → 3. Viết tóm tắt xuất viện: chẩn đoán, điều trị, dặn dò │
│   → 4. Bệnh nhân/người nhà ký xác nhận                      │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1-3 (⏱ 20-30 phút/    │
│ bệnh nhân, bác sĩ phàn nàn vì quá tải cuối ngày).          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-2-3           │
│ (Tự động trích xuất → Soạn draft tóm tắt → Bác sĩ duyệt)   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian soạn thảo từ 25 phút → dưới 5 phút/BN.      │
│ Tỉ lệ thông tin lâm sàng chính xác đạt 99% sau duyệt BS.   │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (LLM đọc bệnh án → Draft tóm tắt, Bác sĩ HITL duyệt cuối) │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Quyết định lựa chọn của cá nhân:

Tôi chọn bài toán **Card #1 — Xanh SM: Xử lý sự cố hết pin thực địa** để thực hiện Deep-Dive.

### Lý do:
- **Tác động trực tiếp đến doanh thu và an toàn:** Xe cạn pin giữa đường không chỉ mất doanh thu tài xế mà còn gây tắc nghẽn giao thông và rủi ro an toàn.
- **Metric rõ ràng:** Có con số cụ thể (15 phút → dưới 3 phút), dễ đo lường hiệu quả sau triển khai.
- **Phù hợp LLM Feature:** Không cần Agent phức tạp — quy trình có cấu trúc cố định, LLM chỉ cần tra cứu API và soạn văn bản.
- **Ranh giới an toàn rõ ràng:** Human-in-the-loop bắt buộc trước khi gửi tin cho tài xế, tránh rủi ro AI gửi sai chỉ dẫn gây nguy hiểm.
