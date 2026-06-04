# Long Châu Safety Bot — Prototype

Prototype chat tra cứu thuốc + Safety Card.  
Hỗ trợ DB local, fuzzy matching tên thuốc và optional AI provider: OpenAI / DeepSeek / Gemini.

---

## 1. Cách chạy local

Từ root repo:

```bash
cd codebase
npm install
cp .env.example .env
npm run dev
```

Mở trình duyệt tại:

```text
http://localhost:3000
```

---

## 2. Yêu cầu môi trường

```text
Node.js 18+
npm
```

---

## 3. Cấu hình API

Prototype có thể chạy bằng **DB local** mà không cần API key.

Nếu muốn bật AI/API fallback, điền key trong file `.env`.

```env
AI_PROVIDER=auto
OPENAI_API_KEY=
DEEPSEEK_API_KEY=
GEMINI_API_KEY=
GOOGLE_API_KEY=
GOOGLE_CSE_ID=
PORT=3000
```

Không commit file `.env` thật. Chỉ commit `.env.example`.

---

## 4. Công cụ sử dụng

| Thành phần | Công cụ |
|---|---|
| Frontend | HTML, CSS, JavaScript |
| Backend | Node.js, Express |
| Fuzzy matching | Fuse.js |
| Local DB | `data/drugs-demo.json` |
| Optional AI provider | OpenAI / DeepSeek / Gemini |
| Optional search | Google CSE |
| Optional OCR | OCR endpoint |

---

## 5. Logic tra cứu

```text
1. Thuốc có trong DB local
   → trả Safety Card từ database, không gọi API.

2. User gõ sai tên thuốc
   → gợi ý fuzzy từ DB trước, không đoán bừa.

3. Thuốc không có trong DB
   → fallback sang OpenAI / DeepSeek / Gemini nếu có API key.

4. Không có dữ liệu hoặc API lỗi
   → không bịa Safety Card, chuyển user sang hỏi dược sĩ.
```

---

## 6. API endpoints

| Method | Path | Mô tả |
|---|---|---|
| GET | `/api/drugs/health` | Kiểm tra trạng thái DB / AI provider |
| GET | `/api/drugs/suggest?q=...` | Gợi ý tên thuốc khi user gõ sai |
| POST | `/api/drugs/lookup` | Tra thuốc và trả Safety Card |
| POST | `/api/drugs/chat` | Chat với AI provider nếu có cấu hình |
| POST | `/api/drugs/ocr` | OCR ảnh thuốc / nhãn thuốc |
| POST | `/api/drugs/search-google` | Debug Google CSE |

Request chính cho `/api/drugs/lookup`:

```json
{
  "condition": "sốt nhẹ",
  "drugQuery": "Paracetamol",
  "age": 25,
  "gender": "nam"
}
```

---

## 7. Demo cases

### Happy path

```text
condition: sốt nhẹ
age: 25
gender: nam
drugQuery: Paracetamol
```

Kỳ vọng: bot tìm thấy thuốc trong DB và trả Safety Card.

### Low-confidence / typo

```text
condition: sốt nhẹ
age: 25
gender: nam
drugQuery: Panadl
```

Kỳ vọng: bot gợi ý thuốc gần đúng, không tự tạo Safety Card ngay.

### Failure / urgent

```text
condition: khó thở, sưng mặt sau khi uống thuốc
age: 30
gender: nữ
drugQuery: Ibuprofen
```

Kỳ vọng: bot dừng flow tra cứu thường và hướng user gọi 115 / đến cơ sở y tế / hỏi dược sĩ.

### Correction

```text
Input ban đầu: Ibuprofen
User sửa: Paracetamol
```

Kỳ vọng: bot bỏ kết quả cũ và tạo Safety Card mới.

---

## 8. Giới hạn prototype

- Không kê đơn.
- Không chẩn đoán bệnh.
- Không đổi liều thuốc.
- Không thay thế dược sĩ/bác sĩ.
- Không tích hợp production API Long Châu.
- DB thuốc là dữ liệu demo phục vụ prototype Day 6.