# 03 - Nhật ký chiêm nghiệm AI

**Lab:** AI Product Scoping - Vin Smart Future

**Người thực hiện:** Nguyễn Chí Hiếu

**MSSV:** 2A202601931

---

## AI đã giúp gì?

Tôi sử dụng AI như một thought-partner trong bốn phần chính:

1. **Quét và phản biện bài toán:** AI hỗ trợ tạo danh sách pain point từ nhiều công ty thành viên, sau đó so sánh các problem scan trong những nhánh của nhóm. Việc so sánh giúp tôi phân biệt đề tài có giá trị sản phẩm với đề tài phù hợp để prototype trong thời lượng lab.

2. **Chuẩn hóa deliverable:** AI đối chiếu `01-problem-scan.md` với worksheet và phát hiện phần “đề xuất bài toán deep-dive” không thuộc bài cá nhân. Tôi đã bỏ phần đó và đưa ba Quick Problem Cards về đúng khung mẫu.

3. **Viết và stress-test system prompt:** AI hỗ trợ làm rõ ba ranh giới: mọi output phải bắt đầu bằng `[DRAFT_ONLY]`; pin dưới 5% không được đề xuất trạm xa hơn 5 km; AI chỉ tạo draft và không được tự dispatch.

4. **Debug prototype:** Khi chạy thật, Gemini API trả `404` vì model trong slide không còn cấp cho API key mới. Tôi dùng danh sách model từ chính API để chuyển sang model khả dụng. Sau đó autograder trên Windows gặp lỗi encoding `cp1252`; tôi cấu hình output UTF-8 và chạy lại thành công.

## AI đã sai hoặc thiếu gì?

Sai sót đáng chú ý nhất là AI ban đầu viết thêm phần đề xuất chọn một bài toán cho nhóm ngay trong file cá nhân. Phần này nghe hợp lý nhưng không có trong mẫu Phase 1-2 và có thể tạo cảm giác một cá nhân quyết định thay cả nhóm.

AI cũng dễ biến số liệu brainstorm thành phát biểu chắc chắn. Các con số như số sự cố mỗi ngày, số giờ lãng phí, API “đã có sẵn” hay ROI không thể coi là dữ liệu thật nếu repo không chứa log, nguồn hoặc xác nhận stakeholder. Một bản báo cáo trông thuyết phục vẫn có thể sai về mặt bằng chứng.

Ngoài ra, chỉ dùng system prompt để bảo vệ ngưỡng pin là chưa đủ cho production. LLM có tính xác suất; điều kiện `battery < 5%`, khoảng cách và tương thích cổng sạc phải được kiểm tra lại bằng code deterministic.

## Tôi đã sửa đổi ra sao?

1. Xóa nội dung vượt phạm vi khỏi problem scan cá nhân và trình bày lại đúng mẫu.
2. Gắn nhãn các metric là giả định scoping, cần kiểm chứng bằng log/phỏng vấn.
3. Tách kiến trúc thành Rule Engine + API/Data Services + LLM Drafting + Human Approval.
4. Viết ba adversarial tests, trong đó có một test kết hợp giả mạo quyền hạn và pin critical.
5. Chạy model thật thay vì chỉ đọc code; sửa model endpoint và lỗi UTF-8 dựa trên lỗi thực tế.
6. Chuyển quyết định readiness từ GO quá sớm sang NOT YET có điều kiện rõ ràng để tiến tới pilot.

## Bài học rút ra

AI hữu ích nhất khi giúp tôi tạo phương án, chỉ ra lỗ hổng và thực hiện kiểm thử lặp lại. Tuy nhiên, quyết định sản phẩm vẫn phải dựa trên bằng chứng. Tôi cần kiểm tra cả ba lớp: nội dung có đúng mẫu không, kiến trúc có giao đúng việc cho AI không, và các tuyên bố về dữ liệu/ROI có nguồn hay chỉ là giả định.
