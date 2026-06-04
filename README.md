"""# REPORT: Chatbot AI Tra Cứu Thuốc

## 1. Thông tin chung

| Hạng mục            | Nội dung                                                               |
| ------------------- | ---------------------------------------------------------------------- |
| Tên project         | Chatbot AI Tra Cứu Thuốc                                               |
| Loại tính năng      | AI Chatbot / Drug Lookup Assistant                                     |
| Mục tiêu chính      | Hỗ trợ người dùng tra cứu thông tin thuốc nhanh, dễ hiểu và an toàn    |
| Nền tảng triển khai | Web app / Mobile app / App nhà thuốc                                   |
| Đối tượng sử dụng   | Người dùng phổ thông, khách hàng nhà thuốc, nhân viên hỗ trợ, dược sĩ  |
| Phạm vi MVP         | Tra cứu thông tin thuốc, không kê đơn, không chẩn đoán, không đổi liều |

---

## 2. Bối cảnh và vấn đề

Người dùng thường có nhu cầu tra cứu nhanh thông tin về thuốc trước hoặc sau khi mua thuốc. Tuy nhiên, thông tin thuốc trên Internet thường phân tán, khó kiểm chứng và có thể gây hiểu nhầm nếu người dùng tự suy diễn.

Một số vấn đề phổ biến:

- Người dùng không biết thuốc dùng để làm gì.
- Người dùng không hiểu hoạt chất trong thuốc.
- Người dùng muốn biết cách dùng thuốc ở mức tham khảo.
- Người dùng cần kiểm tra tác dụng phụ, chống chỉ định hoặc tương tác thuốc.
- Người dùng có thể nhầm lẫn giữa tên thương mại và hoạt chất.
- Người dùng có thể hỏi những câu vượt quá phạm vi an toàn như kê đơn, đổi liều hoặc chẩn đoán bệnh.

Vì vậy, cần xây dựng một chatbot AI có khả năng tra cứu thuốc từ nguồn dữ liệu đã kiểm duyệt, trả lời dễ hiểu và có cơ chế cảnh báo an toàn.

---

## 3. Mục tiêu project

### 3.1. Mục tiêu sản phẩm

Chatbot AI Tra Cứu Thuốc giúp người dùng:

- Tra cứu nhanh thông tin thuốc bằng tên thuốc hoặc hoạt chất.
- Hiểu công dụng, cách dùng tham khảo, tác dụng phụ, chống chỉ định và tương tác thuốc.
- Nhận được câu trả lời có cấu trúc, dễ đọc.
- Được cảnh báo khi câu hỏi vượt quá phạm vi tra cứu thông tin.
- Được khuyến nghị gặp bác sĩ hoặc dược sĩ trong các trường hợp rủi ro cao.

### 3.2. Mục tiêu AI

AI trong project có nhiệm vụ:

- Hiểu câu hỏi tự nhiên của người dùng.
- Phân loại intent tra cứu thuốc.
- Trích xuất tên thuốc, hoạt chất, hàm lượng hoặc dạng dùng nếu có.
- Chuẩn hóa từ khóa tra cứu.
- Truy xuất thông tin từ cơ sở dữ liệu thuốc.
- Tổng hợp câu trả lời ngắn gọn, đúng phạm vi và an toàn.

---

## 4. Phạm vi project

## 4.1. In scope

Project MVP bao gồm:

- Người dùng nhập tên thuốc hoặc câu hỏi về thuốc.
- Chatbot phân loại mục tiêu tra cứu.
- Chatbot hỏi thêm thông tin nếu thiếu dữ liệu.
- Chatbot tra cứu cơ sở dữ liệu thuốc.
- Chatbot trả lời theo format chuẩn.
- Chatbot hiển thị cảnh báo an toàn.
- Chatbot từ chối các yêu cầu kê đơn, chẩn đoán hoặc đổi liều.

## 4.2. Out of scope

Project MVP không bao gồm:

- Không kê đơn thuốc.
- Không chẩn đoán bệnh.
- Không đề xuất phác đồ điều trị cá nhân hóa.
- Không thay thế bác sĩ hoặc dược sĩ.
- Không xử lý cấp cứu y tế.
- Không tự động bán thuốc hoặc quyết định thuốc thay người dùng.
- Không đưa ra liều dùng cá nhân hóa dựa trên bệnh nền nếu chưa có chuyên gia kiểm duyệt.

---

## 5. Người dùng mục tiêu

### 5.1. Primary users

| Nhóm người dùng             | Nhu cầu                                           |
| --------------------------- | ------------------------------------------------- |
| Khách hàng nhà thuốc        | Muốn tra cứu nhanh thông tin thuốc                |
| Người chăm sóc người thân   | Muốn hiểu thuốc người thân đang dùng              |
| Người dùng phổ thông        | Muốn biết công dụng, cách dùng, tác dụng phụ      |
| Người đang dùng nhiều thuốc | Muốn kiểm tra thông tin tương tác ở mức tham khảo |

### 5.2. Secondary users

| Nhóm người dùng | Nhu cầu                                      |
| --------------- | -------------------------------------------- |
| Dược sĩ         | Có công cụ hỗ trợ tra cứu nhanh              |
| Nhân viên CSKH  | Trả lời câu hỏi cơ bản của khách hàng        |
| Admin hệ thống  | Quản lý nguồn dữ liệu và kiểm duyệt nội dung |

---

## 6. User Journey

### 6.1. Journey chính

1. Người dùng mở chatbot.
2. Người dùng nhập tên thuốc hoặc câu hỏi về thuốc.
3. Chatbot xác định người dùng muốn tra cứu nội dung gì.
4. Chatbot kiểm tra câu hỏi có đủ thông tin hay chưa.
5. Nếu thiếu thông tin, chatbot hỏi bổ sung.
6. Nếu đủ thông tin, chatbot chuẩn hóa từ khóa.
7. Chatbot tra cứu dữ liệu thuốc.
8. Chatbot kiểm tra kết quả có phù hợp hay không.
9. Chatbot tổng hợp câu trả lời.
10. Chatbot hiển thị kết quả kèm lưu ý an toàn.
11. Nếu câu hỏi vượt phạm vi, chatbot khuyến nghị gặp bác sĩ hoặc dược sĩ.

### 6.2. Ví dụ hành trình người dùng

**Tình huống:** Người dùng muốn biết thuốc Paracetamol dùng để làm gì.

```text
User: Paracetamol dùng để làm gì?

Chatbot:
- Nhận diện thuốc: Paracetamol
- Intent: Tra cứu công dụng
- Tra cứu CSDL thuốc
- Trả lời: Paracetamol thường được dùng để giảm đau và hạ sốt...
- Hiển thị lưu ý: Thông tin chỉ mang tính tham khảo...
```
