# Thin SPEC — Long Châu Safety Bot

> Bản Thin SPEC cuối Day 05, dùng làm cam kết sản phẩm để nhóm build prototype ở Day 06.  
> Thin SPEC không phải PRD đầy đủ; mục tiêu là chốt rõ: user nào, pain gì, build slice nào, AI quyết định gì, failure xử lý ra sao.

---

## 1. Track, product/app và user

**Track:** Healthcare / Pharmacy — app nhà thuốc thật  

**Product/app thật:** **Long Châu — Chuyên gia thuốc**  

- iOS: https://apps.apple.com/vn/app/long-châu-chuyên-gia-thuốc/id1586071844
- Android: `vn.frt.longchau.app`
- Hotline: 1800 6928

**User cụ thể:** Người đã mua, đang cầm thuốc OTC, hoặc chuẩn bị mua thuốc trên app Long Châu. User có một tình trạng sức khỏe, triệu chứng hoặc bệnh nền cụ thể, ví dụ: sốt, đau đầu, đau dạ dày, tiểu đường, mang thai, dị ứng penicillin.

User muốn biết:

```text
Thuốc/hoạt chất này có điểm gì cần lưu ý với tình trạng của mình không?
```

**Nhóm có phải user thật không?**  
Có một phần. Thành viên nhóm từng dùng app/nhà thuốc để mua thuốc cho bản thân hoặc gia đình. Tuy nhiên nhóm không phải dược sĩ, nên prototype không thay thế tư vấn chuyên môn. Prototype chỉ mô phỏng luồng tra cứu an toàn ban đầu và handoff sang dược sĩ khi cần.

---

## 2. Evidence summary

| Evidence | Nguồn | User / pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| Long Châu có chat dược sĩ, nhưng một số review nói xử lý chậm hoặc treo chat | App Store / AppRecs review | User cần một bước tra nhanh trước khi chờ dược sĩ | Thêm self-serve Safety Card trước queue dược sĩ |
| App có disclaimer: thông tin chỉ tham khảo, không thay thế chẩn đoán/tư vấn y tế | Mô tả app Long Châu | Sản phẩm phải giữ giới hạn an toàn | Mọi Safety Card phải có disclaimer + CTA hỏi dược sĩ |
| Evidence từ teardown NEO: tool/API lỗi ở bước cuối làm user mất trust | Workshop cá nhân Day 5 | Tool fail mà bot vẫn nói chung chung làm user không biết làm gì tiếp | Khi DB/API lỗi, bot không được bịa Safety Card; phải chuyển fallback/handoff |
| Long Châu có entry point chat dược sĩ và tra/chụp ảnh sản phẩm | App Long Châu / app store description | Có ngữ cảnh sản phẩm thật để gắn Safety Bot | Prototype nhận tên thuốc/hoạt chất; OCR để backlog hoặc demo phụ |
| Workshop nhóm chốt flow: tình trạng → thuốc → Safety Card | Workshop nhóm Day 5 | Task đủ hẹp, demo được trong Day 6 | Không build chatbot y tế đa năng; chỉ build Safety Card cho 1 thuốc + 1 tình trạng |

---

## 3. Pain statement

```text
User đang cầm thuốc hoặc chuẩn bị mua thuốc trên Long Châu
bị kẹt ở câu hỏi “thuốc/hoạt chất này có phù hợp với tình trạng của mình không”,
vì app chủ yếu đưa user vào chat dược sĩ hoặc user phải tự Google.

Nếu chat dược sĩ chậm, user không có bước tự kiểm tra nhanh.
Nếu tự Google hoặc hỏi AI generic, user khó biết nguồn nào đáng tin.
Điều này có thể dẫn đến tự ý dùng thuốc sai, bỏ qua chống chỉ định, hoặc lo lắng không cần thiết.

Bằng chứng chính là review về chat chậm/treo, disclaimer app chỉ mang tính tham khảo,
và quan sát nhóm rằng app chưa có Safety Card tự phục vụ trước khi handoff dược sĩ.
```

---

## 4. Opportunity statement

Người dùng không chỉ cần một chatbot trả lời về thuốc. Họ cần một **lớp kiểm tra an toàn ban đầu**:

