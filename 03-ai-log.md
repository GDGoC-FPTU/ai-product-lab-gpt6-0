# 03 — AI Log & Reflection

## 1. Tôi đã dùng AI như thế nào?

Tôi dùng AI như một **thought-partner** ở bốn việc:

1. Mở rộng danh sách pain point qua các mảng Vinhomes, Xanh SM, VinFast, Vinmec và Vinpearl.
2. So sánh rule-based, LLM feature và agentic loop cho từng phần của workflow.
3. Đóng vai CFO/Trưởng vận hành để phản biện metric, business case và ranh giới.
4. Soạn bản đầu của system prompt, JSON schema, adversarial tests và test harness.

AI không được dùng như nguồn dữ liệu nội bộ. Mọi volume, AHT và tỷ lệ lỗi trong bài là giả định scoping nếu không có log chứng minh.

## 2. AI giúp tốt ở đâu?

- AI giúp tạo nhiều phương án nhanh và chỉ ra rằng một bài toán không nhất thiết phải dùng “agent”; kiến trúc nhỏ hơn là rule + LLM + HITL phù hợp hơn.
- AI hữu ích khi biến yêu cầu mơ hồ “trả lời ticket nhanh hơn” thành metric kiểm thử được: macro-F1, urgent recall, groundedness, schema-valid rate, latency và draft acceptance.
- AI giúp nghĩ ra adversarial inputs mà nhóm dễ bỏ sót, như prompt injection nằm trong ticket, yêu cầu tiết lộ dữ liệu người khác và yêu cầu vô hiệu hóa an ninh.
- AI giúp chuẩn hóa đầu ra JSON để prototype có thể được đánh giá tự động, thay vì chỉ đọc cảm tính.

## 3. AI sai hoặc chưa đáng tin ở đâu?

- AI có xu hướng tạo các con số “nghe hợp lý” về số ticket, thời gian và tiết kiệm. Những con số này **không phải bằng chứng**. Tôi đã đổi chúng thành giả định và thêm bước đo baseline.
- AI ban đầu dễ đề xuất tự động route/gửi phản hồi để tối đa ROI. Điều đó vượt ranh giới an toàn của MVP, nên tôi giữ draft-only và bắt buộc human review.
- Một prompt nghiêm ngặt không đủ bảo đảm an toàn. Model vẫn có thể xuất JSON sai, nghe theo injection hoặc cam kết quá mức. Tôi bổ sung rule pre-check, schema validation và post-guardrails.
- Local deterministic test có thể pass toàn bộ nhưng không chứng minh Gemini sẽ pass. Đây chỉ là kiểm thử code/harness; cần chạy live và review output.
- AI có thể đánh giá chất lượng bản nháp quá lạc quan. Metric “accepted with light edit” cần định nghĩa rõ “light edit” và đo bằng log thao tác thực tế.

## 4. Tôi đã sửa gì sau khi phản biện?

| Đề xuất ban đầu | Chỉnh sửa của tôi | Lý do |
|---|---|---|
| Dùng agent đọc CRM, route và gửi phản hồi | LLM copilot chỉ tạo đề xuất/bản nháp | Giảm blast radius và quyền hệ thống. |
| Dùng LLM để phát hiện mọi case khẩn cấp | Rule khẩn cấp chạy trước LLM | Case safety cần cơ chế đơn giản, audit được và high recall. |
| Tính ROI trực tiếp từ số liệu ước tính | Gắn nhãn giả định, yêu cầu baseline tuần 0–2 | Không biến hallucinated numbers thành business case. |
| Chỉ đo accuracy chung | Dùng macro-F1, urgent recall và critical FN | Accuracy có thể che khuất nhóm hiếm nhưng nguy hiểm. |
| Prompt là lớp bảo vệ duy nhất | Thêm schema validator, post-check, HITL, fallback, kill switch | Defense in depth. |
| GO ngay toàn hệ thống | Pilot hẹp, shadow mode rồi copilot | Cho phép phát hiện lỗi trước khi ảnh hưởng cư dân. |

## 5. Reflection cá nhân

Bài học lớn nhất của tôi là **AI product scoping không bắt đầu bằng model** mà bắt đầu bằng workflow, actor, điểm handoff và hậu quả khi hệ thống sai. Bài toán Vinhomes có phần phù hợp với rule, phần phù hợp với LLM và phần bắt buộc để con người quyết định. Nếu gọi toàn bộ là “AI agent”, thiết kế sẽ vừa đắt vừa khó audit.

AI có giá trị nhất khi tôi buộc nó phản biện chính đề xuất của mình. Tuy vậy, tôi phải kiểm tra lại logic, tách fact khỏi assumption và chuyển các tuyên bố mơ hồ thành test có ngưỡng. Tôi chịu trách nhiệm cuối cùng về lựa chọn scope, metric và ranh giới; việc prototype chạy không đồng nghĩa sản phẩm đã sẵn sàng production.

## 6. Việc tiếp theo nếu có dữ liệu thật

1. Lấy mẫu 1.000 ticket đã ẩn danh và đo phân bố category/priority.
2. Đo AHT, first-response SLA, transfer rate và edit distance của bản nháp.
3. Xây gold set có hai annotator và supervisor phân xử.
4. Chạy `python prompt_prototype.py --live --show-json` với Gemini 2.5 Flash.
5. Lưu raw output, chấm từng slice và cập nhật prompt/guardrails dựa trên lỗi quan sát được.

