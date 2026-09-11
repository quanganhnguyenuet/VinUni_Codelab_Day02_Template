# 03 — AI Log & Reflection

## AI được dùng như thế nào

Tôi dùng AI để cấu trúc bài toán predictive maintenance: tách deterministic safety policy, mô hình chuỗi thời gian và lớp giải thích. AI cũng giúp tạo red-team cases về telemetry thiếu, dữ liệu ngoài phân phối và yêu cầu can thiệp xe trái quyền.

## AI hỗ trợ tốt

1. Biến ý tưởng “dự đoán lỗi” thành pipeline: data quality → rule override → risk model → evidence → human review → feedback.
2. Chỉ ra predictive maintenance không chỉ là LLM: tín hiệu chính là telemetry/DTC theo thời gian và nhãn outcome từ xưởng.
3. Làm rõ ranh giới: không tự xóa lỗi, điều khiển xe, khẳng định xe an toàn hoặc tự đặt lịch.

## Sai lệch/hallucination cần tránh

Nguồn công khai cho thấy VinFast có kết nối, theo dõi, cảnh báo và chẩn đoán từ xa; chúng không chứng minh dữ liệu training, schema, độ phủ xe, chất lượng nhãn hay KPI nội bộ. AI cũng dễ nhầm anomaly score là nguyên nhân gốc hoặc quyết định an toàn.

## Cách tôi điều chỉnh prompt và quy trình

- Mục tiêu metric là mục tiêu pilot, không phải fact.
- Model chỉ trả risk triage/draft dựa trên evidence đã cung cấp và luôn yêu cầu người duyệt.
- Policy an toàn/DTC cao override ML/LLM; data stale hoặc out-of-distribution chuyển `needs_human_review`.
- Tách train/test theo thời gian và xe, so với baseline rule, theo dõi drift trước retrain.

## Bài học

Ở vehicle health, dữ liệu, nhãn và rollback quan trọng hơn câu trả lời trôi chảy. LLM chỉ nên giải thích kết quả đã qua policy/model kiểm soát; kỹ thuật viên mới chẩn đoán và phê duyệt quyết định cuối.