```text
1 tình trạng sức khỏe
+ 1 thuốc/hoạt chất
→ thông tin có cấu trúc
→ mức cảnh báo rõ
→ nguồn/disclaimer
→ biết khi nào phải hỏi dược sĩ.
```

Đây là việc đáng sửa vì sai sót trong tra cứu thuốc có thể gây hậu quả cao hơn các domain thông thường. Sản phẩm phải giúp user hiểu rủi ro, nhưng không được thay bác sĩ/dược sĩ đưa quyết định cuối.

---

## 5. Build slice

```text
Cho người dùng app Long Châu đang có một tình trạng/triệu chứng đã khai báo
và muốn tra một tên thuốc hoặc hoạt chất,
prototype dùng DB demo + AI để tra cứu thông tin thuốc,
đối chiếu thuốc với tình trạng user,
tạo Safety Card gồm hoạt chất, chỉ định, chống chỉ định, lưu ý, mức cảnh báo và nguồn,
đồng thời xử lý các failure mode như không tìm thấy thuốc, tình trạng mơ hồ, tên thuốc mơ hồ hoặc triệu chứng khẩn cấp
bằng hỏi lại, gợi ý lựa chọn, hoặc chuyển sang dược sĩ/cơ sở y tế.
```

### Một user, một task, một AI decision, một output

| Thành phần | Nội dung |
|---|---|
| **Một user** | Người đang cầm hoặc chuẩn bị mua một thuốc/hoạt chất trên Long Châu |
| **Một task** | Kiểm tra thuốc đó có điểm gì cần lưu ý với tình trạng của mình |
| **Một AI decision** | Phân loại mức cảnh báo Xanh / Vàng / Đỏ / Không chắc |
| **Một output** | Safety Card có nguồn, disclaimer và CTA hỏi dược sĩ |

---

## 6. Flow prototype Day 06

```text
1. User nhập tình trạng sức khỏe
2. User nhập tuổi và giới tính nếu cần để đối chiếu rule
3. User nhập tên thuốc hoặc hoạt chất
4. Bot kiểm tra triệu chứng khẩn cấp trước
5. Bot tra DB demo / fuzzy matching / API fallback nếu có
6. Bot trả Safety Card
7. Bot đối chiếu thuốc với tình trạng → gắn cảnh báo Xanh / Vàng / Đỏ
8. Nếu Vàng/Đỏ/Không chắc → khuyến nghị hỏi dược sĩ/bác sĩ
9. Nếu khẩn cấp → không tiếp tục flow tra cứu thường, hướng gọi 115 hoặc đến cơ sở y tế
```

---

## 7. Auto/Aug decision

Chọn:

```text
Conditional automation
```

AI được tự động làm trong phạm vi hẹp:

```text
- Tra cứu thuốc/hoạt chất trong DB demo
- Chuẩn hóa hoặc fuzzy-match tên thuốc
- Draft Safety Card
- Đối chiếu rule cơ bản với tình trạng user
- Gắn mức cảnh báo
```

AI không được làm:

```text
- Chẩn đoán bệnh
- Kê đơn
- Đổi liều
- Nói chắc “được uống”
- Tự thay thế dược sĩ/bác sĩ
```

**Human role:**

| Tình huống | Ai giữ quyền quyết định |
|---|---|
| Cờ Xanh | User đọc disclaimer và tự quyết định bước tiếp theo |
| Cờ Vàng | User nên hỏi dược sĩ trước khi dùng |
| Cờ Đỏ | Dược sĩ/bác sĩ/cơ sở y tế xử lý |
| Không chắc | Bot hỏi lại hoặc chuyển dược sĩ |
| Khẩn cấp | Bot dừng flow thường, hướng gọi 115 / cơ sở y tế |

Lý do chọn conditional automation: tra cứu và draft Safety Card có thể tự động trong scope hẹp, nhưng quyết định y tế cuối không thể tự động hóa vì hậu quả khi sai có thể nghiêm trọng và khó hoàn tác.

---

