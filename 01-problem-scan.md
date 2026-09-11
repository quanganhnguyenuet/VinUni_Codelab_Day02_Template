# Lab 02 — Problem Scan & Quick Problem Cards

## 1. Thông tin người thực hiện

| Trường | Nội dung |
|---|---|
| Họ và tên | Nguyễn Vũ Quang Anh |
| Mã học viên | 2A202602805 |
| Nhóm | Chưa cung cấp |


## 2. Bối cảnh và hướng tìm kiếm

**Công ty/mảng quan tâm:** VinFast, Xanh SM, Vinhomes, Vinmec và Vinpearl — các mảng có quy trình dịch vụ khách hàng hoặc vận hành nhiều bước, phù hợp để thử nghiệm AI có kiểm soát.

**Mục tiêu quét bài toán:** Tìm các điểm chuyển giao thông tin thủ công giữa khách hàng, nhân viên vận hành và hệ thống; ưu tiên vấn đề có dữ liệu đầu vào rõ, kết quả đo được và có thể giữ người phê duyệt trong vòng lặp.

> **Nguyên tắc research:** Nguồn công khai chỉ chứng minh rằng quy trình/dịch vụ tồn tại. Các số về thời gian xử lý, khối lượng yêu cầu và độ chính xác trong file là giả thuyết để nhóm đo bằng log hoặc pilot, không phải số liệu do Vingroup công bố.

## 3. Phase 1 — SCAN: Danh sách cơ hội

> Liệt kê **ít nhất 5 bài toán/bottleneck thực tế**. Mỗi bài toán nên gắn với một lens cụ thể và mô tả được quy trình đang gây lãng phí.

