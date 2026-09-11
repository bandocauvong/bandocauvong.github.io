# Bản đồ cầu vồng

Bản demo kỹ thuật: 4 mini-game pixel chạy thẳng trong trình duyệt điện thoại, dùng cho ý tưởng
"quét QR trên bảng là chơi được ngay". Không cần cài app, không đăng nhập, không backend.

**Chơi tại:** https://bandocauvong.github.io/

## Nội dung

| Nhiệm vụ | Cơ chế | Mục tiêu lấy tem |
|---|---|---|
| Ghép Kẹo | Match-3, 6 loại kẹo | cày tới khi hết nước đi |
| Chạy Kẹo | 3 làn, đổi làn + nhảy | vô hạn |
| Landmark 81 | Chạm để bay | vô hạn |
| Đối Kẹo | Pong 2 người / đấu bot | thắng trước 5 bàn |

Lần đầu vào web sẽ hỏi biệt danh trước khi cho chơi.

Điểm quy đổi theo hệ số riêng từng game (một ván tốt ở game nào cũng ~120 điểm), cộng vào
bảng "đua cày"; ngoài ra mỗi game có bảng top 10 riêng. Dữ liệu top 10 là mẫu, có nhãn ghi rõ.

## Cấu trúc

Toàn bộ nằm trong **một file `index.html` duy nhất** (~82KB). Không build, không dependency,
không backend. Art pixel vẽ bằng canvas ở độ phân giải 160×284 rồi phóng to nearest-neighbour.
Nền phức tạp (tia nắng, cầu vồng, bãi cỏ) render sẵn một lần ra canvas ngoài màn hình rồi blit,
nên vẫn mượt trên Android tầm thấp.

Bộ mã hoá QR cũng viết tay trong file (byte mode, mức sửa lỗi M, version 1–10), vì trang
không tải thư viện ngoài.

Phụ thuộc bên ngoài duy nhất: Google Fonts. Trước khi lên bản chạy thật nên nhúng font vào
file để game tự chứa hoàn toàn, chạy tốt cả khi mạng yếu.

## Triển khai

Link đích là `bandocauvong.github.io`. Tên miền `<tên>.github.io` lấy theo **tên chủ sở hữu**
chứ không phải tên repo, nên cần tạo một GitHub Organization miễn phí tên `bandocauvong`
(tên này còn trống), rồi tạo repo `bandocauvong.github.io` trong org đó. Pages tự publish
từ nhánh `main`, không cần cấu hình thêm.

## Đổi link mà mã QR trỏ tới

Sửa đúng một dòng trong `index.html` (tìm `var url =` trong khối QR ở cuối file).

Lưu ý cho bản in thật: **đừng để QR trỏ thẳng vào host**. Bảng in ra rồi thì không sửa được,
mà host thì có thể phải đổi. Trỏ QR vào một link ngắn thuộc domain bạn kiểm soát, rồi cho link
đó redirect sang nơi game đang nằm. Đổi host sau này chỉ cần sửa redirect, và bạn đếm được
lượt quét miễn phí ở lớp đó.

Link càng ngắn thì mã càng thưa ô, quét từ xa càng dễ — điều này quan trọng với bảng dán sau xe.

## Trạng thái

Bản demo, chưa dùng bộ nhận diện chính thức của Chupa Chups. Logo, font và tên vị kẹo cần
client cấp. Màu và element dựng theo hệ đỏ–vàng và dải sản phẩm trong pack shot.
