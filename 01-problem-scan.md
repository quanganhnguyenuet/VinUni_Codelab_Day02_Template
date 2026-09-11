# Lab 02 — Problem Scan & Quick Problem Cards

## 1. Thông tin người thực hiện

| Trường | Nội dung |
|---|---|
| Họ và tên | Nguyễn Vũ Quang Anh |
| Mã học viên | 2A202602805 |
| Nhóm | Chưa cập nhật |

## 2. Bối cảnh và yêu cầu bài tập

**Công ty/mảng quan tâm:** Hệ sinh thái Vin Smart Future, gồm VinFast, Xanh SM, Vinhomes, Vinmec, Vinpearl và VinUni.

**Mục tiêu quét bài toán:** Tìm các quy trình vận hành đang lặp lại, tốn thời gian, dễ sai hoặc gây trải nghiệm kém; sau đó đánh giá nơi Rule, LLM hoặc Agent có thể hỗ trợ con người với ranh giới vận hành rõ ràng.

### Đề bài cần thực hiện qua 3 phase

1. **Phase 1 — SCAN:** Dùng 4 lenses (Repetitive, Time-consuming, AI-upgrade, Stakeholder Pain) để tìm ít nhất 5 bài toán thực tế trong các công ty thành viên Vingroup. Mỗi bài toán phải nêu được quy trình gây lãng phí, người bị ảnh hưởng và bằng chứng hoặc giả định ban đầu.
2. **Phase 2 — QUICK-ASSESS:** Chọn 3 bài toán đáng quan tâm nhất. Với mỗi bài toán, mô tả workflow hiện tại từ 3–5 bước, bottleneck, thời gian xử lý, vị trí AI tham gia, metric có số, kiến trúc phù hợp và rủi ro cần kiểm chứng.
3. **Phase 3 — DEEP-DIVE:** Chọn 1 trong 3 bài toán để nhóm phân tích sâu: vẽ current-state workflow, viết problem statement 6 trường, so sánh Rule/LLM/Agent, thiết kế future-state có Human-in-the-loop và fallback, rồi đưa ra quyết định GO/NOT YET/NO-GO.

> **Lưu ý về số liệu:** Các số liệu ghi là “giả định” dưới đây chỉ là baseline để thiết kế thử nghiệm. Nhóm cần xác minh bằng log vận hành, phỏng vấn stakeholder hoặc pilot trước khi dùng làm business case chính thức.

## 3. Phase 1 — SCAN: Danh sách cơ hội

