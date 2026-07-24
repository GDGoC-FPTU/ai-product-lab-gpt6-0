# 01-problem-scan.md

# Phase 1 - SCAN

## Bảng quét cơ hội

| STT | Công ty | Bài toán | Thấu kính | Vì sao là cơ hội AI? |
|-----|----------|----------|------------|----------------------|
| 1 | Vinmec | AI hỗ trợ tóm tắt hồ sơ bệnh án trước khi bác sĩ khám | Tốn thời gian | Hồ sơ bệnh án dài, bác sĩ mất nhiều thời gian đọc và dễ bỏ sót thông tin quan trọng. |
| 2 | Vinmec | AI tự động nhập bệnh án từ ghi chú hoặc giọng nói | Lặp lại | Điều dưỡng phải nhập dữ liệu thủ công vào hệ thống HIS/EMR, dễ sai sót và mất thời gian. |
| 3 | VinFast | AI phân loại và gợi ý nguyên nhân lỗi bảo hành xe | AI-upgrade | Kỹ thuật viên phải đọc mô tả lỗi và tra cứu tài liệu kỹ thuật thủ công. |
| 4 | Xanh SM | AI hỗ trợ tổng đài xử lý khiếu nại khách hàng | Stakeholder Pain | Nhân viên CSKH phải tra cứu nhiều hệ thống để trả lời khách hàng, làm tăng thời gian chờ. |
| 5 | Vinhomes | AI Chatbot hỗ trợ cư dân tra cứu thông tin và quy định | Lặp lại | Ban quản lý phải trả lời nhiều câu hỏi lặp lại về phí dịch vụ, tiện ích, nội quy,... |

---

# Phase 2 - QUICK ASSESS

# QUICK PROBLEM CARD #1

### Bài toán (1 câu)

AI hỗ trợ bác sĩ tự động đọc, tóm tắt và phân tích hồ sơ bệnh án nhằm giảm thời gian chuẩn bị trước khi khám bệnh.

### Công ty thành viên

- [ ] VinFast
- [ ] Xanh SM
- [ ] Vinhomes
- [x] Vinmec
- [ ] Khác: _______________________

---

### Ai đang đau (Actor)?

**Bác sĩ khám bệnh (Internal User)**

Đối với các bệnh nhân tái khám hoặc có bệnh nền, hồ sơ bệnh án thường rất dài (20–100 trang), bao gồm:

- Tiền sử bệnh
- Kết quả xét nghiệm
- Đơn thuốc
- Chẩn đoán cũ
- Kết quả chẩn đoán hình ảnh
- Ghi chú của nhiều bác sĩ

Trước khi bắt đầu khám, bác sĩ phải tự đọc toàn bộ hồ sơ để nắm được tình trạng bệnh nhân.

Đây là công việc lặp đi lặp lại, tiêu tốn nhiều thời gian nhưng không tạo ra giá trị chuyên môn.

---

### Workflow thủ công hiện tại (3–5 bước)

1. Bác sĩ mở hồ sơ bệnh án trên hệ thống HIS/EMR
      ↓
2. Đọc tiền sử bệnh, kết quả xét nghiệm và đơn thuốc cũ
      ↓
3. Tự ghi nhớ và tổng hợp các thông tin quan trọng
      ↓
4. Bắt đầu khám và hỏi bệnh nhân

---

### Bước nào tốn thời gian/lỗi nhất?

**Bước 2: Đọc và tổng hợp hồ sơ bệnh án**

⏱ **8–15 phút/lượt khám**

Các vấn đề gặp phải:

- Hồ sơ rất dài.
- Có nhiều dữ liệu không liên quan.
- Dễ bỏ sót dị ứng thuốc.
- Dễ bỏ sót bệnh nền.
- Dễ quên các chỉ số xét nghiệm bất thường.
- Áp lực khi số lượng bệnh nhân đông.

---

### AI có thể nhảy vào hỗ trợ ở bước nào?

AI tham gia ngay sau khi bác sĩ mở hồ sơ.

AI sẽ:

- Đọc toàn bộ hồ sơ bệnh án.
- Tóm tắt trong khoảng 5–10 dòng.
- Liệt kê bệnh nền.
- Liệt kê thuốc đang sử dụng.
- Highlight các chỉ số bất thường.
- So sánh với lần khám gần nhất.
- Trả lời câu hỏi của bác sĩ như:
  - "Lần gần nhất HbA1c là bao nhiêu?"
  - "Bệnh nhân có tiền sử dị ứng thuốc không?"
  - "Creatinine có tăng so với lần trước không?"

Bác sĩ chỉ cần kiểm tra kết quả trước khi khám.

---

### Đo thành công bằng gì (Metric có số)?

