# Phase 1 & 2 — Problem Scan và Quick Assessment

## Bài toán được chọn

**Vinhomes Resident Ticket Copilot:** Trợ lý AI phân loại, ưu tiên và soạn bản nháp phản hồi ticket cư dân.

> Các số liệu trong bài là giả định phục vụ scoping, chưa phải dữ liệu vận hành được Vinhomes xác nhận. Nhóm cần đo lại trên log CRM trước khi pilot.

---

# Phase 1 — SCAN

## Danh sách 5 bài toán

| # | Công ty | Lens | Mô tả ngắn |
|---:|---|---|---|
| 1 | Vinhomes | Lặp lại; tốn thời gian; AI-upgrade | Nhân viên đọc, phân loại, xác định ưu tiên và soạn phản hồi đầu tiên cho ticket cư dân. Nội dung thường tự do, thiếu thông tin và có lỗi chính tả. |
| 2 | VinFast | Lặp lại; stakeholder pain | Kỹ thuật viên hợp nhất log chẩn đoán, work order và lịch sử sửa chữa để viết biên bản bàn giao xe. |
| 3 | Vinmec | Tốn thời gian; stakeholder pain | Nhân viên hành chính trích xuất trường dữ liệu từ giấy chuyển viện/hồ sơ scan vào hệ thống bệnh án. |
| 4 | Vinpearl | AI-upgrade; stakeholder pain | Tổng hợp yêu cầu tự do của khách để đề xuất lịch trình phòng, ăn uống và hoạt động. |
| 5 | Xanh SM | Lặp lại; tốn thời gian | Phân loại phản ánh sau chuyến đi, phát hiện nội dung an toàn và chuyển đúng đội xử lý. |

## Ma trận sàng lọc

Thang điểm 1–5; rủi ro càng cao càng bất lợi.

| Bài toán | Tác động | Tần suất | Dữ liệu | AI fit | Rủi ro | Điểm ưu tiên* |
|---|---:|---:|---:|---:|---:|---:|
| Vinhomes ticket | 5 | 5 | 4 | 5 | 3 | **21** |
| Xanh SM phản ánh | 4 | 5 | 4 | 4 | 4 | 17 |
| VinFast tóm tắt log | 4 | 4 | 3 | 4 | 4 | 15 |
| Vinmec nhập hồ sơ | 5 | 4 | 3 | 3 | 5 | 15 |
| Vinpearl lịch trình | 3 | 4 | 4 | 4 | 3 | 15 |

\* Công thức: `2 × Tác động + Tần suất + Dữ liệu + AI fit − Rủi ro`.

---

# Phase 2 — Ba Quick Problem Cards

## Quick Problem Card 1 — Vinhomes

| Trường | Nội dung |
|---|---|
| Bài toán | Giảm thời gian xử lý ban đầu bằng cách để AI đề xuất phân loại, ưu tiên và bản nháp phản hồi ticket cư dân. |
| Công ty | **Vinhomes** |
| Actor | Nhân viên CSKH/ban quản lý tòa nhà; cư dân là người chịu tác động. |
| Workflow hiện tại | 1. Ticket vào app/email/call note → 2. Đọc và tra lịch sử → 3. Chọn nhóm/ưu tiên → 4. Soạn phản hồi → 5. Chuyển đội xử lý. |
| Bước chậm/lỗi nhất | Đọc ngữ cảnh, chọn nhóm và soạn phản hồi: giả định 11 phút/ticket; dễ phân loại không thống nhất. |
| AI hỗ trợ | Trích xuất thông tin, đề xuất category/priority, chỉ ra dữ liệu thiếu và soạn bản nháp dựa trên SOP. |
| Success metric | Median touch time 12 → **≤5 phút**; macro-F1 **≥0,85**; urgent recall **≥0,98**; 100% phản hồi được người duyệt. |
| Kiến trúc | **LLM feature + rule guardrails + HITL**. |

## Quick Problem Card 2 — Xanh SM

| Trường | Nội dung |
|---|---|
| Bài toán | Phân loại phản ánh sau chuyến đi và cảnh báo sớm nội dung liên quan an toàn. |
| Công ty | **Xanh SM** |
| Actor | Nhân viên Trust & Safety/CSKH; hành khách và tài xế. |
| Workflow hiện tại | 1. Nhận phản ánh → 2. Đọc lịch sử chuyến → 3. Gắn nhãn/mức độ → 4. Chuyển đơn vị → 5. Soạn xác nhận. |
| Bước chậm/lỗi nhất | Phân biệt phản ánh dịch vụ thường với sự cố an toàn: giả định 6 phút/lượt. |
| AI hỗ trợ | Trích xuất dấu hiệu an toàn và đề xuất tuyến xử lý; rule khẩn cấp chạy trước LLM. |
| Success metric | Recall case an toàn **≥0,995**; thời gian chuyển tuyến P95 **<2 phút**; critical false negative bằng 0 trong pilot. |
| Kiến trúc | **Rule + LLM + HITL**. |

## Quick Problem Card 3 — VinFast

| Trường | Nội dung |
|---|---|
| Bài toán | Tóm tắt log chẩn đoán và ghi chú sửa chữa thành biên bản bàn giao dễ đọc. |
| Công ty | **VinFast** |
| Actor | Kỹ thuật viên và cố vấn dịch vụ. |
| Workflow hiện tại | 1. Mở log DTC → 2. Đọc work order → 3. Đối chiếu lịch sử → 4. Viết tóm tắt → 5. Review. |
| Bước chậm/lỗi nhất | Hợp nhất dữ liệu nhiều nguồn và diễn giải mã kỹ thuật: giả định 15 phút/xe. |
| AI hỗ trợ | Tóm tắt có trích nguồn, chỉ ra mâu thuẫn/thiếu dữ liệu; không tự kết luận xe an toàn. |
| Success metric | Thời gian 15 → **≤6 phút**; factual consistency **≥0,95**; 100% mã DTC có nguồn. |
| Kiến trúc | **LLM/RAG + HITL**. |

## Stress-test và quyết định chọn bài toán

| Góc phản biện | Vinhomes | Xanh SM | VinFast |
|---|---|---|---|
| Điểm yếu logic | Chưa có volume và mức tái sử dụng mẫu thật. | Sự cố an toàn hiếm, khó đánh giá recall. | Log phân mảnh, quyền truy cập phức tạp. |
| Điểm yếu metric | Cần đo tỷ lệ sửa bản nháp và chất lượng. | “0 false negative” khó chứng minh với mẫu nhỏ. | Cần rubric chuyên gia cho factual consistency. |
| Phần rule làm tốt hơn | Từ khóa khẩn cấp, routing theo mã tòa và SLA. | Từ khóa/sự kiện khẩn cấp và khóa tài khoản. | Tra mã lỗi chuẩn và validate trường cố định. |
| Kết luận | **Chọn** — dữ liệu văn bản có khả năng dồi dào, rủi ro kiểm soát được bằng draft-only. | Để phase sau vì hậu quả false negative cao. | Để phase sau vì tích hợp khó. |

