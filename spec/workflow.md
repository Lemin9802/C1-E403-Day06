# Workflow — Long Châu Safety Bot

> Tài liệu workflow Day 6, mô tả cách nhóm phối hợp, luồng sản phẩm và các output cần có để demo prototype.

---

## 1. Tổng quan dự án

**Long Châu Safety Bot** là prototype AI hỗ trợ người dùng tra cứu an toàn thuốc trong bối cảnh nhà thuốc online.

Prototype tập trung vào một lát cắt hẹp:

```text
User nhập tình trạng sức khỏe + tuổi + giới tính + tên thuốc/hoạt chất
→ bot tra DB local / fuzzy matching / API fallback
→ trả Safety Card có mức cảnh báo, nguồn, disclaimer
→ chuyển dược sĩ hoặc kênh y tế phù hợp nếu không chắc hoặc có rủi ro.
```

Sản phẩm không chẩn đoán bệnh, không kê đơn, không đổi liều và không thay thế dược sĩ/bác sĩ. Vai trò của AI là hỗ trợ tra cứu, chuẩn hóa thông tin và cảnh báo rủi ro trong phạm vi demo.

---

## 2. Vấn đề cần giải quyết

Người dùng Long Châu có thể đang cầm thuốc, chuẩn bị mua thuốc hoặc muốn kiểm tra nhanh một hoạt chất. Họ thường gặp các vấn đề sau:

- Không chắc thuốc/hoạt chất có phù hợp với tình trạng hiện tại không.
- Chat dược sĩ có thể phải chờ, trong khi user cần bước kiểm tra nhanh ban đầu.
- Thông tin trên Internet phân tán, khó kiểm chứng và dễ gây hiểu nhầm.
- AI generic có thể trả lời tự tin nhưng không có nguồn hoặc không biết dừng khi thiếu dữ liệu.
- Với thuốc, nếu bot trả lời sai hoặc bỏ sót cảnh báo, hậu quả có thể nghiêm trọng.

Vì vậy, nhóm chọn hướng **Safety Card có nguồn + cảnh báo + disclaimer + CTA hỏi dược sĩ** thay vì chatbot y tế đa năng.

---

## 3. Mục tiêu sản phẩm Day 6

Prototype Day 6 cần chứng minh được:

1. User có thể nhập tình trạng, tuổi, giới tính và tên thuốc/hoạt chất.
2. Bot tìm thuốc trong DB demo hoặc gợi ý khi user gõ sai.
3. Bot tạo Safety Card có cấu trúc rõ ràng.
4. Bot gắn mức cảnh báo Xanh / Vàng / Đỏ hoặc trạng thái không chắc.
5. Bot không bịa thông tin khi thiếu dữ liệu hoặc khi API lỗi.
6. Bot dừng flow thường khi phát hiện triệu chứng khẩn cấp.
7. Bot có đường recovery khi user sửa tên thuốc hoặc tình trạng.

---

## 4. Phạm vi MVP

### 4.1. Có trong phạm vi

- Chat UI mô phỏng Long Châu.
- Nhập tình trạng sức khỏe, tuổi, giới tính, tên thuốc/hoạt chất.
- DB thuốc demo trong `codebase/data/drugs-demo.json`.
- Fuzzy matching tên thuốc bằng Fuse.js.
- Safety Card từ DB local.
- Rule đối chiếu tình trạng / tuổi / giới tính.
- Urgent keyword detection.
- API fallback OpenAI / DeepSeek / Gemini nếu có key.
- OCR endpoint ở mức demo phụ.
- Demo happy path, low-confidence path, failure path và correction path.

### 4.2. Không nằm trong phạm vi

- Không kê đơn thuốc.
- Không chẩn đoán bệnh.
- Không tự động đổi liều thuốc.
- Không thay thế dược sĩ hoặc bác sĩ.
- Không tích hợp production API Long Châu.
- Không xử lý đầy đủ tương tác nhiều thuốc như hệ thống dược lâm sàng thật.
- Không tự động đặt hàng hoặc thanh toán.

---

## 5. Luồng sản phẩm

```text
1. User mở prototype Long Châu Safety Bot.
2. User nhập tình trạng sức khỏe.
3. User nhập tuổi và giới tính.
4. User nhập tên thuốc hoặc hoạt chất.
5. Bot kiểm tra red flag / triệu chứng khẩn cấp.
6. Nếu có red flag → dừng flow tra cứu thường, hướng gọi 115 / đến cơ sở y tế / hỏi dược sĩ.
7. Nếu không có red flag → bot tìm thuốc trong DB local.
8. Nếu tên thuốc gõ sai hoặc mơ hồ → bot gợi ý fuzzy và yêu cầu user xác nhận.
9. Nếu tìm thấy thuốc → bot tạo Safety Card.
10. Bot đối chiếu rule với tình trạng, tuổi, giới tính.
11. Bot trả mức cảnh báo Xanh / Vàng / Đỏ hoặc Không chắc.
12. Nếu Vàng / Đỏ / Không chắc → bot khuyến nghị hỏi dược sĩ.
13. Nếu user sửa tên thuốc hoặc tình trạng → bot tạo lại Safety Card mới và bỏ kết quả cũ.
```

