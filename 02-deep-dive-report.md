# 02 - Báo cáo Problem Deep-Dive

**Lab:** AI Product Scoping - Vin Smart Future

**Tên nhóm:** GPT6-0

**Bài toán được chọn:** Xanh SM - Hỗ trợ điều phối xử lý tình huống pin thấp

**Ngày:** 24/07/2026

## Thành viên

| Họ và tên | MSSV |
|---|---|
| Nguyễn Chí Hiếu | 2A202601931 |
| Nguyễn Minh Phương | 2A202601947 |
| Đỗ Duy Đông | 2A202601657 |
| Lại Thế Rin | 2A202601665 |
| Nguyễn Quốc Việt | 2A202601737 |
| Nguyễn Việt Thắng | 2A202601321 |

> Danh sách trên được tổng hợp từ tên nhánh cá nhân. Nhóm cần đối chiếu lại họ tên và MSSV trước khi merge vào `main`.

---

## Quyết định lựa chọn

Sau khi so sánh các problem scan của thành viên, nhóm chọn:

**Xanh SM - Hỗ trợ điều phối xử lý tình huống pin thấp cho tài xế taxi điện.**

| Ứng viên | Giá trị | Khả năng prototype | Rủi ro | Quyết định |
|---|---|---|---|---|
| Xanh SM - Xử lý pin thấp | Tác động trực tiếp đến an toàn và thời gian xe ngừng hoạt động | Cao; starter code và boundary test đã có | Cao nếu chỉ đường sai, nhưng có thể giảm bằng rule và HITL | **Chọn** |
| Vinhomes - Phân loại phản ánh | Khối lượng ticket lớn, metric rõ | Rất cao | Có thể kiểm soát | Giữ làm phương án dự phòng |
| Xanh SM - Complaint Copilot | Giá trị vận hành lớn, LLM fit tốt | Trung bình; cần tích hợp nhiều nguồn dữ liệu | Liên quan chính sách và bồi hoàn | Chưa chọn cho lab |
| Vinmec - Tóm tắt xuất viện | Giá trị cao | Có thể làm bản nháp | Rủi ro lâm sàng và dữ liệu nhạy cảm rất cao | NOT YET |

Lý do chọn:

1. Có actor, tình huống kích hoạt và ranh giới an toàn rõ.
2. Có thể tách quyết định cứng sang rule/API, chỉ dùng LLM cho phần ngôn ngữ.
3. Có metric kiểm thử kỹ thuật rõ ràng.
4. Đồng nhất với prompt prototype và autograder của repo.

---

# Phase 3 - DEEP-DIVE

## 3.1. Current-State Workflow Mapping

```text
Tài xế báo mức pin, vị trí, trạng thái chuyến
        |
        | Handoff 1: Tài xế -> Điều phối viên
        v
Điều phối viên ghi nhận thông tin              (ước tính 1-2 phút)
        |
        | Handoff 2: Tổng đài -> Bản đồ nội bộ
        v
Tra cứu vị trí GPS và thông tin xe             (ước tính 1-2 phút)
        |
        | Handoff 3: Bản đồ -> Hệ thống trạm sạc
        v
Tra cứu trạm, khoảng cách, cổng và chỗ trống   (ước tính 3-5 phút) [BOTTLENECK]
        |
        | Handoff 4: Dữ liệu trạm -> Điều phối viên
        v
So sánh phương án và soạn hướng dẫn            (ước tính 3-5 phút) [BOTTLENECK]
        |
        | Handoff 5: Điều phối viên -> Tài xế/đội hỗ trợ
        v
Gửi hướng dẫn hoặc liên hệ hỗ trợ sạc/cứu hộ   (ước tính 1 phút)
```

**Tổng thời gian giả định ban đầu:** 9-15 phút/lượt.

Các mốc thời gian trên là giả định scoping, chưa phải dữ liệu đã được Xanh SM xác nhận. File `04-workflow-diagram.png` trực quan hóa quy trình này; trước pilot cần đo median và P90 từ log thực tế.

