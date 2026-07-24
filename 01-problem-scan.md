# 🔍 Phase 1 — SCAN & Phase 2 — QUICK-ASSESS (Báo cáo Cá nhân)

---

## 🏛️ Thông tin Học viên
* **Họ và tên:** Nguyễn Việt Thắng
* **Mã số sinh viên:** 2A202601321
* **Vai trò:** AI Product Engineer tại Vin Smart Future (Vingroup)

---

# 🔍 Phase 1 — SCAN: Danh sách Quét cơ hội AI cho Vingroup

Bảng tổng hợp 5 bài toán vận hành thủ công thuộc các công ty thành viên Vingroup được quét qua 4 Lenses:

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán & Con số thất thoát |
|---|----------------------------------|------|-----------------------------------------|
| 1 | **Xanh SM (GSM)** | Tốn thời gian | Điều phối viên xử lý thủ công sự cố sạc pin thực địa (mất 15 min/lượt, lãng phí 20h/ngày của team điều vận, rò rỉ 15% doanh thu cuốc). |
| 2 | **Vinhomes** | Lặp lại | Phân loại & điều hướng tự động phản ánh cư dân trên App Vinhomes Resident (trễ SLA 8-12 tiếng, phân loại sai bộ phận 12%). |
| 3 | **Vinmec** | Stakeholder Pain | Trích xuất dữ liệu bệnh án điện tử để soạn thảo tóm tắt xuất viện bằng ngôn ngữ dễ hiểu cho bệnh nhân (bác sĩ tốn 30 min/hồ sơ, chờ 3-4h). |
| 4 | **VinFast** | Lặp lại | Đối chiếu thủ công dữ liệu sạc điện từ trạm sạc đối tác nhượng quyền hằng tuần (tốn 3 ngày/tuần, sai lệch 2.5% tổng hóa đơn). |
| 5 | **Xanh SM (GSM)** | AI-upgrade | Phân tích tự động file ghi âm và note tài xế để lọc 10 lý do hủy cuốc thời gian thực (bỏ sót 95% dữ liệu hủy cuốc, churn rate 8-10%). |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Dưới đây là 3 thẻ bài toán tiềm năng nhất được lựa chọn từ danh sách trên để phân tích chi tiết:

---

### 🃏 QUICK PROBLEM CARD #1

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                   │
│                                                                         │
│ Bài toán: Điều phối viên Xanh SM xử lý sự cố hết pin / sạc pin khẩn     │
│ cấp thực địa của tài xế taxi điện.                                      │
│ Công ty thành viên: [x] Xanh SM (GSM)  [ ] VinFast  [ ] Vinhomes        │
│                                                                         │
│ Ai đang đau (Actor)? Tài xế (chờ đợi ngoài đường), Điều phối viên (quá  │
│ tải trong giờ cao điểm).                                                │
│                                                                         │
│ Workflow thủ công hiện tại (5 bước):                                    │
│   1. Tài xế gọi tổng đài điều vận báo sắp hết pin                       │
│   ──> 2. Điều phối viên mở bản đồ tra cứu vị trí định vị GPS của xe     │
│   ──> 3. Tra cứu thủ công Dashboard trạm sạc VinFast xem trụ trống      │
│   ──> 4. Soạn tin nhắn văn bản chỉ dẫn đường đi gửi qua App tài xế      │
│   ──> 5. Gọi điện liên hệ đội xe cứu hộ pin nếu xe dưới 5% pin          │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 10-12 phút/lượt)          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4                        │
│ (Tự động tra cứu trụ sạc tương thích -> Auto-draft SMS chỉ đường)       │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                   │
│   1. Giảm tổng thời gian xử lý sự cố pin từ 15 phút ──> dưới 3 phút.    │
│   2. Tỉ lệ gợi ý đúng trụ sạc phù hợp cổng sạc xe đạt 98%.              │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent    │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### 🃏 QUICK PROBLEM CARD #2

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                   │
│                                                                         │
│ Bài toán: Phân loại và điều hướng tự động ý kiến/phản ánh của cư dân    │
│ gửi qua App Vinhomes Resident về đúng Ban Quản Lý tòa nhà.              │
│ Công ty thành viên: [ ] Xanh SM  [ ] VinFast  [x] Vinhomes  [ ] Vinmec  │
│                                                                         │
│ Ai đang đau (Actor)? Cư dân (chờ phản hồi lâu), Nhân viên BQL (ngập     │
│ trong ticket thủ công).                                                 │
│                                                                         │
│ Workflow thủ công hiện tại (4 bước):                                    │
│   1. Cư dân gửi phản ánh bằng văn bản/hình ảnh lên App                 │
│   ──> 2. Nhân viên lễ tân/BQL đọc thủ công từng ticket                  │
│   ──> 3. Đánh nhãn thủ công loại sự cố (Nước/Điện/Bảo vệ/Hành chính)    │
│   ──> 4. Tạo công việc (ticket) và chuyển tiếp cho đội kỹ thuật phụ trách│
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 8 - 12 tiếng phân loại)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3                        │
│ (Tự động trích xuất ý định -> Đánh nhãn tag -> Điều hướng ticket)       │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                   │
│   1. Giảm thời gian phân loại và chuyển ticket từ 8 giờ ──> dưới 5 phút.│
│   2. Tỉ lệ điều hướng đúng bộ phận đạt trên 95%.                        │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent    │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### 🃏 QUICK PROBLEM CARD #3

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                   │
│                                                                         │
│ Bài toán: Tự động trích xuất dữ liệu EMR và soạn thảo bản tóm tắt hồ    │
│ sơ xuất viện (Discharge Summary) dễ hiểu cho bệnh nhân.                 │
│ Công ty thành viên: [ ] Xanh SM  [ ] VinFast  [ ] Vinhomes  [x] Vinmec  │
│                                                                         │
│ Ai đang đau (Actor)? Bác sĩ (quá tải hồ sơ giấy tờ), Bệnh nhân (mệt mỏi │
│ chờ làm thủ tục xuất viện).                                             │
│                                                                         │
│ Workflow thủ công hiện tại (4 bước):                                    │
│   1. Bác sĩ mở hồ sơ bệnh án điện tử (EMR) đọc diễn biến bệnh           │
│   ──> 2. Trích xuất chỉ số xét nghiệm, đơn thuốc và ghi chú điều trị    │
│   ──> 3. Tóm tắt thủ công quá trình điều trị bằng thuật ngữ chuyên môn   │
│   ──> 4. Diễn giải sang văn bản dặn dò tái khám/uống thuốc cho bệnh nhân │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 25 - 30 phút/hồ sơ)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4                        │
│ (Tự động trích xuất dữ liệu EMR -> Draft bản tóm tắt ngôn ngữ phổ thông)│
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                   │
│   1. Giảm thời gian soạn thảo tóm tắt xuất viện từ 30 min ──> under 5 min.│
│   2. 100% bản nháp AI đều qua Bác sĩ kiểm duyệt (HITL) trước khi in.    │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent    │
└─────────────────────────────────────────────────────────────────────────┘
```
