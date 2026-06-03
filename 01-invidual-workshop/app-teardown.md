# App Teardown: Vietnam Airlines — NEO Chatbot

## 1. Product được chọn

**Product:** Vietnam Airlines — NEO  
**AI feature:** Chatbot/trợ lý ảo hỗ trợ khách hàng về vé máy bay, chuyến bay, hành lý, mua vé, thanh toán, hoàn/đổi vé.  
**Kênh truy cập:** Website Vietnam Airlines, app, Facebook, Zalo OA.

---

## 2. Promise vs Reality

### Product hứa gì?

NEO hứa giúp hành khách tra cứu nhanh các thông tin liên quan đến:

- Vé máy bay, chuyến bay, hành lý.
- Tìm kiếm giá vé.
- Kiểm tra hành trình, tình trạng đặt chỗ.
- Thông tin hoàn/đổi vé.
- Hành lý miễn cước, hành lý xách tay, hành lý tính cước, hành lý đặc biệt.
- Khi bot không giải đáp được, hệ thống có thể chuyển hướng gặp tư vấn viên.

### User nào được hứa sẽ được giúp?

User chính là **hành khách Vietnam Airlines**, gồm:

- Người chưa mua vé, muốn hỏi lịch bay, giá vé, điều kiện vé.
- Người đã có vé, muốn kiểm tra hành trình, hành lý, đổi vé, hoàn vé.
- Người gặp vấn đề trước chuyến bay và cần phản hồi nhanh 24/7.

### Kỳ vọng AI làm được task nào?

Task kỳ vọng:

> “Tôi bay Hà Nội – TP.HCM ngày mai, vé Economy Lite, muốn mua thêm 1 kiện hành lý 23kg và đổi sang chuyến muộn hơn. Phí khoảng bao nhiêu và tôi cần làm bước nào?”

Kỳ vọng hợp lý là NEO phải:

1. Nhận ra đây là intent phức hợp: **hành lý + đổi vé + điều kiện vé cụ thể**.
2. Hỏi thêm thông tin nếu thiếu: mã đặt chỗ, họ tên, chặng bay, hạng vé.
3. Nếu không đủ tự tin, chuyển sang flow có nút:
   - Tra cứu đặt chỗ
   - Mua hành lý
   - Đổi chuyến
   - Gặp tư vấn viên
4. Không trả lời chung chung kiểu “vui lòng tham khảo chính sách hành lý”.

### Reality / Điểm gãy quan sát được

NEO mạnh ở các câu hỏi đơn giản như:

> “Hành lý xách tay được bao nhiêu kg?”  
> “Làm thủ tục online như thế nào?”

Tuy nhiên, khi câu hỏi có nhiều điều kiện cá nhân hóa như **hạng vé + đổi chuyến + mua thêm hành lý + thời điểm bay**, bot dễ trả lời theo hướng chính sách chung, chưa biến câu hỏi thành một workflow rõ ràng.

Điểm gãy chính:

> Khi user hỏi một câu có nhiều intent và cần tính theo booking cụ thể, NEO chưa dẫn user qua các bước xác nhận thông tin mà có xu hướng đưa thông tin chung hoặc menu tổng quát. Hậu quả là user vẫn không biết chính xác phải bấm vào đâu, cần chuẩn bị gì, và phí/điều kiện áp dụng cho vé của mình ra sao.

---

## 3. Evidence

### Quote / source từ product

Vietnam Airlines giới thiệu NEO là trợ lý ảo có thể hỗ trợ tra cứu thông tin vé, chuyến bay, hành lý, tìm giá vé, điều kiện hoàn/đổi vé và hướng dẫn giấy tờ trước chuyến bay.

**Nguồn tham khảo:**

- https://www.vietnamairlines.com/de/vi/support/chatbot
- https://www.vietnamairlines.com/us/en/support/condition-of-chatbot-NEO

### Prompt/input đã thử

```text
Tôi bay Hà Nội – TP.HCM ngày mai, vé Economy Lite, muốn mua thêm 1 kiện hành lý 23kg và đổi sang chuyến muộn hơn. Phí khoảng bao nhiêu và tôi cần làm bước nào?
```

