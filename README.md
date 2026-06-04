# C1-E403-Day06 — Long Châu Safety Bot

## 1. Thông tin nhóm

**Lớp:** E403  
**Nhóm:** C1  
**Track:** Healthcare / Pharmacy  
**Sản phẩm:** Long Châu Safety Bot  
**App tham chiếu:** Long Châu — Chuyên gia thuốc  

---

## 2. Thành viên và phân công công việc

| Mã học viên | Họ và tên | GitHub ID | Phân công công việc Day 6 |
|---|---|---|---|
| 2A202601004 | Phùng Văn Thạch | `ThachPhung` | **Prototype owner chính.** Xây dựng flow Long Châu Safety Bot, chỉnh logic chatbot, cập nhật UI chat-only, xử lý intent/symptom flow, API client, OCR/image input, deploy entrypoint và hỗ trợ demo kỹ thuật. |
| 2A202600564 | Lương Quốc Đoàn | `luongdoan305` | **UX/UI owner.** Chỉnh giao diện, logo/assets, visual polish, làm prototype gần phong cách Long Châu hơn, hỗ trợ trải nghiệm người dùng trong flow tra cứu thuốc và Safety Card. |
| 2A202600847 | Trịnh Vũ Anh Tuấn | `hannuta69@gmail.com` | **Rule/DB owner.** Cập nhật `drugs-demo.json`, bổ sung thuốc, aliases, chống chỉ định, warnings, `conditionRules`, `genderRules`, source demo và rule cảnh báo Xanh/Vàng/Đỏ. |
| 2A202600951 | Hoàng Phương Thảo | `pthaoxinhgai` | **Workflow/documentation support.** Hỗ trợ workflow, tài liệu Day 5/Day 6, kiểm tra luồng trải nghiệm, hỗ trợ submission hygiene như `.gitignore` để tránh commit file nhạy cảm. |
| 2A202600783 | Thái Thị Yến Nhi | `Lemin9802` | **Repo/submission owner.** Đại diện nhóm nộp repo, gom code về repo nộp, merge branch/PR, kiểm tra cấu trúc repo, cập nhật README, SPEC Day 6, failure/correction path và chuẩn hóa nội dung nộp. |

---

## 3. Mô tả ngắn sản phẩm

**Long Châu Safety Bot** là prototype AI hỗ trợ người dùng tra cứu an toàn thuốc trong bối cảnh nhà thuốc online.

Lát cắt sản phẩm:

```text
User nhập tình trạng sức khỏe + tuổi + giới tính + tên thuốc/hoạt chất
→ bot tra DB local / fuzzy matching / API fallback
→ trả Safety Card có mức cảnh báo, nguồn, disclaimer
→ chuyển dược sĩ hoặc kênh y tế phù hợp nếu không chắc hoặc có rủi ro.
```

Prototype **không chẩn đoán bệnh**, **không kê đơn**, **không đổi liều**, và **không thay thế dược sĩ/bác sĩ**.

---

## 4. Cách chạy prototype

Toàn bộ code prototype nằm trong folder:

```text
codebase/
```

### 4.1. Yêu cầu môi trường

```text
Node.js 18+
npm
```

### 4.2. Chạy local

```bash
cd codebase
npm install
cp .env.example .env
npm run dev
```

Sau đó mở trình duyệt tại:

```text
http://localhost:3000
```

### 4.3. Biến môi trường

Prototype có thể chạy bằng **local demo DB** mà không cần API key.  
Nếu muốn bật các provider AI/API fallback, điền key trong file `.env`.

Ví dụ `.env`:

```env
AI_PROVIDER=auto
OPENAI_API_KEY=
DEEPSEEK_API_KEY=
GEMINI_API_KEY=
GOOGLE_API_KEY=
GOOGLE_CSE_ID=
PORT=3000
```

Lưu ý:

```text
Không commit file .env thật.
Chỉ commit .env.example.
```

---

## 5. Công cụ và API đã dùng

### 5.1. Framework / thư viện

| Thành phần | Công cụ |
|---|---|
| Frontend | HTML, CSS, JavaScript |
| Backend | Node.js, Express |
| Fuzzy matching tên thuốc | Fuse.js |
| Database demo | `data/drugs-demo.json` |
| UI style | Mock giao diện Long Châu |
| Runtime | npm / Node.js |

