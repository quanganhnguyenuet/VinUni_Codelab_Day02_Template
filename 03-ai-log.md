# Lab 02 — AI Log & Reflection

**Họ và tên:** Nguyễn Vũ Quang Anh  
**Mã học viên:** 2A202602805  
**Bài toán:** Hỗ trợ điều phối sự cố pin yếu cho Xanh SM

## 1. Tôi đã dùng AI để làm gì?

- [x] Brainstorm vấn đề và chọn use case Xanh SM.
- [x] Soạn System Prompt với `[DRAFT_ONLY]` và quy tắc pin dưới 5%.
- [x] Viết/sửa `evaluate_prompt()` để gọi Gemini SDK.
- [x] Phản biện workflow, metric, rủi ro và Human-in-the-loop.

**Prompt hữu ích:** “Hãy xây dựng system prompt cho điều phối viên Xanh SM, luôn tạo bản nháp và phải điều xe sạc di động khi pin dưới 5%.”

**AI đã giúp:** AI giúp chuyển yêu cầu nghiệp vụ thành prompt, JSON output và các test adversarial nhanh hơn. Tôi vẫn tự kiểm tra ranh giới an toàn và không dùng AI để quyết định hành động thật.

## 2. Điều AI làm sai hoặc chưa đủ

| Phát hiện | Tôi đã sửa/kiểm tra |
|---|---|
| `gemini-2.5-flash` trả về lỗi 404 vì không còn cấp cho tài khoản mới. | Đổi model mặc định sang `gemini-3.6-flash` và cho phép đổi qua biến `GEMINI_MODEL`. |
| AI có thể coi kịch bản lab là chính sách vận hành thật. | Ghi rõ đây là prototype mô phỏng; dữ liệu GPS, trạm sạc và ngưỡng an toàn thực tế phải do đội vận hành xác minh. |

## 3. Kết quả chạy prototype

| Test | Kết quả thực tế | Trạng thái | Bước tiếp theo |
|---|---|---|---|
| Pin 2%, yêu cầu trạm 8 km | Lần chạy đầu bị lỗi 404 model, chưa đánh giá được output. | Chưa chạy lại | Chạy lại với `gemini-3.6-flash`; kiểm tra `dispatch_mobile_charger`. |
| Yêu cầu bỏ `[DRAFT_ONLY]` | Lần chạy đầu bị lỗi 404 model, chưa đánh giá được output. | Chưa chạy lại | Kiểm tra output bắt đầu bằng `[DRAFT_ONLY]`. |
| Test bổ sung: GPS thiếu/mâu thuẫn | Chưa chạy. | Chưa chạy | Kiểm tra AI yêu cầu thông tin thay vì bịa trạm sạc. |

## 4. Reflection ngắn

Tôi học được rằng Rule phù hợp cho quyết định an toàn có điều kiện rõ ràng, còn LLM phù hợp để hiểu ngôn ngữ và soạn bản nháp. Human-in-the-loop cần giữ ở bước duyệt trước khi gửi tin hoặc điều phối hỗ trợ.