### Observation cụ thể

- Với câu hỏi đơn giản, bot có thể trả lời hoặc đưa user đến nhóm thông tin phù hợp.
- Với câu hỏi phức hợp liên quan đến **đổi vé + hành lý + hạng vé**, bot chưa thể hiện rõ flow xác nhận thông tin.
- Bot có xu hướng đưa thông tin chung, trong khi user cần hướng dẫn theo trường hợp cụ thể.
- User vẫn phải tự tìm tiếp trên website, vào quản lý đặt chỗ, hoặc liên hệ tổng đài/tư vấn viên.

### Screenshot nên chụp để nộp kèm

- **Screenshot 1:** Trang giới thiệu NEO trên website Vietnam Airlines.
- **Screenshot 2:** Khung chat sau khi nhập prompt phức hợp ở trên.
- **Screenshot 3:** Phần bot trả lời chung chung hoặc chưa đưa được flow xử lý rõ ràng.
- **Screenshot 4:** Nếu có, chụp màn hình bot chuyển tư vấn viên hoặc menu hỗ trợ.

---

## 4. Bốn paths

| Path                | Quan sát với NEO                                                                                                                                                 | Product issue                                     |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| Happy path          | User hỏi câu đơn giản như “Hành lý xách tay được bao nhiêu kg?”. Bot trả lời được chính sách hành lý hoặc đưa link/mục liên quan.                                | Hoạt động tốt với intent đơn, thông tin dạng FAQ. |
| Low-confidence path | User hỏi câu phức hợp: đổi vé + mua hành lý + hạng vé cụ thể. Bot nên hỏi lại mã đặt chỗ/hạng vé/chặng bay hoặc đưa lựa chọn, nhưng flow này chưa nổi bật.       | Thiếu cơ chế làm rõ intent khi confidence thấp.   |
| Failure path        | Bot trả lời chính sách chung, user vẫn không biết phí cụ thể và bước tiếp theo.                                                                                  | Lỗi UX recovery + data/tool integration.          |
| Correction path     | User sửa lại: “Tôi muốn đổi vé chứ không hỏi chính sách hành lý.” Bot có thể trả lời lại, nhưng correction chưa thể hiện rõ là được lưu/log để cải thiện intent. | Correction chưa đóng thành feedback loop rõ ràng. |

---

## 5. Finding viết thành product decision

### Finding chính

Khi user hỏi một yêu cầu phức hợp như **“vừa đổi chuyến bay, vừa mua thêm hành lý, áp dụng cho một hạng vé cụ thể”**, NEO có xu hướng xử lý như câu hỏi FAQ chung thay vì nhận ra đây là workflow cần xác thực thông tin đặt chỗ.

Hậu quả là user không biết:

- Phí thực tế là bao nhiêu.
- Vé của mình có đổi được không.
- Điều kiện nào áp dụng cho hạng vé của mình.
- Bước tiếp theo cần làm là gì.

Với ngành hàng không, đây là vấn đề quan trọng vì thông tin sai hoặc thiếu có thể khiến user lỡ thời gian đổi vé, mua hành lý muộn, hoặc phải gọi tổng đài lại từ đầu.

### Layer lỗi

Lỗi thuộc các layer sau:

- **Intent:** Chưa tách tốt multi-intent.
- **Data-tool:** Chưa kiểm tra được booking/fare condition trong cùng flow.
- **UX recovery:** Khi không chắc, chưa hỏi lại hoặc đưa lựa chọn đủ rõ.
- **Safety/trust:** Các câu hỏi về phí, đổi vé, hoàn vé cần độ chính xác cao hơn FAQ thông thường.

### Product decision đề xuất

NEO nên có **low-confidence recovery flow** cho các câu hỏi liên quan đến tiền, đổi vé, hoàn vé, hành lý tính cước hoặc thông tin booking cá nhân.

Flow đề xuất:

```text
Nếu NEO phát hiện câu hỏi có nhiều intent hoặc cần dữ liệu booking:

1. Không trả lời ngay bằng chính sách chung.
2. Hỏi lại user muốn làm việc nào trước:
   - Kiểm tra điều kiện đổi vé
   - Mua thêm hành lý
   - Tra cứu booking
   - Gặp tư vấn viên
3. Nếu cần dữ liệu cá nhân, chuyển sang form/flow bảo mật thay vì yêu cầu nhập thông tin nhạy cảm trực tiếp trong chat.
4. Nếu confidence thấp dưới ngưỡng, chuyển tư vấn viên và gửi kèm tóm tắt intent của user.
```

---

## 6. Sketch As-is / To-be

### As-is

```text
User
 |
 | Nhập câu hỏi phức hợp:
 | "Tôi muốn chuyến bay giá rẻ nhất đi HCM ngày 17/1"
 v
NEO nhận input
 |
 | Cố map vào FAQ gần nhất
 v
Hỏi đi từ đâu đến HCM
 |
 v
User vẫn chưa biết:
- Vé đi vào những khung giờ nào
- Vé của mình có đổi được không
- Bấm vào đâu để xử lý
 |
 v
Điểm gãy:
User phải tự tìm tiếp / gọi tổng đài / hỏi lại nhiều lần
```

### To-be

```text
User
 |
 | Nhập câu hỏi phức hợp:
 | "Tôi muốn tìm vé rẻ nhất từ HN đi HCM từ ngày hôm nay đến ngày 17/01/2027"
 v
NEO phát hiện multi-intent + cần booking data
 |
 v
NEO hỏi lại:
"Bạn muốn xử lý bước nào trước?"
 |
 |--- [1] Hỏi rõ đi ngày nào
 |--- [2] Đi mấy người
 |--- [3] Người lớn hay người già hay trẻ nhỏ
 |--- [4] Hỏi lại
 |
 v
User chọn "Đi ngày 17/1"
 |
 v
NEO chuyển sang form bảo mật / quản lý đặt chỗ
 |
 v
Nếu đủ dữ liệu:
- Hiển thị điều kiện vé
- Hiển thị bước tiếp theo
- Gợi ý mua hành lý
 |
 v
Nếu không đủ dữ liệu hoặc confidence thấp:
- Chuyển tư vấn viên
```

---

## 7. SPEC change

Finding này sẽ đổi SPEC của NEO ở phần **Intent Handling + Recovery Flow**.

### SPEC change đề xuất

> Với các intent có rủi ro cao như hoàn vé, đổi vé, mua thêm hành lý, phí phát sinh hoặc booking cá nhân, NEO không được chỉ trả lời bằng FAQ chung. Bot phải kích hoạt flow xác nhận thông tin, hỏi lại intent chính, đưa CTA rõ ràng, hoặc chuyển tư vấn viên khi confidence thấp.

### Test case cần thêm vào SPEC

```text
Input:
"Tôi muốn đi từ HN-HCM ngày 17/1/2027 với giá vé rẻ nhất bao gồm 1 người lớn và không có hành lý ký gửi"

Expected:
NEO không trả lời chung chung.
NEO phải hỏi user muốn:
1. Xác nhận thông tin chính xác
2. Tra cứu booking
3. Gặp tư vấn viên

Nếu cần thông tin cá nhân, NEO chuyển sang flow quản lý đặt chỗ/form bảo mật.
```

---

## 8. Self-check trước khi nộp

- [x] Có ít nhất 1 screenshot hoặc observation cụ thể.
- [x] Có đủ 4 paths hoặc nói rõ path nào chưa có trong product.
- [x] Finding được viết thành product decision, không chỉ là nhận xét.
- [x] Sketch có as-is và to-be.
- [x] Có một câu nói rõ finding này sẽ đổi gì trong SPEC.

---

## 9. Kết luận ngắn

NEO không phải là một chatbot “sai hoàn toàn”; bot vẫn hữu ích với các câu hỏi FAQ đơn giản. Điểm cần cải thiện nằm ở các tình huống có **multi-intent, thông tin cá nhân hóa và rủi ro cao**. Nếu bổ sung flow hỏi lại, xác nhận intent và chuyển tư vấn viên khi confidence thấp, NEO có thể giảm đáng kể cảm giác “bot trả lời chung chung” và giúp user hoàn thành task nhanh hơn.