| # | Công ty thành viên | Lens | Đề bài/vấn đề đáng quan tâm | Ai bị ảnh hưởng? | Bằng chứng hoặc ước tính ban đầu | Giải pháp đề xuất |
|---:|---|---|---|---|---|---|
| 1 | **Xanh SM** | Time-consuming | Khi xe điện báo pin yếu giữa đường, điều phối viên phải tra vị trí xe, trạm sạc, loại cổng và soạn hướng dẫn thủ công. | Tài xế, điều phối viên, khách đang chờ xe | Worked example của lab dùng baseline 15 phút/lượt và xác định bước tra trạm, soạn tin là bottleneck. | Dùng rule kiểm tra ngưỡng pin và khả năng tương thích; API lấy GPS/trạm trống; LLM chỉ soạn tin nháp. Pin dưới 5% phải đề xuất xe sạc di động; điều phối viên duyệt trước khi gửi. |
| 2 | **VinFast** | AI-upgrade | Cố vấn dịch vụ phải chuyển mô tả lỗi xe bằng tiếng Việt tự nhiên của khách thành nhóm lỗi và hạng mục kiểm tra ban đầu. | Khách hàng, cố vấn dịch vụ, kỹ thuật viên xưởng | Giả định cần xác minh: 8–12 phút/phiếu để hỏi lại và mã hóa triệu chứng; mô tả mơ hồ dễ chuyển sai tổ kỹ thuật. | LLM trích xuất triệu chứng, bộ phận xe, điều kiện phát sinh và tạo danh sách mã lỗi ứng viên; rule kiểm tra schema. Kỹ thuật viên chẩn đoán và phê duyệt, AI không kết luận an toàn xe. |
| 3 | **Vinhomes** | Repetitive | Phản ánh cư dân về điện, nước, thang máy, an ninh và tiếng ồn phải được nhân viên đọc rồi chuyển đến đúng tòa nhà/bộ phận. | Cư dân, CSKH, ban quản lý và đội kỹ thuật | Giả định cần xác minh: 3–5 phút/ticket; ticket chuyển sai làm tăng một vòng handoff và kéo dài SLA. | Bộ phân loại kết hợp rule theo tòa/căn hộ với LLM nhận diện chủ đề, mức khẩn cấp và tóm tắt; tự route ticket có độ tin cậy cao, còn ticket nhạy cảm hoặc mơ hồ chuyển người duyệt. |
| 4 | **Vinmec** | Time-consuming | Bác sĩ phải tổng hợp bệnh án, xét nghiệm, thuốc và dặn dò để viết tóm tắt xuất viện dễ hiểu cho bệnh nhân. | Bác sĩ, điều dưỡng, bệnh nhân | Inspiration kit nêu đây là tác vụ tốn thời gian; giả định cần xác minh: 20–30 phút/hồ sơ và có nguy cơ bỏ sót khi dữ liệu nằm ở nhiều màn hình. | LLM tạo bản nháp từ các trường dữ liệu đã chọn; rule đối chiếu thuốc, dị ứng và lịch tái khám. Bác sĩ bắt buộc duyệt/ký; AI không thêm chẩn đoán hoặc chỉ định không có trong hồ sơ. |
| 5 | **Vinpearl** | Stakeholder Pain | Nhân viên đặt phòng phải đọc email đoàn có nhiều loại phòng, ngày ở, suất ăn và yêu cầu đặc biệt rồi nhập lại vào hệ thống. | Nhân viên reservation, đối tác lữ hành, khách đoàn | Giả định cần xác minh: 15–25 phút/yêu cầu; thay đổi qua nhiều email dễ gây nhập thiếu hoặc nhầm phiên bản. | LLM trích xuất yêu cầu thành JSON; rule kiểm tra ngày, số khách, sức chứa và tồn phòng; hệ thống tạo draft báo giá/booking để nhân viên xác nhận trước khi giữ phòng. |
| 6 | **VinUni** | Repetitive | Giảng viên/trợ giảng phải đọc log autograder và viết phản hồi riêng cho nhiều bài lab có lỗi cú pháp hoặc logic tương tự. | Sinh viên, giảng viên, trợ giảng | Giả định cần xác minh: 5–10 phút/bài sau khi autograder chạy; phản hồi thủ công dễ không đồng đều giữa các lớp. | Giữ autograder deterministic để chấm test; LLM giải thích lỗi và gợi ý bước sửa dựa trên rubric. Giảng viên duyệt phản hồi; AI không tự thay đổi điểm hoặc cung cấp lời giải hoàn chỉnh. |

### Vì sao 6 vấn đề này đáng quan tâm

- **Xanh SM:** tác động trực tiếp đến an toàn vận hành và thời gian xe ngừng phục vụ; dữ liệu đầu vào có cấu trúc và prototype hiện tại đã bám đúng use case.
- **VinFast:** giảm thời gian tiếp nhận tại xưởng và cải thiện handoff từ ngôn ngữ khách hàng sang quy trình kỹ thuật.
- **Vinhomes:** khối lượng ticket lặp lại phù hợp để tự động phân loại, đồng thời metric SLA và tỷ lệ route đúng khá dễ đo.
- **Vinmec:** có tiềm năng giải phóng thời gian bác sĩ nhưng rủi ro lâm sàng cao, nên chỉ phù hợp dưới dạng draft có bác sĩ duyệt.
- **Vinpearl:** email đoàn là dữ liệu bán cấu trúc mà LLM xử lý tốt; có thể đo ngay thời gian nhập liệu và tỷ lệ trường bị sai.
- **VinUni:** kết hợp tốt giữa chấm code bằng rule/test và phản hồi ngôn ngữ bằng LLM, với ranh giới rõ là không tự quyết định điểm.