| # | Công ty thành viên | Lens | Quy trình/bài toán | Ai bị ảnh hưởng? | Bằng chứng hoặc ước tính ban đầu | Giải pháp đề xuất và ranh giới |
|---:|---|---|---|---|---|---|
| 1 | VinFast | Time-consuming | Phân loại yêu cầu đặt lịch bảo dưỡng/sửa chữa từ mô tả tiếng Việt của khách hàng và chọn xưởng hoặc dịch vụ lưu động phù hợp. | Khách hàng, nhân viên CSKH, cố vấn dịch vụ | Ứng dụng VinFast yêu cầu chọn loại dịch vụ, mô tả vấn đề, địa điểm và thời gian; giả thuyết cần xác minh: nhân viên mất 10–20 phút/yêu cầu để kiểm tra và xác nhận thủ công. [Nguồn VinFast](https://vinfastauto.com/vn_vi/huong-dan-su-dung-tien-ich-dich-vu-tren-ung-dung-vinfast) | LLM trích xuất triệu chứng và đề xuất nhóm dịch vụ; rule kiểm tra địa điểm/thời gian khả dụng. Cố vấn dịch vụ quyết định loại sửa chữa và xác nhận lịch; AI không chẩn đoán lỗi an toàn. |
| 2 | VinFast | Stakeholder Pain | Tiếp nhận và điều phối cứu hộ khi xe hết pin hoặc gặp sự cố giữa đường: xác minh vị trí, loại xe, khả năng tiếp cận và điểm kéo xe phù hợp. | Chủ xe, tổng đài viên, đội cứu hộ | VinFast áp dụng cứu hộ bằng xe kéo RSA cho xe cá nhân hết pin từ 06/06/2025; chính sách này không áp dụng cho xe GSM và đối tác vận tải. Thời gian 10–30 phút/lượt là giả định cần xác minh. [Nguồn VinFast](https://vinfastauto.com/vn_vi/thong-bao-ve-viec-dieu-chinh-phuong-thuc-cuu-ho-cho-cac-truong-hop-xe-het-pin-tu-ngay-06062025) | Speech-to-text/LLM chuẩn hóa thông tin cuộc gọi; rule yêu cầu đủ vị trí, biển số và điều kiện tiếp cận. Tổng đài viên phê duyệt điều xe; AI không tự điều phối, không tự đưa chỉ dẫn an toàn. |
| 3 | Vinhomes | Repetitive | Phản ánh cư dân về điện, nước, thang máy, an ninh và tiếng ồn phải được nhân viên đọc rồi chuyển đến đúng tòa nhà/bộ phận. | Cư dân, CSKH, ban quản lý và đội kỹ thuật | Báo cáo thường niên 2024 cho biết Vinhomes Resident có khoảng 130.000 tài khoản và kết nối cư dân với Ban Quản lý; quy mô này khiến routing ticket là use case đáng khảo sát. Thời gian 3–5 phút/ticket vẫn là giả định cần xác minh. [Nguồn Vinhomes](https://gcp-cdn.vinhomes.vn/cms-data/VIE_Vinhomes%20AR%202024_250411_compressed.pdf) | Bộ phân loại kết hợp rule theo tòa/căn hộ với LLM nhận diện chủ đề, mức khẩn cấp và tóm tắt; tự route ticket có độ tin cậy cao, còn ticket nhạy cảm hoặc mơ hồ chuyển người duyệt. |
| 4 | Vinmec | Time-consuming | Phân loại yêu cầu đặt khám và gợi ý chuyên khoa/bác sĩ/lịch phù hợp trước khi chuyển nhân viên Contact Center xác nhận. | Bệnh nhân, nhân viên Contact Center, lễ tân | Form đặt lịch yêu cầu chọn cơ sở, chuyên khoa, bác sĩ, ngày khám và lý do khám; yêu cầu vẫn cần Contact Center xác nhận. [Nguồn Vinmec](https://www.vinmec.com/eng/booking/) | LLM tóm tắt lý do khám và đề xuất câu hỏi làm rõ; rule map sang chuyên khoa/lịch. Nhân viên xác nhận; AI không chẩn đoán bệnh, kê đơn hoặc tự đặt lịch. |
| 5 | Vinmec | Repetitive | Kiểm tra và chuẩn hóa hồ sơ tiếp nhận bệnh nhân, giấy tờ tùy thân, bảo hiểm, phiếu đồng ý dịch vụ và chứng từ thanh toán để giảm nhập liệu lặp lại. | Bệnh nhân, lễ tân, thu ngân, nhân viên bảo hiểm | Quy trình công khai gồm nhiều bước kiểm tra hồ sơ, định danh, ký phiếu xét nghiệm/điều trị và thanh toán; giả thuyết cần xác minh: 10–20 phút/ca cho phần giấy tờ và nhập liệu. [Nguồn Vinmec](https://www.vinmec.com/vie/bai-viet/quy-trinh-kham-chua-benh-tai-vinmec-vi) | OCR/LLM trích xuất dữ liệu thành bản nháp; rule kiểm tra trường bắt buộc và định dạng. Lễ tân xác minh giấy tờ gốc; AI không tự xác nhận bảo hiểm hay thay đổi hồ sơ bệnh án. |
| 6 | Vinpearl | Time-consuming | Nhân viên reservation cần kiểm tra yêu cầu lưu trú, hội họp và dịch vụ bổ sung của khách đoàn trước khi xác nhận dịch vụ. | Nhân viên reservation/sales, đối tác lữ hành, khách đoàn | Vinpearl định nghĩa khách đoàn từ 10 phòng/đêm hoặc 5 biệt thự/đêm, và có xác nhận dịch vụ khách đoàn; nhu cầu có nhiều trường dữ liệu cần kiểm tra. Thời gian xử lý là giả định cần xác minh. [Nguồn Vinpearl](https://vinpearl.com/vi/terms-of-use) | LLM trích xuất yêu cầu email thành JSON có dẫn nguồn; rule kiểm tra ngày, số khách, loại phòng và tồn phòng; nhân viên duyệt báo giá/booking draft trước khi xác nhận. |
| 7 | Xanh SM | Stakeholder Pain | Tài xế điện cần kênh hỗ trợ nhanh khi gặp sự cố vận hành hoặc có thông tin điều phối thiếu rõ ràng trong lúc đang phục vụ khách. | Tài xế, điều phối viên, hành khách | GSM là doanh nghiệp vận tải có tài xế ô tô điện và có hotline CSKH công khai. Kịch bản pin yếu và điều phối trạm sạc là bài tập mô phỏng trong lab; số sự cố và thời gian xử lý không có nguồn công khai, cần lấy từ log nội bộ. [Nguồn Xanh SM](https://tuyentaixe.xanhsm.com/gioithieu) | Rule xử lý tình huống an toàn; kết nối GPS/trạm sạc khi có quyền truy cập; LLM chỉ tạo tin nhắn nháp. Điều phối viên phê duyệt trước khi gửi hoặc điều xe cứu hộ. |

### Ghi chú về 4 lenses

- **Repetitive:** Tác vụ lặp đi lặp lại nhiều lần mỗi ngày.
- **Time-consuming:** Nhân viên mất nhiều thời gian xử lý thủ công.
- **AI-upgrade:** Dịch vụ hiện tại chậm, rập khuôn hoặc có thể cá nhân hóa tốt hơn.
- **Stakeholder Pain:** Khách hàng, nhân viên hoặc đối tác đang gặp khó chịu/bottleneck rõ ràng.

## 4. Phase 2 — QUICK-ASSESS

### Quick Problem Card #1

**Tên bài toán:** Phân loại yêu cầu đặt lịch bảo dưỡng/sửa chữa

**Bài toán trong một câu:** Khách hàng mô tả lỗi xe bằng tiếng Việt, nhưng nhân viên phải đọc, hỏi lại và phân loại thủ công để chọn đúng dịch vụ, xưởng hoặc lịch hẹn.

**Công ty thành viên:** VinFast

**Lens:** Time-consuming

**Ai đang đau (Actor/Stakeholder):** Khách hàng, nhân viên Contact Center, cố vấn dịch vụ.

**Workflow thủ công hiện tại:**

1. Khách hàng gửi yêu cầu qua ứng dụng hoặc gọi tổng đài.
2. Nhân viên đọc mô tả lỗi và hỏi thêm thông tin.
3. Nhân viên phân loại loại dịch vụ cần thực hiện.
4. Nhân viên kiểm tra xưởng/dịch vụ lưu động và thời gian phù hợp.
5. Nhân viên xác nhận lịch với khách hàng.

**Bước tốn thời gian hoặc dễ lỗi nhất:** Bước 2–4 — khoảng 10–20 phút/yêu cầu, là ước tính cần kiểm chứng.

**AI có thể hỗ trợ ở bước:** Bước 2–3: trích xuất triệu chứng, phân loại sơ bộ loại dịch vụ và đề xuất xưởng phù hợp. Nhân viên vẫn phải kiểm tra và xác nhận.

**Success metric có số:** Giảm thời gian phân loại ban đầu từ 10–20 phút xuống dưới 5 phút/yêu cầu; tỷ lệ chuyển đúng nhóm dịch vụ đạt tối thiểu 90%.

**Quick Architecture:** [ ] No AI  &nbsp;&nbsp; [ ] Rule  &nbsp;&nbsp; [x] LLM Feature  &nbsp;&nbsp; [ ] Agent

**Rủi ro hoặc điều cần kiểm chứng:** AI không được tự chẩn đoán lỗi nghiêm trọng hoặc tự xác nhận lịch; cần kiểm tra độ chính xác với các mô tả lỗi thực tế. Quy trình đặt dịch vụ công khai của VinFast gồm chọn loại dịch vụ, mô tả vấn đề, địa điểm và thời gian. [Nguồn VinFast](https://vinfastauto.com/vn_vi/cau-hoi-thuong-gap/cau-hoi-xe-o-to/chinh-sach-hau-mai)

### Quick Problem Card #2

**Tên bài toán:** Điều phối cứu hộ RSA khi xe hết pin

**Bài toán trong một câu:** Khi xe hết pin hoặc gặp sự cố giữa đường, nhân viên phải thu thập thông tin, xác minh vị trí và điều phối xe kéo thủ công, khiến khách hàng phải chờ lâu.

**Công ty thành viên:** VinFast

**Lens:** Stakeholder Pain

**Ai đang đau (Actor/Stakeholder):** Chủ xe, nhân viên tổng đài, đội cứu hộ.

**Workflow thủ công hiện tại:**

1. Khách hàng gọi tổng đài và báo xe gặp sự cố.
2. Nhân viên xác minh biển số, vị trí, mức pin và tình trạng xe.
3. Nhân viên kiểm tra điều kiện tiếp cận và điểm cứu hộ phù hợp.
4. Nhân viên điều phối xe kéo RSA.
5. Nhân viên thông báo hướng dẫn và thời gian dự kiến cho khách hàng.

**Bước tốn thời gian hoặc dễ lỗi nhất:** Bước 2–4 — khoảng 10–30 phút/lượt, là ước tính cần kiểm chứng.

**AI có thể hỗ trợ ở bước:** Bước 2–3: chuyển nội dung cuộc gọi thành dữ liệu có cấu trúc, kiểm tra thông tin còn thiếu và đề xuất phương án điều phối. Nhân viên phải phê duyệt trước khi điều xe.

**Success metric có số:** Giảm thời gian tiếp nhận và điều phối từ 20 phút xuống dưới 8 phút/lượt; tỷ lệ hồ sơ có đủ thông tin ngay lần đầu đạt 95%.

**Quick Architecture:** [ ] No AI  &nbsp;&nbsp; [x] Rule  &nbsp;&nbsp; [x] LLM Feature  &nbsp;&nbsp; [ ] Agent

**Rủi ro hoặc điều cần kiểm chứng:** AI không được tự động điều xe, không được đưa ra hướng dẫn thiếu an toàn và phải xác minh vị trí/điều kiện giao thông. VinFast đã công bố sử dụng cứu hộ bằng xe kéo RSA cho trường hợp xe hết pin từ ngày 06/06/2025. [Nguồn VinFast](https://vinfastauto.com/vn_vi/thong-bao-ve-viec-dieu-chinh-phuong-thuc-cuu-ho-cho-cac-truong-hop-xe-het-pin-tu-ngay-06062025)

### Quick Problem Card #3

**Tên bài toán:** Gợi ý chuyên khoa và lịch khám phù hợp

**Bài toán trong một câu:** Bệnh nhân mô tả lý do khám chưa rõ ràng, khiến nhân viên phải đọc, hỏi lại và xác định chuyên khoa/bác sĩ trước khi xác nhận lịch.

**Công ty thành viên:** Vinmec

**Lens:** Time-consuming

**Ai đang đau (Actor/Stakeholder):** Bệnh nhân, nhân viên Contact Center, lễ tân.

**Workflow thủ công hiện tại:**

1. Bệnh nhân gửi yêu cầu đặt lịch và mô tả lý do khám.
2. Nhân viên đọc nội dung và gọi lại nếu thông tin chưa đầy đủ.
3. Nhân viên xác định chuyên khoa hoặc bác sĩ phù hợp.
4. Nhân viên kiểm tra ngày giờ còn trống.
5. Nhân viên gọi lại để xác nhận lịch khám.

**Bước tốn thời gian hoặc dễ lỗi nhất:** Bước 2–4 — khoảng 10–15 phút/yêu cầu, là ước tính cần kiểm chứng.

**AI có thể hỗ trợ ở bước:** Bước 2–3: tóm tắt lý do khám, gợi ý chuyên khoa và tạo danh sách câu hỏi cần hỏi thêm. Nhân viên Contact Center chịu trách nhiệm xác nhận cuối cùng.

**Success metric có số:** Giảm thời gian xử lý yêu cầu từ 10–15 phút xuống dưới 4 phút/yêu cầu; tỷ lệ gợi ý đúng chuyên khoa đạt tối thiểu 90%.

**Quick Architecture:** [ ] No AI  &nbsp;&nbsp; [x] Rule  &nbsp;&nbsp; [x] LLM Feature  &nbsp;&nbsp; [ ] Agent

**Rủi ro hoặc điều cần kiểm chứng:** AI chỉ hỗ trợ phân loại hành chính, không được chẩn đoán bệnh, tư vấn điều trị hoặc tự đặt lịch. Form đặt lịch Vinmec yêu cầu cơ sở, chuyên khoa, bác sĩ, ngày khám và lý do khám; Contact Center vẫn xác nhận lịch cuối cùng. [Nguồn Vinmec](https://www.vinmec.com/eng/booking/)

## 5. Lựa chọn bài toán cho Deep Dive

**Bài toán nhóm chọn:** Card #1 — Phân loại yêu cầu đặt lịch bảo dưỡng/sửa chữa VinFast.

**Lý do lựa chọn:**

- Quy trình công khai đã thể hiện rõ các trường đầu vào: loại dịch vụ, mô tả sự cố, địa điểm, thời gian; VinFast phân luồng sửa tại xưởng hoặc Mobile Service tùy tình trạng xe và khả năng phục vụ tại địa điểm. Điều này giúp nhóm vẽ được workflow hiện tại mà không phải suy đoán toàn bộ quy trình nội bộ.
- Bottleneck có thể đo trực tiếp bằng thời gian từ lúc yêu cầu được tạo đến khi nhân viên xác nhận nhóm dịch vụ; pilot cần thiết lập baseline trước, vì số 10–20 phút hiện chỉ là giả định.
- Có thể tạo bộ dữ liệu thử nghiệm đã khử định danh gồm mô tả lỗi, loại xe, địa điểm, loại dịch vụ được cố vấn chọn và kết quả lịch hẹn; ngay cả khi chưa có dữ liệu thật, nhóm có thể dùng case giả lập để đánh giá format và safety boundary của prompt.
- Rule phù hợp với kiểm tra trường bắt buộc, vị trí và lịch; LLM phù hợp để hiểu mô tả tiếng Việt không chuẩn, tóm tắt và đề xuất nhóm dịch vụ. Cố vấn dịch vụ phê duyệt quyết định cuối, nên phạm vi AI hẹp và kiểm soát được.

**Lý do không chọn Card #2:** Cứu hộ hết pin có rủi ro an toàn cao và chính sách công khai hiện áp dụng RSA cho xe cá nhân, đồng thời loại trừ xe GSM. Nhóm cần xác minh playbook theo từng loại xe, điều kiện giao thông và quyền điều phối trước khi dùng AI trong luồng này.

**Lý do không chọn Card #3:** Phân loại nhu cầu khám liên quan dữ liệu sức khỏe và có thể ảnh hưởng đến an toàn bệnh nhân. Use case chỉ nên bắt đầu sau khi có phê duyệt về dữ liệu, rubric chuyên môn do bác sĩ xây dựng và cơ chế đánh giá sai lệch.

**Câu hỏi cần xác minh trong Deep Dive:**

1. Những loại dịch vụ và điều kiện nào cho phép Mobile Service, và quy tắc nào buộc phải chuyển xe tới xưởng?
2. Tỷ lệ yêu cầu hiện bị hỏi lại, phân loại sai hoặc đổi lịch là bao nhiêu; thời gian trung vị và p90 từ tạo yêu cầu đến xác nhận là bao lâu?
3. Trường dữ liệu nào có thể dùng để tạo bộ đánh giá đã khử định danh, ai có quyền truy cập và thời gian lưu trữ bao lâu?
4. Confidence threshold nào cần chuyển yêu cầu cho cố vấn; fallback khi LLM không đủ thông tin là gì?
5. Những mô tả triệu chứng nào bắt buộc đánh dấu khẩn cấp và không được để AI gợi ý lịch hoặc dịch vụ?

## 6. Nguồn research công khai đã dùng

- VinFast mô tả quy trình đặt dịch vụ trên ứng dụng, gồm chọn dịch vụ, mô tả, địa điểm, thời gian và xác nhận: [Hướng dẫn tiện ích dịch vụ VinFast](https://vinfastauto.com/vn_vi/huong-dan-su-dung-tien-ich-dich-vu-tren-ung-dung-vinfast).
- VinFast thông báo từ 06/06/2025 dùng cứu hộ xe kéo RSA cho xe cá nhân hết pin; chính sách không áp dụng cho xe GSM/đối tác vận tải: [Thông báo cứu hộ hết pin](https://vinfastauto.com/vn_vi/thong-bao-ve-viec-dieu-chinh-phuong-thuc-cuu-ho-cho-cac-truong-hop-xe-het-pin-tu-ngay-06062025).
- Vinhomes công bố quy mô và vai trò kết nối của Vinhomes Resident: [Báo cáo thường niên Vinhomes 2024](https://gcp-cdn.vinhomes.vn/cms-data/VIE_Vinhomes%20AR%202024_250411_compressed.pdf).
- Vinmec có luồng đặt lịch theo cơ sở, chuyên khoa, bác sĩ, ngày khám và lý do khám: [Trang đặt lịch Vinmec](https://www.vinmec.com/eng/booking/).
- Vinpearl có định nghĩa và quy trình xác nhận dịch vụ cho khách đoàn: [Điều khoản sử dụng Vinpearl](https://vinpearl.com/vi/terms-of-use).
- Xanh SM công khai thông tin doanh nghiệp và kênh hỗ trợ tài xế/khách hàng; các chi tiết điều phối trong bài vẫn phải kiểm chứng nội bộ: [Giới thiệu Xanh SM](https://tuyentaixe.xanhsm.com/gioithieu).