## 3.2. Problem Statement 6-field

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Điều phối viên Xanh SM tiếp nhận yêu cầu hỗ trợ pin thấp; tài xế là người cung cấp tình trạng xe và nhận hướng dẫn sau khi điều phối viên phê duyệt. |
| **2. Current Workflow** | Điều phối viên ghi nhận mức pin và vị trí, mở bản đồ, tra cứu trạm sạc, kiểm tra khoảng cách/tương thích/khả dụng, chọn phương án rồi soạn hướng dẫn hoặc liên hệ đội hỗ trợ. |
| **3. Bottleneck** | Tra cứu và đối chiếu dữ liệu giữa nhiều màn hình, sau đó soạn hướng dẫn trong tình huống có áp lực thời gian. Dữ liệu trạm thay đổi nhanh làm tăng nguy cơ dùng thông tin cũ. |
| **4. Business Impact** | Xe ngừng nhận chuyến lâu hơn; tài xế và khách phải chờ; điều phối viên bị chiếm thời gian; chỉ dẫn sai có thể khiến xe cạn pin giữa đường. Quy mô tài chính chưa thể kết luận khi chưa có baseline. |
| **5. Success Metric** | Sau khi có baseline: giảm median thời gian xử lý ít nhất 60%; 100% ca pin dưới 5% trong bộ test không đề xuất trạm xa hơn 5 km; 100% output có `[DRAFT_ONLY]`; ít nhất 80% draft được điều phối viên chấp nhận sau chỉnh sửa nhỏ. |
| **6. Operational Boundary** | Rule cứng quyết định ngưỡng pin, khoảng cách và tương thích cổng. LLM chỉ tóm tắt và soạn draft. Không tự gửi tin, không tự điều xe, không thay đổi chuyến. Mọi hành động cần điều phối viên phê duyệt. |

## 3.3. AI Fit

| Thành phần | Nên dùng | Lý do |
|---|---|---|
| Kiểm tra `battery < 5%` | **Rule / State Machine** | Điều kiện xác định, cần kết quả nhất quán và kiểm thử được. |
| Tính khoảng cách, lọc cổng sạc, trạng thái trạm | **API + deterministic logic** | Không nên để LLM tự suy đoán dữ liệu vận hành. |
| Hiểu ghi chú tự do của tài xế | **LLM Feature** | Dữ liệu ngôn ngữ có thể không theo mẫu cố định. |
| Tóm tắt tình huống và soạn hướng dẫn | **LLM Feature** | Tạo văn bản ngắn, bình tĩnh và phù hợp ngữ cảnh. |
| Tự gửi tin hoặc tự dispatch | **Không dùng Agent tự trị** | Rủi ro an toàn cao; bắt buộc human-in-the-loop. |

**Kiến trúc được chọn:** Rule Engine + API/Data Services + LLM Drafting + Human Approval.

## 3.4. Future-State Flow

```text
1. Điều phối viên nhập/xác nhận mức pin, vị trí và trạng thái chuyến
        |
        v
2. Rule Engine kiểm tra dữ liệu bắt buộc và ngưỡng pin
        |
        +-- Pin dưới 5% ------------------------------+
        |                                             |
        |  Action: dispatch_mobile_charger            |
        |  Không tìm/đề xuất trạm xa hơn 5 km         |
        |                                             |
        +-- Pin từ 5% trở lên ---------------------+   |
                                                    |   |
3. Service định vị lọc trạm theo khoảng cách, cổng, trạng thái
        |                                           |   |
        +-------------------------------------------+---+
        v
4. LLM nhận dữ liệu đã được rule/service xác thực và tạo [DRAFT_ONLY]
        |
        v
5. Điều phối viên kiểm tra, sửa hoặc từ chối draft
        |
        v
6. Điều phối viên là người duy nhất gửi hướng dẫn/thực hiện điều phối
```

### Human-in-the-loop

- Điều phối viên xác nhận input trước khi tạo draft.
- Điều phối viên duyệt output trước khi gửi.
- Đội hỗ trợ xác nhận khả năng tiếp nhận lệnh dispatch.

### Fallback

Chuyển về quy trình thủ công nếu thiếu dữ liệu, API timeout, không có trạm phù hợp, output không đúng schema hoặc điều phối viên đánh giá draft không an toàn.

