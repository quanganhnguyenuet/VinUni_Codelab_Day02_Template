# 01 — Problem Scan & Quick Cards

**Đề tài chọn:** AI Vehicle Health & Predictive Maintenance cho xe điện VinFast.

## Quét cơ hội

| # | Công ty | Lens | Pain point |
|---:|---|---|---|
| 1 | VinFast | Time-consuming + AI-upgrade | Kỹ thuật viên phải đọc mã lỗi, lịch sử cảnh báo và telemetry rời rạc để quyết định xe cần kiểm tra ngay hay theo dõi. |
| 2 | VinFast | Repetitive | Chủ xe nhận cảnh báo kỹ thuật nhưng khó hiểu mức khẩn và thời điểm đặt lịch. |
| 3 | Xanh SM | Stakeholder pain | Dấu hiệu suy giảm của xe đội có thể chỉ được phát hiện sau khi xe ngừng khai thác. |
| 4 | VinFast | Time-consuming | Xưởng nhận xe khi chưa có bản tóm tắt triệu chứng, DTC và lịch sử liên quan. |
| 5 | VinFast | Repetitive | Quality engineer phải rà nhiều log để thấy mẫu lỗi lặp theo cấu hình/phiên bản. |

## Cơ sở nghiên cứu

VinFast công bố chức năng chẩn đoán lỗi từ xa, gửi mã lỗi/cảnh báo tới ứng dụng và trung tâm dịch vụ, đồng thời mô tả khả năng kết nối xe với theo dõi hiệu năng và thông báo từ xa cho quyết định bảo dưỡng. Bài này **không** giả định quyền truy cập telemetry, log sửa chữa hay KPI nội bộ; mọi ngưỡng/mục tiêu là giả định pilot cần xác thực.

- [Chẩn đoán lỗi và chăm sóc từ xa](https://vinfastauto.com/vn_vi/tinh-nang-moi-cham-soc-khach-hang-tu-dong-vf-e34)
- [Kết nối xe và performance monitoring](https://vinfastauto.com/vn_en/vinfast-chooses-t-mobile-as-exclusive-global-connectivity-provider-for-electric-vehicles)
- [Cảnh báo lỗi/bảo dưỡng qua ứng dụng](https://vinfastauto.com/vn_vi/huong-dan-su-dung-ung-dung-vinfast)

## Quick Problem Cards

### Card #1 — Vehicle Health Risk Copilot (được chọn)

| Trường | Nội dung |
|---|---|
| Bài toán | Phát hiện, xếp hạng rủi ro bất thường từ telemetry/DTC để tạo bản nháp kiểm tra trước khi sự cố trở nên nghiêm trọng. |
| Actor | Kỹ thuật viên triage, quản lý fleet và cố vấn dịch vụ. |
| Workflow hiện tại | Xe/hệ thống báo lỗi → kỹ thuật viên đọc DTC/lịch sử → gọi hỏi triệu chứng → quyết định theo dõi/đặt lịch/escalate → ghi ticket. |
| Bottleneck | Dữ liệu đa biến theo chuỗi thời gian; mã lỗi đơn lẻ không đủ cho mức khẩn hay xu hướng tiến triển. |
| AI hỗ trợ | ML phát hiện anomaly/risk score từ dữ liệu đã gán nhãn; rule ưu tiên DTC an toàn; LLM chỉ giải thích evidence và soạn draft. |
| Metric | Recall ≥90% cho case “kiểm tra trong 24h” đã gán nhãn; false-positive ≤15%; giảm 30% thời gian triage; 0 hành động điều khiển xe tự động. |
| Architecture | **Rules + time-series ML + LLM explanation + human approval**. |

### Card #2 — Pre-visit service packet

Tạo tóm tắt trước khi xe vào xưởng gồm DTC liên quan, trend telemetry, lịch sử sửa chữa đã ẩn danh và checklist. Kỹ thuật viên xác nhận, không để AI kết luận nguyên nhân hay tự kê vật tư. Metric: ≥80% hồ sơ chuẩn bị trước lịch hẹn và giảm 20% thời gian đọc hồ sơ ban đầu.

### Card #3 — Quality signal clustering

Nhóm các case lỗi tương tự theo cấu hình, phiên bản phần mềm và dấu hiệu telemetry để quality engineer điều tra. Chỉ dashboard nêu bằng chứng; con người quyết định mở investigation. Metric pilot: ít nhất 3 cluster có thể hành động được do chuyên gia xác nhận trong 8 tuần.

## Lý do chọn Card #1

Đây không phải chatbot/RAG: giá trị cốt lõi là **dữ liệu chuỗi thời gian và phát hiện rủi ro**, LLM chỉ là lớp giải thích có kiểm soát. Nó khác case dispatcher có sẵn: đầu vào là vehicle-health telemetry/DTC, đầu ra là risk triage và đề nghị kiểm tra — không phải điều phối xe/trạm sạc.
