# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con chở người, gồm sedan, SUV, taxi và xe bán tải. Xe đang đỗ vẫn thuộc lớp này. | Phân biệt với `truck` bằng mục đích và kiểu thân xe; xe bán tải thuộc `car` theo schema bài học. |
| `truck` | Xe tải chở hàng hoặc xe đầu kéo. | Không gán xe bán tải thành `truck`; kiểm tra thùng hàng và kết cấu xe khi phân biệt. |
| `bus` | Xe buýt hoặc minibus chở khách. | Phân biệt với `van` dựa trên kiểu xe chở khách; trường hợp không rõ cần ghi ảnh và hỏi giảng viên. |
| `van` | Xe van hoặc minivan chở hàng hay chở người. | Không gán xe buýt lớn thành `van`, không gán xe con thông thường thành `van`. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: mỗi xe hợp lệ có một box riêng, ôm sát phần thân xe nhìn thấy, không lấy thêm mặt đường hoặc xe bên cạnh; hai xe chồng nhau vẫn có hai box.
- Vật thể bị che khuất: gán một box quanh phần nhìn thấy nếu vẫn nhận diện được loại xe; không đoán hình dạng của phần bị che.
- Vật thể bị cắt bởi rìa ảnh: cho cạnh box chạm đúng rìa ảnh, không mở rộng ra ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: chỉ gán khi đủ dấu hiệu để xác định một trong bốn lớp; nếu không chắc, ghi lại ảnh và hỏi giảng viên thay vì đoán.
- Phạm vi: không gán người, xe máy, xe đạp, biển báo, đèn giao thông, hình xe trên quảng cáo hoặc ảnh phản chiếu không phải xe thật trong cảnh.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_014` | Xe ở biểu tượng rẽ không rõ thuộc loại nào,nửa này nửa kia | Quyết định chọn đó là truck | Vì nó là đầu kéo |
| `drive_044` | Xe bị mờ | Quyết định đó là car | Theo đặc điểm nhận dạng,nó là xe ô tô sedan |
| `drive_049` | Xe dài dài ở cuối ảnh | Quyết định đó là xe bus | Vì nó có dấu hiệu không phải xe chở hàng và mang cho mình kiểu dáng xe bus/khách |