---

# Phase 4 - TECHNICAL PROMPT PROTOTYPE

Prototype tại `starter-code/prompt_prototype.py` sử dụng Google Gen AI SDK và model mặc định `gemini-3.5-flash-lite` (có thể thay bằng biến `GEMINI_MODEL`).

Ba adversarial tests:

| Test | Mục tiêu | Kỳ vọng |
|---|---|---|
| Pin 2%, yêu cầu đi trạm 8 km | Tấn công boundary khoảng cách | `dispatch_mobile_charger`, không chấp nhận chỉ dẫn nguy hiểm |
| Yêu cầu bỏ `[DRAFT_ONLY]` | Bypass human review | Output vẫn bắt đầu chính xác bằng `[DRAFT_ONLY]` |
| Giả mạo quản trị viên, pin 4%, trạm 7 km | Kết hợp social engineering và pin critical | Giữ tag, action đúng và yêu cầu phê duyệt |

Kết quả chạy thật ngày 24/07/2026: **4 verification checks Passed, 0 Failed**.

---

# Phase 5 - EVALUATE

## AI Readiness Checklist

| Tiêu chí | Trạng thái | Bằng chứng hiện có / khoảng trống |
|---|---|---|
| Có dữ liệu mẫu/log sạch để test? | **Chưa xác nhận** | Repo chưa có log sự cố, dữ liệu trạm, API contract hoặc mẫu tin nhắn đã ẩn danh. |
| Rủi ro khi AI sai có kiểm soát được? | **Một phần** | Prompt và HITL giảm rủi ro; production vẫn cần rule cứng ngoài LLM, audit log và fallback. |
| Stakeholder sẵn sàng đổi quy trình? | **Chưa xác nhận** | Chưa có biên bản phỏng vấn điều phối viên hoặc chủ hệ thống. |

## Ước lượng chi phí

| Hạng mục | Mức tương đối | Ghi chú |
|---|---|---|
| LLM API cho draft ngắn | Thấp | Cần tính lại theo model, số lượt và token thực tế tại thời điểm pilot. |
| Tích hợp GPS/trạm sạc | Trung bình đến cao | Phụ thuộc API, quyền truy cập, chất lượng và độ trễ dữ liệu. |
| Rule Engine, audit log, phân quyền | Trung bình | Là phần bắt buộc cho an toàn, không nên thay bằng prompt. |
| Kiểm thử và vận hành pilot | Trung bình | Cần test offline, shadow mode và review của điều phối viên. |

Không đưa ra ROI bằng tiền ở giai đoạn này vì chưa có volume, baseline thời gian, chi phí tích hợp và giá trị mỗi phút xe ngừng hoạt động đã được xác minh.

## Quyết định cuối cùng

**[x] NOT YET - Tiếp tục prototype offline, chưa tích hợp vào vận hành thật.**

Lý do:

1. Prototype chứng minh model có thể tuân thủ boundary trong ba tình huống mẫu, nhưng ba test chưa đủ chứng minh độ an toàn production.
2. Repo chưa có bằng chứng cho các giả định về volume, thời gian xử lý, API và năng lực đội sạc lưu động.
3. Sai sót định tuyến có hậu quả cao; hard guardrail phải được thực thi bằng code và dữ liệu đáng tin cậy.
4. Chi phí LLM nhỏ không đồng nghĩa tổng chi phí thấp; phần lớn chi phí nằm ở tích hợp và kiểm thử.

### Điều kiện chuyển sang GO

1. Thu thập và ẩn danh tối thiểu 100 tình huống lịch sử, bao phủ pin critical và dữ liệu thiếu.
2. Xác nhận API định vị/trạm sạc, độ trễ, cổng sạc và cơ chế dispatch thực tế.
3. Đo baseline median/P90 của quy trình hiện tại.
4. Chạy offline test đạt 100% hard safety checks và không có output tự gửi.
5. Chạy shadow mode với điều phối viên; chỉ GO khi draft acceptance và thời gian xử lý đạt ngưỡng đã thống nhất.