---

## 6. Four paths cần demo

| Path | Input ví dụ | Prototype phải thể hiện |
|---|---|---|
| Happy path | `sốt nhẹ`, `25 tuổi`, `nam`, `Paracetamol` | Tìm thấy thuốc trong DB, trả Safety Card có nguồn, disclaimer và mức cảnh báo phù hợp. |
| Low-confidence path | `Panadl` hoặc `Panadol` | Không đoán bừa; show gợi ý fuzzy hoặc yêu cầu user chọn thuốc/hoạt chất đúng. |
| Failure / urgent path | `khó thở, sưng mặt sau khi uống thuốc`, `Ibuprofen` | Dừng flow tra cứu thường, hướng gọi 115 / đến cơ sở y tế / hỏi dược sĩ. |
| Correction path | User đổi `Ibuprofen` thành `Paracetamol` | Bỏ Safety Card cũ, tạo Safety Card mới, log correction ở mức session/demo. |

---

## 7. Phân công công việc nhóm Day 6

| Thành viên | GitHub / email | Vai trò chính | Công việc phụ trách |
|---|---|---|---|
| Phùng Văn Thạch | `ThachPhung` | Prototype owner chính | Xây dựng flow Long Châu Safety Bot, chỉnh chatbot logic, xử lý intent/symptom flow, API client, OCR/image input, deploy entrypoint và hỗ trợ demo kỹ thuật. |
| Lương Quốc Đoàn | `luongdoan305` | UX/UI owner | Chỉnh giao diện, logo/assets, visual polish, làm prototype gần phong cách Long Châu hơn, hỗ trợ trải nghiệm người dùng trong flow tra cứu thuốc và Safety Card. |
| Trịnh Vũ Anh Tuấn | `hannuta69@gmail.com` | Rule/DB owner | Cập nhật `drugs-demo.json`, bổ sung thuốc, aliases, chống chỉ định, warnings, `conditionRules`, `genderRules`, source demo và rule cảnh báo Xanh/Vàng/Đỏ. |
| Hoàng Phương Thảo | `pthaoxinhgai` | Workflow/documentation support | Viết và chỉnh workflow, hỗ trợ tài liệu Day 5/Day 6, kiểm tra luồng trải nghiệm, hỗ trợ submission hygiene như `.gitignore` để tránh commit file nhạy cảm. |
| Thái Thị Yến Nhi | `Lemin9802` | Repo/submission owner | Đại diện nhóm nộp repo, gom code về repo nộp, merge branch/PR, kiểm tra cấu trúc repo, cập nhật README, SPEC Day 6, failure/correction path và chuẩn hóa nội dung nộp. |

---

## 8. Cách phối hợp

```text
1. Thống nhất thin spec và build slice.
2. Thạch dựng prototype flow chính và logic chatbot.
3. Tuấn cập nhật DB/rule để prototype có dữ liệu tra cứu.
4. Đoàn chỉnh UI/UX, logo/assets và giao diện Long Châu-style.
5. Thảo viết workflow, kiểm tra tài liệu và submission hygiene.
6. Nhi gom repo, cập nhật README/SPEC, kiểm tra file/folder đúng format nộp.
7. Cả nhóm test 4 paths: happy, low-confidence, failure, correction.
8. Nhóm chụp screenshot/log demo và chuẩn bị thuyết trình.
```

---

## 9. Output cần có trong repo

| Output | Vị trí | Người phụ trách chính |
|---|---|---|
| README tổng | `README.md` | Thái Thị Yến Nhi |
| Thin SPEC Day 5 | `spec/thin-spec-day5.md` | Thái Thị Yến Nhi + Phùng Văn Thạch |
| SPEC chính thức Day 6 | `spec/spec-day06.md` | Thái Thị Yến Nhi + Phùng Văn Thạch |
| Workflow | `spec/workflow.md` | Hoàng Phương Thảo |
| Prototype code | `codebase/` | Phùng Văn Thạch |
| UI/assets | `codebase/` | Lương Quốc Đoàn |
| DB/rules | `codebase/data/drugs-demo.json` | Trịnh Vũ Anh Tuấn |
| Codebase README | `codebase/README.md` | Nhóm |
| Reflection cá nhân | `reflection/` | Từng thành viên |

---

## 10. Tiêu chí hoàn thành trước khi nộp

- Repo có `README.md`, `spec/`, `codebase/`, `reflection/`.
- README tổng có thành viên, mã học viên, phân công, mô tả sản phẩm, cách chạy.
- `spec/` có `thin-spec-day5.md`, `spec-day06.md`, `workflow.md`.
- `codebase/` chạy được bằng `npm install` và `npm run dev`.
- Không commit `.env` thật.
- Có `.env.example` để hướng dẫn cấu hình.
- Prototype demo được happy path và ít nhất một path phục hồi/failure.
- Phân công trong README, workflow và commit history không mâu thuẫn.

---

## 11. Câu chốt workflow

```text
Nhóm không build AI bác sĩ.
Nhóm build một prototype Safety Bot có giới hạn rõ:
tra một thuốc/hoạt chất trong một bối cảnh sức khỏe cụ thể,
trả Safety Card có nguồn và biết dừng khi không chắc.
```