## 8. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| **Happy path** | User nhập tình trạng rõ + thuốc có trong DB, ví dụ `sốt nhẹ + Paracetamol` → bot trả Safety Card đầy đủ, có nguồn, disclaimer và mức cảnh báo phù hợp. |
| **Low-confidence path** | Tên thuốc mơ hồ hoặc gõ sai, ví dụ `Panadl` hoặc `Panadol` → bot không đoán bừa, show 2–3 lựa chọn hoặc hỏi user xác nhận. Tình trạng mơ hồ như `đau` → bot hỏi thêm. |
| **Failure path** | Thuốc không có trong DB, DB/API lỗi hoặc triệu chứng khẩn cấp → bot không bịa Safety Card. Với triệu chứng khẩn như khó thở, đau ngực, phản vệ, bot dừng flow thường và hướng gọi 115 / đến cơ sở y tế / hỏi dược sĩ ngay. |
| **Correction path** | User sửa thuốc hoặc tình trạng, ví dụ “không phải Ibuprofen mà Paracetamol” → bot bỏ Safety Card cũ, tạo lại Safety Card mới và log correction trong session demo. |

---

## 9. Failure mode nguy hiểm nhất

```text
Failure mode nguy hiểm nhất là bot nhận nhầm thuốc hoặc đánh giá cảnh báo quá thấp
khi user nhập tên thuốc/tình trạng mơ hồ.

Nếu xảy ra, user có thể tự ý dùng thuốc có chống chỉ định,
bỏ qua dấu hiệu nguy hiểm, hoặc không hỏi dược sĩ khi cần.
```

Prototype xử lý bằng:

```text
- Bắt buộc xác nhận khi tên thuốc mơ hồ
- Fuzzy suggestion thay vì tự chọn im lặng
- Không bịa Safety Card khi DB/API lỗi
- Luôn có nguồn + disclaimer
- Cờ Vàng/Đỏ luôn kèm CTA hỏi dược sĩ
- Urgent symptoms dừng flow tra cứu thường và chuyển cấp cứu/cơ sở y tế
- Không dùng câu tuyệt đối như “được uống” hoặc “an toàn tuyệt đối”
```

---

## 10. Owner plan cho Day 06

| Thành viên | Vai trò Day 06 | Bằng chứng / output cần có |
|---|---|---|
| **Phùng Văn Thạch** | Prototype owner chính, safety bot flow, logic chatbot, API/client, demo kỹ thuật | Code prototype, flow chính, demo chạy được |
| **Lương Quốc Đoàn** | UX/UI owner, logo/assets, visual polish, giao diện Long Châu-style | UI prototype, assets/logo, screenshot giao diện |
| **Trịnh Vũ Anh Tuấn** | Rule/DB owner, dữ liệu thuốc demo, condition/gender rules | `data/drugs-demo.json`, rule Xanh/Vàng/Đỏ |
| **Hoàng Phương Thảo** | Workflow/documentation support, kiểm tra luồng trải nghiệm, submission hygiene | Workflow note, review tài liệu, `.gitignore`/README support |
| **Thái Thị Yến Nhi** | Repo/submission owner, SPEC, failure/correction path, gom repo nộp | Repo nộp, README, SPEC, test/failure notes |

---

## 11. Safety Card mẫu

| Trường | Nội dung mẫu |
|---|---|
| Thuốc / hoạt chất | Paracetamol |
| Tình trạng user khai báo | Sốt nhẹ, 25 tuổi, nam |
| Chỉ định tóm tắt | Hạ sốt, giảm đau |
| Chống chỉ định / lưu ý | Dị ứng paracetamol, bệnh gan nặng, tránh quá liều |
| Đối chiếu với tình trạng | Xanh hoặc Vàng tùy rule demo |
| Nguồn | DB demo / nguồn tham khảo đã khai báo |
| Disclaimer | Không thay thế tư vấn dược sĩ/bác sĩ |
| CTA | Hỏi dược sĩ Long Châu |

---

## 12. Câu chốt Thin SPEC

```text
Nhóm không build AI bác sĩ.
Nhóm build một lát cắt hẹp: user có một tình trạng sức khỏe và một thuốc/hoạt chất cần tra,
bot tạo Safety Card có nguồn, gắn mức cảnh báo, và biết dừng hoặc chuyển dược sĩ khi không chắc.
```

---

*Thin SPEC — Batch 02 · Long Châu Safety Bot · Day 05*