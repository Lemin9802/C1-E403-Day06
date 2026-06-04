# Reflection cá nhân — Thái Thị Yến Nhi

**Họ và tên:** Thái Thị Yến Nhi  
**Mã học viên:** 2A202600783  
**Nhóm:** C1 — E403  
**Sản phẩm:** Long Châu Safety Bot  

---

## 1. Vai trò của tôi trong nhóm

Trong Day 6, tôi phụ trách vai trò **repo/submission owner** và hỗ trợ phần **SPEC, failure/correction path** của sản phẩm.

Cụ thể, tôi đảm nhận các việc:

- Đại diện nhóm gom code từ repo làm việc chung về repo nộp chính thức.
- Kiểm tra và chỉnh lại cấu trúc repo theo format yêu cầu: `README.md`, `spec/`, `codebase/`, `reflection/`.
- Cập nhật README tổng của repo: thông tin nhóm, phân công công việc, mô tả sản phẩm, cách chạy prototype, công cụ/API đã dùng và link tài liệu.
- Rà soát lại Thin SPEC Day 5 và SPEC Day 6 để đảm bảo nội dung đồng nhất với prototype thực tế.
- Kiểm tra các path quan trọng của sản phẩm AI: happy path, low-confidence path, failure path và correction path.
- Chuẩn hóa nội dung nộp để repo dễ đọc, dễ chấm và phản ánh đúng đóng góp của từng thành viên.

---

## 2. Việc tôi đã làm trong Day 6

### 2.1. Gom và chuẩn hóa repo nộp

Nhóm ban đầu làm việc trên nhiều branch và có một số folder/file từ Day 5 còn nằm lẫn trong repo. Tôi đã kiểm tra lại cấu trúc repo và sắp xếp nội dung theo format nộp Day 6:

```text
README.md
spec/
codebase/
reflection/
```

Tôi cũng kiểm tra để tránh các lỗi như:

- Link tài liệu trong README bị sai.
- Folder prototype còn tên cũ.
- File Day 5 cũ bị lẫn với file nộp Day 6.
- README mô tả không đúng code prototype hiện tại.
- Thiếu thông tin thành viên hoặc phân công công việc.

### 2.2. Cập nhật README tổng

Tôi viết lại README tổng để người chấm có thể nhanh chóng hiểu:

- Nhóm là ai.
- Sản phẩm giải quyết vấn đề gì.
- Prototype chạy như thế nào.
- Công cụ/API nào đã dùng.
- Ai phụ trách phần nào.
- Tài liệu SPEC, workflow và codebase nằm ở đâu.

Phần phân công được tôi chỉnh lại theo đóng góp thực tế trong commit, ví dụ:

- Thạch phụ trách chính prototype flow và logic kỹ thuật.
- Đoàn phụ trách UX/UI và logo/assets.
- Tuấn Anh phụ trách DB/rule safety.
- Phương Thảo phụ trách workflow/documentation.
- Tôi phụ trách repo, SPEC, failure/correction path và nội dung nộp.

### 2.3. Rà soát SPEC và workflow

Tôi kiểm tra Thin SPEC Day 5 và SPEC Day 6 để đảm bảo hai file không mâu thuẫn với nhau.

Tôi tập trung kiểm tra các điểm:

- Sản phẩm không bị mô tả quá rộng thành “AI bác sĩ”.
- Lát cắt build vẫn giữ hẹp: một tình trạng + một thuốc/hoạt chất → Safety Card.
- AI chỉ hỗ trợ tra cứu và cảnh báo, không kê đơn hay chẩn đoán.
- Có đủ bốn đường đi trải nghiệm: happy, low-confidence, failure, correction.
- Có phần trust/safety rõ ràng khi AI không chắc hoặc khi input có triệu chứng nguy hiểm.

---

## 3. Phần AI đã hỗ trợ tôi

Tôi có sử dụng AI để hỗ trợ trong quá trình chuẩn hóa tài liệu và kiểm tra logic sản phẩm.

AI hỗ trợ tôi ở các việc:

- Gợi ý cấu trúc README đúng với yêu cầu nộp repo Day 6.
- Tóm tắt và viết lại SPEC Day 6 từ Thin SPEC Day 5 và code prototype hiện tại.
- Rà soát sự khác nhau giữa các file cũ như `thin-spec-template.md`, `day06_spec.md` và file SPEC chính thức.
- Viết lại phần phân công công việc sao cho rõ vai trò từng thành viên.
- Gợi ý nội dung cho workflow, demo cases và reflection cá nhân.
- Kiểm tra các lỗi dễ bị sót như link sai, folder cũ, tên file không khớp hoặc README còn chỉ dẫn sai đường dẫn.