### 5.2. AI / API optional

| API / Model | Mục đích |
|---|---|
| OpenAI | Fallback chat / trả lời khi cần AI provider |
| DeepSeek | Fallback chat / reasoning provider |
| Gemini | Fallback AI + hỗ trợ OCR nếu cấu hình |
| Google CSE | Tìm kiếm nguồn ngoài nếu được bật |
| OCR endpoint | Nhận ảnh thuốc / nhãn thuốc để thử flow nhận diện |

### 5.3. Các endpoint chính

| Method | Endpoint | Mục đích |
|---|---|---|
| GET | `/api/drugs/health` | Kiểm tra trạng thái DB, AI provider, Google CSE |
| GET | `/api/drugs/suggest?q=...` | Gợi ý tên thuốc khi user gõ sai |
| POST | `/api/drugs/lookup` | Tra thuốc/tình trạng và trả Safety Card |
| POST | `/api/drugs/chat` | Chat với AI provider nếu có cấu hình |
| POST | `/api/drugs/ocr` | OCR ảnh thuốc / nhãn thuốc |
| POST | `/api/drugs/search-google` | Debug Google CSE |

---

## 6. Demo cases

| Case | Input | Kỳ vọng |
|---|---|---|
| Happy path | `sốt nhẹ`, `25 tuổi`, `nam`, `Paracetamol` | Bot tìm thấy thuốc trong DB và trả Safety Card có nguồn, cảnh báo, disclaimer |
| Low-confidence | Gõ sai thuốc: `Panadl` | Bot không đoán bừa, gợi ý thuốc gần đúng để user xác nhận |
| Failure / urgent | `khó thở, sưng mặt sau khi uống thuốc` + `Ibuprofen` | Bot dừng flow tra cứu thường và hướng user gọi 115 / đến cơ sở y tế / hỏi dược sĩ |
| Correction | Đổi thuốc từ `Ibuprofen` sang `Paracetamol` | Bot bỏ kết quả cũ và tạo lại Safety Card mới |

---

## 7. Cấu trúc repo

```text
C1-E403-Day06/
├── README.md
├── spec/
│   ├── thin-spec-day5.md
│   └── spec-day06.md
├── codebase/
│   ├── README.md
│   ├── .env.example
│   ├── package.json
│   ├── index.html
│   ├── css/
│   │   └── styles.css
│   ├── data/
│   │   └── drugs-demo.json
│   ├── js/
│   │   ├── app.js
│   │   ├── api-client.js
│   │   ├── drug-engine.js
│   │   └── local-lookup.js
│   └── server/
│       ├── index.js
│       ├── drug-db.js
│       ├── conversation.js
│       ├── ai-provider.js
│       └── routes/
│           └── drugs.js
└── reflection/
    └── reflection cá nhân
```

---

## 8. Link tài liệu

- Thin SPEC Day 5: [`spec/thin-spec-day5.md`](spec/thin-spec-day05.md)
- SPEC Day 6: [`spec/spec.md`](spec/spec-day06.md)
- Code prototype: [`codebase/`](codebase/)
- Hướng dẫn chạy code: [`codebase/README.md`](codebase/README.md)
- Reflection cá nhân: [`reflection/`](reflection/)

---

## 9. Giới hạn của prototype

Prototype hiện tại chỉ phục vụ mục tiêu demo Day 6, nên có một số giới hạn:

- Không kê đơn thuốc.
- Không chẩn đoán bệnh.
- Không thay thế dược sĩ hoặc bác sĩ.
- Không tự động quyết định người dùng có được dùng thuốc hay không.
- Không tích hợp production API của Long Châu.
- Không xử lý đầy đủ tương tác nhiều thuốc như một hệ thống dược lâm sàng thật.
- Dữ liệu thuốc trong `drugs-demo.json` chỉ là DB demo phục vụ prototype.

---

## 10. Câu chốt sản phẩm

Long Châu Safety Bot không cố trở thành “AI bác sĩ”. Nhóm chỉ build một lát cắt hẹp: khi user có một tình trạng sức khỏe, tuổi, giới tính và một thuốc/hoạt chất cần tra, bot tạo Safety Card có nguồn, cảnh báo an toàn và CTA hỏi dược sĩ. Giá trị chính của prototype là trả lời có giới hạn, có căn cứ và biết dừng khi không chắc.