## 4. Phase 2 — QUICK-ASSESS

Ba bài toán được chọn để đánh giá nhanh là **#1 Xanh SM**, **#3 Vinhomes** và **#5 Vinpearl**. Đây là ba use case có tác động vận hành rõ, đầu vào tương đối sẵn có và kết quả có thể đo bằng thời gian, độ chính xác hoặc SLA.

### Quick Problem Card #1

**Tên bài toán:** Hỗ trợ điều phối sự cố pin yếu cho Xanh SM

**Bài toán trong một câu:** Điều phối viên Xanh SM mất nhiều thời gian tra cứu và hướng dẫn tài xế khi xe pin yếu giữa đường, trong khi một đề xuất sai có thể khiến xe cạn pin trước khi đến trạm.

**Công ty thành viên:** Xanh SM (GSM)

**Lens:** Time-consuming

**Ai đang đau (Actor/Stakeholder):** Điều phối viên, tài xế và khách hàng đang chờ chuyến.

**Workflow thủ công hiện tại:**

1. Tài xế gọi hoặc nhắn cho trung tâm điều vận, cung cấp biển số và tình trạng pin.
2. Điều phối viên tra vị trí GPS của xe trên hệ thống nội bộ.
3. Điều phối viên mở dashboard để tìm trạm đang hoạt động, có trụ trống và tương thích với xe.
4. Điều phối viên đánh giá xe có thể đến trạm hay phải gọi hỗ trợ di động.
5. Điều phối viên soạn, kiểm tra và gửi hướng dẫn cho tài xế.

**Bước tốn thời gian hoặc dễ lỗi nhất:** Bước 3–5 — baseline của worked example khoảng 10–12 phút/lượt.

**Giải pháp và vị trí AI hỗ trợ:** Rule/state machine kiểm tra mức pin, khoảng cách và loại cổng; API tự lấy dữ liệu GPS/trạm; LLM soạn bản nháp hướng dẫn ngắn gọn. Điều phối viên duyệt rồi mới gửi.

**Success metric có số:** Giảm tổng thời gian xử lý từ 15 phút xuống dưới 3 phút; ít nhất 98% đề xuất đúng trạm và đúng cổng; 100% phản hồi có nhãn `[DRAFT_ONLY]`; 100% trường hợp pin dưới 5% không đề xuất trạm xa trên 5 km.

**Quick Architecture:** [ ] No AI &nbsp;&nbsp; [x] Rule &nbsp;&nbsp; [x] LLM Feature &nbsp;&nbsp; [ ] Agent

**Rủi ro hoặc điều cần kiểm chứng:** Độ mới của dữ liệu trụ trống; sai số GPS; mapping loại xe–cổng sạc; ngưỡng pin và khoảng cách cần được đội an toàn vận hành phê duyệt. Nếu API lỗi hoặc model không chắc chắn, chuyển về quy trình thủ công.

### Quick Problem Card #2

**Tên bài toán:** Phân loại và điều hướng phản ánh cư dân Vinhomes

**Bài toán trong một câu:** Nhân viên CSKH phải đọc và chuyển thủ công phản ánh tự do của cư dân đến đúng bộ phận, khiến ticket dễ chậm hoặc đi sai nơi.

**Công ty thành viên:** Vinhomes

**Lens:** Repetitive

**Ai đang đau (Actor/Stakeholder):** Cư dân, nhân viên CSKH, ban quản lý tòa nhà và đội kỹ thuật.