| Metric | Hiện tại | Mục tiêu |
|---------|-----------|-----------|
| Thời gian đọc hồ sơ | 10 phút | < 2 phút |
| Thời gian tìm kết quả xét nghiệm | 3 phút | < 30 giây |
| Độ chính xác bản tóm tắt | - | ≥95% |
| Tỷ lệ bỏ sót thông tin quan trọng | ~10% | <2% |
| Mức hài lòng của bác sĩ | - | >90% |

---

### Quick Architecture

- [ ] No AI
- [ ] Rule
- [x] LLM
- [ ] Agent

Lý do:

Bài toán yêu cầu AI hiểu ngữ cảnh, đọc tài liệu dài, tổng hợp thông tin và trả lời bằng ngôn ngữ tự nhiên. Đây là thế mạnh của LLM kết hợp RAG.

# QUICK PROBLEM CARD #2

### Bài toán (1 câu)

AI tự động tạo bệnh án điện tử từ cuộc hội thoại giữa bác sĩ và bệnh nhân.

### Công ty thành viên

- [ ] VinFast
- [ ] Xanh SM
- [ ] Vinhomes
- [x] Vinmec
- [ ] Khác: _______________________

---

### Ai đang đau (Actor)?

Điều dưỡng và bác sĩ.

Sau khi khám xong, bác sĩ hoặc điều dưỡng phải nhập lại toàn bộ thông tin khám vào hệ thống HIS.

Việc nhập liệu vừa mất thời gian vừa dễ sai sót.

---

### Workflow thủ công hiện tại

1. Khám bệnh
      ↓
2. Ghi chú nhanh bằng giấy hoặc máy tính
      ↓
3. Nhập lại vào HIS
      ↓
4. Kiểm tra lỗi
      ↓
5. Lưu hồ sơ

---

### Bước nào tốn thời gian/lỗi nhất?

Bước 3 - Nhập bệnh án

⏱ 5–8 phút/lượt

Lỗi thường gặp:

- Sai chính tả
- Thiếu thông tin
- Nhập nhầm thuốc
- Quên nhập kết quả khám

---

### AI có thể nhảy vào hỗ trợ ở bước nào?

Sau khi bác sĩ khám.

AI sẽ:

- Speech-to-Text
- Chuẩn hóa thuật ngữ y khoa
- Sinh bệnh án theo mẫu chuẩn
- Điều dưỡng chỉ cần xác nhận trước khi lưu.

---

### Đo thành công bằng gì?

- Giảm thời gian nhập liệu từ **7 phút → dưới 2 phút**
- Giảm lỗi nhập liệu **70%**
- Tăng số bệnh nhân xử lý mỗi ca **30%**

---

### Quick Architecture

- [ ] No AI
- [ ] Rule
- [x] LLM
- [ ] Agent

# QUICK PROBLEM CARD #3

### Bài toán (1 câu)

AI Agent hỗ trợ bác sĩ tìm kiếm và đối chiếu hướng dẫn điều trị (Clinical Guideline) phù hợp với từng bệnh nhân.

### Công ty thành viên

- [ ] VinFast
- [ ] Xanh SM
- [ ] Vinhomes
- [x] Vinmec
- [ ] Khác: _______________________

---

### Ai đang đau (Actor)?

Bác sĩ chuyên khoa.

Đối với các ca bệnh phức tạp, bác sĩ cần tra cứu guideline từ WHO, Bộ Y tế, ESC, ACC... trước khi đưa ra quyết định.

Việc tìm kiếm và đọc tài liệu rất mất thời gian.

---

### Workflow thủ công hiện tại

1. Xem hồ sơ bệnh nhân
      ↓
2. Google guideline
      ↓
3. Đọc PDF
      ↓
4. So sánh với bệnh nhân
      ↓
5. Ra quyết định

---

### Bước nào tốn thời gian/lỗi nhất?

Bước 2 và bước 3

⏱ 10–20 phút/lần

Khó khăn:

- Có nhiều phiên bản guideline.
- Tài liệu dài hàng trăm trang.
- Dễ sử dụng tài liệu cũ.

---

### AI có thể nhảy vào hỗ trợ ở bước nào?

Sau khi bác sĩ chọn bệnh nhân.

AI Agent sẽ:

- Tự tìm guideline mới nhất.
- Đọc tài liệu.
- Trích dẫn nguồn.
- So sánh với hồ sơ bệnh nhân.
- Sinh báo cáo tóm tắt để bác sĩ tham khảo.

---

### Đo thành công bằng gì?

- Giảm thời gian tra cứu từ **15 phút → dưới 1 phút**
- **100%** câu trả lời có nguồn trích dẫn
- Độ chính xác tìm tài liệu **≥95%**

---

### Quick Architecture

- [ ] No AI
- [ ] Rule
- [ ] LLM
- [x] Agent