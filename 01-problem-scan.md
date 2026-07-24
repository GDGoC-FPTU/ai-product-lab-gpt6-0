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

## Quick Problem Card 1

### Tên bài toán

**AI Medical Record Assistant**

**Công ty:** Vinmec

### Actor (Người gặp khó khăn)

Bác sĩ khám bệnh

### Quy trình thủ công hiện tại

```text
Bệnh nhân đến khám
        ↓
Mở hồ sơ bệnh án
        ↓
Đọc tiền sử bệnh
        ↓
Đọc kết quả xét nghiệm
        ↓
Đọc đơn thuốc cũ
        ↓
Tự tóm tắt thông tin
        ↓
Bắt đầu khám
```

### Bottleneck

- Đọc hồ sơ bệnh án mất khoảng **8–15 phút/bệnh nhân**.
- Hồ sơ có thể dài từ **20–100 trang**.
- Dễ bỏ sót các thông tin quan trọng như dị ứng thuốc, bệnh nền hoặc kết quả xét nghiệm bất thường.

### AI tham gia

- Đọc toàn bộ hồ sơ bệnh án.
- Tự động tóm tắt các thông tin quan trọng.
- Highlight các chỉ số bất thường.
- Trả lời câu hỏi của bác sĩ dựa trên dữ liệu bệnh án.

### Success Metrics

- Giảm thời gian đọc hồ sơ từ **10 phút xuống dưới 2 phút**.
- Độ chính xác của bản tóm tắt đạt **≥95%**.
- Giảm **80%** thời gian tìm kiếm thông tin trong hồ sơ.

### Đề xuất kiến trúc

**LLM + RAG**

---

## Quick Problem Card 2

### Tên bài toán

**AI Clinical Documentation Assistant**

**Công ty:** Vinmec

### Actor

Điều dưỡng

### Quy trình thủ công hiện tại

```text
Khám bệnh
    ↓
Ghi chú giấy
    ↓
Nhập vào HIS/EMR
    ↓
Kiểm tra lại
    ↓
Lưu hồ sơ
```

### Bottleneck

- Nhập liệu mất khoảng **5–8 phút/bệnh nhân**.
- Dễ xảy ra lỗi nhập liệu hoặc thiếu thông tin.
- Khối lượng công việc lớn vào giờ cao điểm.

### AI tham gia

- Chuyển giọng nói thành văn bản (Speech-to-Text).
- Tự động tạo bệnh án theo mẫu.
- Điều dưỡng chỉ cần kiểm tra và xác nhận.

### Success Metrics

- Giảm thời gian nhập liệu từ **7 phút xuống dưới 2 phút**.
- Giảm lỗi nhập liệu **≥70%**.
- Tăng năng suất xử lý hồ sơ **≥40%**.

### Đề xuất kiến trúc

**Speech-to-Text + LLM**

---

## Quick Problem Card 3

### Tên bài toán

**Medical Guideline Assistant**

**Công ty:** Vinmec

### Actor

Bác sĩ

### Quy trình thủ công hiện tại

```text
Có ca bệnh
      ↓
Google tài liệu
      ↓
Tìm guideline
      ↓
Đọc file PDF
      ↓
Đối chiếu với bệnh án
      ↓
Ra quyết định
```

### Bottleneck

- Mỗi lần tra cứu mất khoảng **10–20 phút**.
- Có nhiều nguồn tài liệu khác nhau (WHO, Bộ Y tế, ESC, ACC...).
- Dễ sử dụng tài liệu cũ hoặc bỏ sót hướng dẫn mới.

### AI tham gia

- Tự động tìm guideline phù hợp.
- Đọc và tóm tắt tài liệu.
- Trích dẫn nguồn.
- So sánh với hồ sơ bệnh nhân.

### Success Metrics

- Giảm thời gian tra cứu từ **15 phút xuống dưới 1 phút**.
- **100%** câu trả lời có nguồn trích dẫn.
- Tỷ lệ tìm đúng tài liệu **≥95%**.

### Đề xuất kiến trúc

**Agent + RAG + Search**

---

# Thống kê tổn thất ước tính

| Pain Point | Thời gian hiện tại | Sau AI | Tổn thất ước tính |
|------------|-------------------|---------|-------------------|
| Đọc hồ sơ bệnh án | 8–15 phút/hồ sơ | <2 phút | Với 200 hồ sơ/ngày có thể tiết kiệm khoảng **20–43 giờ làm việc/ngày**. |
| Nhập liệu bệnh án | 5–8 phút/hồ sơ | <2 phút | Với 300 hồ sơ/ngày có thể tiết kiệm khoảng **15–30 giờ làm việc/ngày**. |
| Tra cứu guideline | 10–20 phút/lần | <1 phút | Nếu có 50 lượt tra cứu/ngày có thể tiết kiệm khoảng **7,5–16 giờ/ngày**. |
| Trả lời câu hỏi lặp lại của bệnh nhân | 3–5 phút/lượt | <30 giây | Giảm **60–80%** khối lượng công việc của tổng đài. |
| Tổng hợp kết quả xét nghiệm | 5–10 phút/ca | <1 phút | Giảm đáng kể thời gian chuẩn bị trước khi bác sĩ khám. |

---

# Đề xuất bài toán ưu tiên

Bài toán phù hợp nhất để phát triển trong các giai đoạn tiếp theo là **AI Medical Record Assistant** vì:

- Giải quyết đúng pain point lớn của bác sĩ.
- Có thể áp dụng **LLM + RAG** hiệu quả.
- Có metric đánh giá rõ ràng.
- Dễ thiết kế Operational Boundary, Human-in-the-loop và Prompt Defense.
- Phù hợp để phát triển thành AI Agent trong môi trường doanh nghiệp.