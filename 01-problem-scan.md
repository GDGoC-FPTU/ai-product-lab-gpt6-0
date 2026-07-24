# 01 - Problem Scan

**Lab:** AI Product Scoping - Vin Smart Future  
**Người thực hiện:** Nguyễn Chí Hiếu  
**MSSV:** 2A202601931  

---

## Phase 1 - SCAN: Tìm kiếm cơ hội AI

Dùng 4 lenses để quét các quy trình vận hành của các công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | Xanh SM | Stakeholder Pain | Tài xế xe điện gặp tình huống pin rất thấp khi đang trên đường đón/trả khách, phải gọi tổng đài để hỏi trạm sạc hoặc yêu cầu xe sạc lưu động. Điều phối viên xử lý thủ công chậm và dễ gợi ý trạm quá xa. |
| 2 | Xanh SM | Tốn thời gian | Điều phối viên phải đọc tin nhắn, ghi chú tài xế và tọa độ GPS để sửa điểm đón sai trong giờ cao điểm, làm tăng thời gian khách chờ xe. |
| 3 | VinFast | AI-upgrade | Khách hàng mô tả lỗi xe bằng ngôn ngữ tự nhiên như "xe kêu cục cục ở bánh trước"; nhân viên CSKH phải tự đọc, hỏi lại nhiều lần và phân loại mã lỗi ban đầu. |
| 4 | Vinhomes | Lặp lại | Ban quản lý phải phân loại hàng trăm phản ánh cư dân trên app mỗi ngày, sau đó route thủ công đến đội kỹ thuật, vệ sinh, an ninh hoặc chăm sóc khách hàng. |
| 5 | Vinpearl | Tốn thời gian | Nhân viên booking phải đọc email đặt phòng theo đoàn với nhiều điều kiện phòng, ngày, số lượng trẻ em, dịch vụ kèm theo và tự kiểm tra phòng trống. |
| 6 | Vinmec | Tốn thời gian | Bác sĩ phải viết tóm tắt hồ sơ xuất viện từ bệnh án, xét nghiệm và ghi chú điều trị, mất nhiều thời gian cho mỗi bệnh nhân. |

**Ghi chú về độ tin cậy của số liệu:** Các mốc thời gian, tỷ lệ lỗi và ngưỡng mục tiêu trong ba Quick Problem Cards dưới đây là **ước tính ban đầu phục vụ scoping**, chưa phải số liệu vận hành đã được Vingroup xác nhận. Trước khi quyết định GO, nhóm cần kiểm chứng bằng log/ticket thực tế và phỏng vấn người vận hành.

---

## Phase 2 - QUICK-ASSESS: 3 Quick Problem Cards

Chọn 3 bài toán tiềm năng nhất từ danh sách scan:

1. Xanh SM - Xử lý sự cố pin thấp và điều phối sạc khẩn cấp.
2. Vinhomes - Phân loại và điều hướng phản ánh cư dân.
3. VinFast - Phân loại lỗi xe từ mô tả tiếng Việt của khách hàng.

---

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                        │
│                                                                              │
│ Bài toán (1 câu): Tài xế Xanh SM báo pin rất thấp khi đang vận hành,         │
│ điều phối viên mất nhiều thời gian để xác định phương án sạc an toàn.        │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes                   │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)                           │
│                                                                              │
│ Ai đang đau (Actor)? Tài xế, điều phối viên và khách đang chờ chuyến.        │
│                                                                              │
│ Workflow thủ công hiện tại (3-5 bước):                                       │
│   1. Tài xế báo pin/vị trí --> 2. Điều phối viên kiểm tra bản đồ             │
│   3. Tra cứu trạm sạc      --> 4. Chọn phương án xử lý                       │
│   5. Soạn hướng dẫn hoặc liên hệ đội xe sạc/cứu hộ                           │
│                                                                              │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (10-15 phút/lượt)                  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-5                               │
│                                                                              │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   Giảm thời gian xử lý từ 15 phút xuống dưới 3 phút/lượt;                    │
│   95% ca pin dưới 5% không bị gợi ý trạm xa hơn 5 km.                        │
│                                                                              │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent                  │
└──────────────────────────────────────────────────────────────────────────────┘
```

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                        │
│                                                                              │
│ Bài toán (1 câu): Phản ánh cư dân trên ứng dụng Vinhomes Resident bị         │
│ phân loại chậm hoặc chuyển sai bộ phận, làm tăng thời gian xử lý.            │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes                   │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)                           │
│                                                                              │
│ Ai đang đau (Actor)? Cư dân, CSKH và các đội vận hành tòa nhà.               │
│                                                                              │
│ Workflow thủ công hiện tại (3-5 bước):                                       │
│   1. Cư dân gửi phản ánh   --> 2. CSKH đọc và phân loại                      │
│   3. Gán ticket cho bộ phận --> 4. Yêu cầu bổ sung thông tin                 │
│   5. Cập nhật trạng thái và phản hồi cho cư dân                              │
│                                                                              │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (6-10 phút/ticket)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4                               │
│                                                                              │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   85% ticket được phân loại đúng trong dưới 10 giây;                         │
│   giảm tỷ lệ chuyển sai bộ phận từ 18% xuống dưới 5%.                        │
│                                                                              │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent                  │
└──────────────────────────────────────────────────────────────────────────────┘
```

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                        │
│                                                                              │
│ Bài toán (1 câu): Khách hàng VinFast mô tả triệu chứng xe bằng tiếng         │
│ Việt đời thường, khiến CSKH mất thời gian hỏi lại và phân loại lỗi.          │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes                   │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)                           │
│                                                                              │
│ Ai đang đau (Actor)? Khách hàng, CSKH, cố vấn dịch vụ, kỹ thuật viên.        │
│                                                                              │
│ Workflow thủ công hiện tại (3-5 bước):                                       │
│   1. Khách mô tả triệu chứng --> 2. CSKH hỏi thông tin bổ sung               │
│   3. Gán nhóm lỗi ban đầu     --> 4. Chuyển ticket cho kỹ thuật              │
│   5. Liên hệ lại nếu ticket còn thiếu thông tin                              │
│                                                                              │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (8-12 phút/ticket)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4                               │
│                                                                              │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   Giảm thời gian tạo ticket từ 12 phút xuống dưới 4 phút;                    │
│   80% ticket đủ thông tin trước khi chuyển cho cố vấn dịch vụ.               │
│                                                                              │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent                  │
└──────────────────────────────────────────────────────────────────────────────┘
```