**Workflow thủ công hiện tại:**

1. Cư dân gửi phản ánh qua ứng dụng, có thể kèm ảnh và nội dung tự do.
2. CSKH đọc nội dung, tra tòa/căn hộ và xác định nhóm vấn đề.
3. Nhân viên đánh giá mức độ khẩn cấp và chọn bộ phận tiếp nhận.
4. Ticket được chuyển cho ban quản lý hoặc kỹ thuật; nơi nhận có thể trả lại nếu route sai.
5. CSKH theo dõi SLA và cập nhật trạng thái cho cư dân.

**Bước tốn thời gian hoặc dễ lỗi nhất:** Bước 2–4 — giả định 3–5 phút/ticket, chưa gồm thời gian phát sinh nếu chuyển sai.

**Giải pháp và vị trí AI hỗ trợ:** Rule dùng metadata tòa/căn hộ và từ khóa khẩn cấp; LLM phân loại chủ đề, tóm tắt và đề xuất bộ phận xử lý. Chỉ tự route khi confidence vượt ngưỡng; khiếu nại pháp lý, an ninh hoặc mức độ khẩn cấp phải có người duyệt.

**Success metric có số:** Ít nhất 90% ticket được phân loại trong 10 giây; tỷ lệ route đúng ngay lần đầu đạt ít nhất 95%; giảm thời gian xử lý phân loại trung vị ít nhất 70%; recall cho nhóm khẩn cấp đạt ít nhất 99% trên bộ test đã gán nhãn.

**Quick Architecture:** [ ] No AI &nbsp;&nbsp; [x] Rule &nbsp;&nbsp; [x] LLM Feature &nbsp;&nbsp; [ ] Agent

**Rủi ro hoặc điều cần kiểm chứng:** Cần taxonomy thống nhất giữa các khu đô thị; dữ liệu chứa thông tin cá nhân; ngôn ngữ mỉa mai hoặc phản ánh nhiều vấn đề có thể làm phân loại sai. Fallback là hàng chờ CSKH hiện tại.

### Quick Problem Card #3

**Tên bài toán:** Trích xuất yêu cầu đặt phòng đoàn Vinpearl

**Bài toán trong một câu:** Nhân viên reservation phải đọc chuỗi email phức tạp và nhập lại yêu cầu đoàn vào hệ thống, gây chậm báo giá và có nguy cơ sai ngày, số khách hoặc loại phòng.

**Công ty thành viên:** Vinpearl

**Lens:** Stakeholder Pain

**Ai đang đau (Actor/Stakeholder):** Nhân viên reservation, sales, đối tác lữ hành và khách đoàn.

**Workflow thủ công hiện tại:**

1. Đối tác gửi email nêu ngày ở, số khách, loại phòng, suất ăn và yêu cầu đặc biệt.
2. Nhân viên đọc email và các file đính kèm, đối chiếu phiên bản mới nhất.
3. Nhân viên nhập từng trường vào PMS/booking system và kiểm tra tồn phòng.
4. Nhân viên tính giá theo hợp đồng, chính sách đoàn và các dịch vụ kèm theo.
5. Nhân viên soạn báo giá hoặc booking draft để gửi đối tác xác nhận.

**Bước tốn thời gian hoặc dễ lỗi nhất:** Bước 2–4 — giả định 15–25 phút/yêu cầu.

**Giải pháp và vị trí AI hỗ trợ:** LLM trích xuất email và bảng đính kèm thành JSON có dẫn nguồn; rule kiểm tra ngày, tổng khách, sức chứa, giá hợp đồng và tồn phòng; hệ thống tạo draft để nhân viên xác nhận.

**Success metric có số:** Giảm thời gian tạo booking draft xuống dưới 5 phút; độ chính xác theo từng trường đạt ít nhất 98% với các trường bắt buộc; 100% booking phải được nhân viên duyệt trước khi giữ phòng hoặc gửi báo giá.