Tuy nhiên, tôi không copy máy móc. Tôi kiểm tra lại nội dung AI gợi ý với repo thật, commit thật và yêu cầu nộp bài. Những phần nào chưa khớp với sản phẩm nhóm, tôi chỉnh lại cho đúng với prototype hiện tại.

---

## 4. Điều tôi hiểu rõ để giải thích khi demo

Sau khi làm Day 6, tôi hiểu rõ hơn vì sao nhóm không nên build một chatbot y tế quá rộng.

Điểm cốt lõi của sản phẩm là:

```text
Long Châu Safety Bot không thay thế bác sĩ/dược sĩ.
Bot chỉ giúp user tra cứu an toàn thuốc trong một lát cắt hẹp,
có nguồn, có cảnh báo và biết chuyển sang người thật khi không chắc.
```

Tôi có thể giải thích các quyết định sản phẩm sau:

- Vì sao cần Safety Card thay vì câu trả lời chat tự do.
- Vì sao bot phải có disclaimer và không được nói “bạn uống được”.
- Vì sao input cần tình trạng, tuổi, giới tính và tên thuốc/hoạt chất.
- Vì sao tên thuốc mơ hồ hoặc gõ sai phải đi qua fuzzy suggestion.
- Vì sao triệu chứng khẩn cấp như khó thở, sưng mặt, đau ngực phải dừng flow tra cứu thường.
- Vì sao correction path quan trọng: khi user sửa thuốc hoặc tình trạng, bot phải tạo lại kết quả mới thay vì tiếp tục dựa trên thông tin cũ.

---

## 5. Failure mode tôi quan tâm nhất

Failure mode tôi quan tâm nhất là:

```text
Bot nhận nhầm tên thuốc hoặc đánh giá cảnh báo quá thấp,
khiến user hiểu nhầm rằng thuốc an toàn để dùng.
```

Đây là lỗi nguy hiểm vì người dùng có thể tự ý uống thuốc có chống chỉ định hoặc bỏ qua dấu hiệu cần hỏi dược sĩ/bác sĩ.

Prototype xử lý bằng các cách:

- Gợi ý fuzzy khi user gõ sai tên thuốc.
- Không tự chọn thuốc nếu tên thuốc mơ hồ.
- Không tạo Safety Card nếu không có dữ liệu đáng tin.
- Luôn hiển thị disclaimer.
- Có CTA hỏi dược sĩ.
- Với triệu chứng khẩn cấp, bot dừng flow thường và hướng user đến hỗ trợ y tế.

---

## 6. Bài học sau demo

Qua Day 6, tôi rút ra một số bài học:

1. **Sản phẩm AI không chỉ là prompt tốt.**  
   Một prototype AI cần có workflow, fallback, failure path và cách người dùng sửa sai.

2. **Trong lĩnh vực y tế, biết dừng quan trọng hơn trả lời nhiều.**  
   Bot không nên cố trả lời mọi câu hỏi. Khi thiếu dữ liệu hoặc có dấu hiệu nguy hiểm, sản phẩm phải chuyển sang dược sĩ/bác sĩ.

3. **SPEC phải bám sát code thật.**  
   Nếu SPEC mô tả một tính năng chưa có trong prototype, khi demo sẽ rất dễ bị lệch. Vì vậy tôi học được cách đối chiếu SPEC với repo và cập nhật lại tài liệu theo sản phẩm thật.

4. **Repo nộp bài cũng là một phần của sản phẩm.**  
   Một repo rõ ràng, có README, spec, codebase và reflection đầy đủ sẽ giúp người khác hiểu nhóm đã làm gì và từng thành viên đóng góp ra sao.

5. **AI hỗ trợ tốt nhất khi mình biết kiểm chứng.**  
   AI giúp viết nhanh hơn, nhưng người làm sản phẩm vẫn phải kiểm tra logic, bằng chứng, code và yêu cầu nộp bài.

---

## 7. Kết luận cá nhân

Vai trò của tôi trong nhóm không phải là người code nhiều nhất, mà là người giúp gom, kiểm tra và chuẩn hóa sản phẩm để nhóm có thể nộp một repo hoàn chỉnh.

Sau Day 6, tôi hiểu rõ hơn cách biến một ý tưởng AI thành một prototype có thể demo: phải có vấn đề cụ thể, lát cắt nhỏ, AI decision rõ, failure path rõ và tài liệu đủ để người khác chạy được sản phẩm.