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

## Quick Problem Card #1 - Xanh SM xử lý sự cố pin thấp

**Bài toán:** Tài xế Xanh SM báo pin rất thấp khi đang vận hành, cần nhận phương án an toàn: trạm sạc gần nhất hoặc xe sạc lưu động.

**Công ty thành viên:** Xanh SM

**Ai đang đau (Actor)?**  
Tài xế Xanh SM, điều phối viên tổng đài và khách hàng đang chờ chuyến.

**Workflow thủ công hiện tại:**

1. Tài xế gọi/tin nhắn về tổng đài báo mức pin, vị trí hiện tại và tình trạng khách.
2. Điều phối viên mở bản đồ nội bộ để xem tọa độ xe.
3. Điều phối viên tra cứu thủ công các trạm sạc gần đó và tình trạng trụ trống.
4. Điều phối viên ước lượng khoảng cách, mức pin còn lại và chọn phương án.
5. Điều phối viên soạn tin chỉ dẫn cho tài xế hoặc liên hệ đội xe sạc/cứu hộ.

**Bước tốn thời gian/lỗi nhất:**  
Bước 3-4, khoảng 10-15 phút/lượt, dễ sai khi giờ cao điểm hoặc dữ liệu trạm sạc thay đổi nhanh.

**AI có thể hỗ trợ ở bước nào?**  
AI hỗ trợ bước 3-5: đọc thông tin tài xế, tóm tắt tình huống, đề xuất phương án an toàn, tạo draft tin nhắn cho điều phối viên kiểm tra.

**Metric thành công:**  
Giảm thời gian xử lý sự cố pin thấp từ 15 phút xuống dưới 3 phút/lượt; 95% trường hợp pin dưới 5% không bị gợi ý trạm sạc xa hơn 5 km.

**Quick Architecture:** LLM Feature + Rule Guardrail

**Operational Boundary sơ bộ:**

- Nếu pin dưới 5%, AI không được gợi ý trạm sạc xa hơn 5 km.
- AI chỉ được tạo draft, không tự động gửi lệnh điều phối.
- Điều phối viên phải phê duyệt trước khi gửi hướng dẫn cho tài xế.

---

## Quick Problem Card #2 - Vinhomes phân loại phản ánh cư dân

**Bài toán:** Phản ánh của cư dân trên app Vinhomes Resident bị route chậm hoặc sai bộ phận, làm tăng SLA xử lý khiếu nại.

**Công ty thành viên:** Vinhomes

**Ai đang đau (Actor)?**  
Cư dân, nhân viên ban quản lý tòa nhà và các đội vận hành như kỹ thuật, vệ sinh, an ninh.

**Workflow thủ công hiện tại:**

1. Cư dân gửi phản ánh trên app kèm mô tả và hình ảnh.
2. Nhân viên CSKH đọc nội dung và xác định loại sự cố.
3. Nhân viên gán ticket cho bộ phận phụ trách.
4. Bộ phận nhận ticket hỏi lại nếu thiếu thông tin.
5. CSKH cập nhật trạng thái và phản hồi cho cư dân.

**Bước tốn thời gian/lỗi nhất:**  
Bước 2-3, khoảng 6-10 phút/ticket; lỗi phân loại sai làm ticket bị chuyển qua lại giữa các bộ phận.

**AI có thể hỗ trợ ở bước nào?**  
AI hỗ trợ bước 2-4: phân loại ticket, trích xuất tòa nhà/tầng/căn hộ, mức độ ưu tiên, và draft câu hỏi bổ sung nếu thiếu thông tin.

**Metric thành công:**  
85% ticket được phân loại đúng trong dưới 10 giây; giảm tỷ lệ ticket bị route sai từ 18% xuống dưới 5%.

**Quick Architecture:** Rule + LLM Feature + Human Review

LLM dùng để hiểu mô tả tự do và trích xuất thông tin; rule dùng để ánh xạ loại sự cố, mức ưu tiên và bộ phận phụ trách theo danh mục nghiệp vụ đã được Vinhomes phê duyệt.

**Operational Boundary sơ bộ:**

- AI không được hứa bồi thường, miễn phí dịch vụ hay đưa kết luận pháp lý.
- Ticket liên quan an toàn, cháy nổ, thang máy, bảo vệ trẻ em phải route khẩn cấp cho người trực.
- Nhân viên CSKH duyệt nội dung phản hồi trước khi gửi cho cư dân.

---

## Quick Problem Card #3 - VinFast phân loại lỗi xe từ mô tả khách hàng

**Bài toán:** Khách hàng VinFast mô tả triệu chứng lỗi xe bằng tiếng Việt đời thường, khiến CSKH mất thời gian hỏi lại và phân loại lỗi kỹ thuật ban đầu.

**Công ty thành viên:** VinFast

**Ai đang đau (Actor)?**  
Khách hàng, nhân viên CSKH, cố vấn dịch vụ và kỹ thuật viên tại xưởng.

**Workflow thủ công hiện tại:**

1. Khách hàng gọi/tin nhắn mô tả triệu chứng xe.
2. CSKH hỏi lại thông tin xe, dòng xe, thời điểm xảy ra lỗi và mức độ nguy hiểm.
3. CSKH tự gán nhóm lỗi ban đầu theo kinh nghiệm.
4. Ticket được chuyển sang cố vấn dịch vụ/kỹ thuật viên.
5. Cố vấn dịch vụ liên hệ lại nếu ticket thiếu thông tin.

**Bước tốn thời gian/lỗi nhất:**  
Bước 2-3, khoảng 8-12 phút/ticket; lỗi dễ xảy ra khi mô tả của khách hàng không dùng thuật ngữ kỹ thuật.

**AI có thể hỗ trợ ở bước nào?**  
AI hỗ trợ bước 2-4: trích xuất triệu chứng, gợi ý nhóm lỗi ban đầu, mức độ ưu tiên, và tạo danh sách câu hỏi cần bổ sung.

**Metric thành công:**  
Giảm thời gian tạo ticket từ 12 phút xuống dưới 4 phút; 80% ticket có đủ thông tin tối thiểu trước khi chuyển sang cố vấn dịch vụ.

**Quick Architecture:** Rule + LLM Feature + Human Review

LLM dùng để chuẩn hóa mô tả đời thường và gợi ý câu hỏi bổ sung; rule dùng để nhận diện từ khóa an toàn nghiêm trọng và bắt buộc chuyển người xử lý.

**Operational Boundary sơ bộ:**

- AI không được kết luận chẩn đoán kỹ thuật cuối cùng.
- Lỗi liên quan phanh, pin, túi khí, mất lái hoặc nguy cơ cháy nổ phải gán mức ưu tiên cao và yêu cầu nhân viên xử lý ngay.
- Tất cả khuyến nghị liên quan an toàn vận hành phải do nhân viên VinFast xác nhận.

---

## Đề xuất bài toán nên chọn cho deep-dive nhóm

**Đề xuất chọn:** Quick Problem Card #1 - Xanh SM xử lý sự cố pin thấp.

**Lý do:**

- Quy trình có actor rõ: tài xế, điều phối viên, đội xe sạc/cứu hộ.
- Bottleneck có thể đo bằng thời gian xử lý và tỷ lệ gợi ý sai.
- AI phù hợp ở mức LLM Feature, không cần Agent quá phức tạp.
- Có operational boundary rõ ràng: pin dưới 5% thì ưu tiên xe sạc lưu động, không gợi ý trạm quá xa.
- Starter code của repo đã có sẵn hướng prompt prototype cho case này, nên dễ tiếp tục làm task code.