**Quick Architecture:** [ ] No AI &nbsp;&nbsp; [x] Rule &nbsp;&nbsp; [x] LLM Feature &nbsp;&nbsp; [ ] Agent

**Rủi ro hoặc điều cần kiểm chứng:** Email có nhiều phiên bản, bảng ảnh scan, chính sách giá phức tạp hoặc yêu cầu mâu thuẫn. Nếu thiếu trường bắt buộc hoặc confidence thấp, hệ thống phải đánh dấu để nhân viên hỏi lại đối tác, không tự đặt phòng.

## 5. Lựa chọn bài toán cho Deep Dive

**Bài toán nhóm chọn:** Card #1 — Hỗ trợ điều phối sự cố pin yếu cho Xanh SM.

**Lý do lựa chọn:**

- Quy trình hiện tại đã được worked example mô tả rõ gồm nhận sự cố, tra GPS, tra trạm sạc, chọn phương án và soạn hướng dẫn.
- Bottleneck đủ lớn và đo được: baseline của lab là 15 phút/lượt, trong đó tra cứu và soạn tin chiếm phần lớn thời gian.
- Có thể tạo dữ liệu test giả lập gồm mức pin, vị trí, khoảng cách, trạng thái trụ và loại cổng; nhóm cũng đã có `prompt_prototype.py` để stress-test ranh giới.
- Rule xử lý quyết định an toàn; LLM chỉ xử lý ngôn ngữ và tạo draft. Cách kết hợp này tận dụng đúng điểm mạnh của AI mà vẫn giữ Human-in-the-loop.
- Metric rõ ràng: thời gian dưới 3 phút, độ chính xác đề xuất 98%, tuân thủ nhãn draft và quy tắc pin tới hạn 100% trên test set.

**Lý do không chọn Card #2:** Bài toán Vinhomes có tiềm năng lớn nhưng cần taxonomy ticket thống nhất, dữ liệu lịch sử đã gán nhãn và đánh giá riêng các nhóm pháp lý/an ninh trước khi tự route.

**Lý do không chọn Card #3:** Bài toán Vinpearl cần tích hợp email, file đính kèm, PMS, tồn phòng và bảng giá hợp đồng; phạm vi tích hợp rộng hơn thời lượng prototype của lab.

**Câu hỏi cần xác minh trong Deep Dive:**

1. API GPS và API trạm sạc cập nhật với độ trễ bao nhiêu, và có trả về đầy đủ loại cổng, trạng thái hoạt động, số trụ trống không?
2. Baseline thực tế về số sự cố/ngày, thời gian xử lý trung vị và chi phí mỗi phút xe ngừng phục vụ là bao nhiêu?
3. Đội an toàn phê duyệt ngưỡng pin/khoảng cách nào; trường hợp nào bắt buộc điều xe sạc di động hoặc chuyển điều phối viên xử lý?
4. Ai là người duyệt draft, thời gian duyệt mục tiêu bao lâu và fallback nào được dùng khi API hoặc Gemini không phản hồi?
5. Bộ test adversarial cần bao phủ những trường hợp nào: pin thiếu/mâu thuẫn, GPS sai, trạm hết chỗ, sai cổng sạc và yêu cầu bỏ qua `[DRAFT_ONLY]`?

## 6. Checklist hoàn thành

- [x] Có ít nhất 5 bài toán trong bảng SCAN.
- [x] Các bài toán sử dụng nhiều lens khác nhau.
- [x] Đã hoàn thiện 3 Quick Problem Cards.
- [x] Mỗi card có workflow từ 3–5 bước.
- [x] Mỗi card có bottleneck và thời gian ước tính.
- [x] Mỗi card có metric đo được bằng số.
- [x] Đã chọn một bài toán cho `02-deep-dive-report.md`.
