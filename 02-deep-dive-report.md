# 02 — Deep-Dive Report: AI Vehicle Health & Predictive Maintenance

## Executive summary

Đề xuất phát hiện sớm rủi ro sức khỏe xe VinFast từ telemetry, DTC, lịch sử dịch vụ và outcome sửa chữa đã gán nhãn. Hệ thống tạo **risk score + evidence + bản nháp khuyến nghị**, không tự chẩn đoán an toàn, sửa/điều khiển xe hay tự đặt lịch. Quyết định: **NOT YET production; GO offline pilot có kiểm soát**.

## 1. Current-state workflow

| Bước | Người/hệ thống | Hoạt động | Thời gian cần đo | Rủi ro/bottleneck |
|---:|---|---|---:|---|
| 1 | Xe/backend | Ghi telemetry, DTC và cảnh báo. | near-real-time đến theo lô | Mất kết nối, thiếu dữ liệu, schema/version không đồng nhất. |
| 2 | Chủ xe/CSKH | Chủ xe thấy cảnh báo hoặc gọi hỗ trợ; CSKH tạo ticket. | 2–5 phút | 🔄 Handoff; mô tả thiếu bối cảnh. |
| 3 | Kỹ thuật viên triage | Đọc DTC, lịch sử và xu hướng thông số để ưu tiên. | 10–20 phút (giả định) | 🔴 Dữ liệu nhiều chiều, khó thấy trend tiến triển. |
| 4 | Cố vấn dịch vụ | Liên hệ chủ xe, đề xuất kiểm tra/đặt lịch theo SOP. | 5–10 phút | 🔄 Không cam kết chẩn đoán/ETA khi chưa xác minh. |
| 5 | Xưởng | Kiểm tra thực tế, sửa chữa, ghi root cause/outcome. | tùy lỗi | Nhãn feedback chưa chuẩn khiến ML học sai. |

VinFast công bố chẩn đoán lỗi từ xa và thông báo tới ứng dụng/trung tâm dịch vụ; điều này là bối cảnh công khai, không chứng minh sẵn dataset predictive-maintenance hoàn chỉnh. [Nguồn](https://vinfastauto.com/vn_vi/tinh-nang-moi-cham-soc-khach-hang-tu-dong-vf-e34)

## 2. Problem statement — 6 fields

| Field | Nội dung |
|---|---|
| Actor / Operator | Kỹ thuật viên triage, cố vấn dịch vụ, reliability engineer; chủ xe chỉ nhận thông báo sau duyệt. |
| Current Workflow | Con người đọc DTC/cảnh báo cùng lịch sử xe, sau đó theo dõi, hướng dẫn, đặt lịch hoặc escalate theo SOP. |
| Bottleneck | Telemetry có thể báo trend bất thường trước một rule ngưỡng; không thể đọc sâu mọi xe mỗi ngày. |
| Business Impact | Có thể giảm sự cố bất ngờ, xe nằm xưởng và thời gian triage; chưa gán giá trị tiền tệ khi chưa có dữ liệu nội bộ. |
| Success Metric | Recall ≥90% cho “inspect within 24h”; false-positive ≤15%; lead time trung vị ≥24h trước event xác nhận; 100% alert có evidence/audit ID. |
| Operational Boundary | Chỉ risk triage/draft. Không ra lệnh dừng xe, xóa DTC, cập nhật firmware, điều khiển từ xa, tự đặt lịch hay kết luận “xe an toàn”. Policy DTC mức cao luôn override model và escalate theo SOP. |

## 3. Future-state flow & AI fit

1. **Data-quality gate (Rule):** kiểm tra consent, schema, freshness, firmware version và missingness. Không đạt → `needs_human_review`, không scoring.
2. **Safety policy (Rule):** DTC/điều kiện có severity cao đã được kỹ sư duyệt → escalation theo SOP, không chờ ML.
3. **Risk model (time-series ML):** dữ liệu hợp lệ tạo anomaly score và risk trong horizon xác định, với feature/model version đã phê duyệt.
4. **Evidence builder:** nêu feature thay đổi, độ mới dữ liệu, DTC liên quan và case lịch sử đã ẩn danh; không nói là root cause.
5. **LLM explanation:** chuyển evidence cấu trúc thành JSON/draft; không suy diễn raw telemetry hoặc khuyến nghị ngoài policy.
6. **🟢 Human-in-the-loop:** kỹ thuật viên chấp nhận/sửa/bác bỏ alert; cố vấn dịch vụ mới gửi thông báo/đề xuất lịch.
7. **Feedback loop:** outcome kiểm tra/sửa chữa được chuẩn hóa thành nhãn; review bias/drift trước retrain.
8. **↩️ Fallback:** model timeout, data stale, out-of-distribution hay conflict → chỉ hiện dữ liệu gốc/checklist và route SOP thủ công.

| Thành phần | Loại | Lý do |
|---|---|---|
| Consent, schema, DTC severity, quyền hành động | Rule/state machine | Policy rõ, cần audit và độ tin cậy cao. |
| Anomaly/risk prediction | Time-series ML | Pattern đa biến theo thời gian, rule đơn giản khó bao phủ. |
| Tóm tắt cho người dùng/kỹ thuật viên | LLM feature | Hữu ích cho ngôn ngữ, bị giới hạn bởi evidence. |
| Đặt lịch, điều khiển xe, retrain | Không dùng agent | Quá rủi ro cho pilot. |

## 4. Data, model và evaluation plan

| Hạng mục | Thiết kế pilot |
|---|---|
| Dữ liệu | Telemetry pseudonymized: timestamp, model/version, DTC, battery/thermal/charging signals do kỹ sư duyệt, odometer, service outcome; tách PII/GPS chính xác. |
| Nhãn | Kỹ thuật viên xác nhận: no-issue, inspect-soon, inspect-24h, safety escalation; lưu lý do và SOP version. |
| Split | Time-based theo xe để tránh leakage; đánh giá riêng theo model/firmware/mùa. |
| Baseline | Rule theo DTC/threshold hiện hữu; chỉ dùng model nếu cải thiện recall/lead-time ở false-positive chấp nhận được. |
| Red-team | Telemetry thiếu/giả, model-version lạ, yêu cầu xóa lỗi/khẳng định xe an toàn, prompt injection trong ghi chú dịch vụ. |
| Monitoring | Drift, missingness, calibration, alert volume, acceptance rate, false negative hậu kiểm; kill switch về baseline rule. |

## 5. Readiness & decision

- [ ] Dataset có consent, data contract và nhãn outcome đáng tin: **chưa xác nhận**.
- [ ] Owner kỹ thuật phê duyệt feature/DTC, SOP escalation và quyền truy cập: **chưa xác nhận**.
- [x] Có thể giới hạn rủi ro bằng rule override, HITL, audit và kill switch.
- [ ] Baseline rule và tiêu chí rollback đã ký duyệt: **cần thiết lập**.

### Quyết định: NOT YET production; GO offline pilot

Chỉ pilot khi có data governance, nhãn chất lượng, baseline rõ và review kỹ thuật. Production cần KPI offline, shadow mode, safety review và bằng chứng không tăng false negative nghiêm trọng. [Ứng dụng VinFast đã công bố theo dõi/cảnh báo trạng thái xe](https://vinfastauto.com/vn_vi/quan-ly-xe-o-to-dien-qua-ung-dung-vinfast), nhưng đề xuất không giả định quyền truy cập hệ thống nội bộ.
