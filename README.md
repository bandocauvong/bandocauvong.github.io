# Bản đồ cầu vồng

4 mini-game pixel chạy thẳng trong trình duyệt điện thoại, cho ý tưởng "quét QR trên bảng là
chơi được ngay". Không cần cài app, không backend, không build.

**Chơi tại:** https://bandocauvong.github.io/

## Nội dung

| Trò | Cơ chế | Mốc hoàn thành |
|---|---|---|
| Ghép Kẹo | Match-3, 6 loại kẹo, có kẹo phép (nổ hàng/cột/bom/cầu vồng) | cày tới khi hết nước đi |
| Chạy Kẹo | 3 làn, đổi làn + vuốt lên để nhảy | 550 mét |
| Landmark 81 | Chạm để bay, lách qua khe giữa hai cột | 8 tầng |
| Đối Kẹo | Pong 2 người chung máy / đấu bot | thắng trước 5 bàn |

Lần đầu vào web phải qua ba bước trước khi chơi: **số điện thoại → mã xác thực →
biệt danh**.

> ⚠️ **Phần đăng nhập hiện là bản demo, không phải xác thực thật.** Trang không có
> backend nên mã xác thực sinh ngay tại máy và hiện lên màn hình — không gửi SMS, và
> ai cũng nhập được số của người khác. Muốn xác thực thật thì phải có server phát
> và kiểm mã cùng một cổng SMS.

Điểm quy đổi theo hệ số riêng từng trò (một ván tốt ở trò nào cũng ~120 điểm), cộng vào bảng
"đua cày"; ngoài ra mỗi trò có bảng top 10 riêng. **Dữ liệu top 10 là mẫu**, có nhãn ghi rõ
trên giao diện.

Ngoài bốn trò còn có:

- **Ảnh khoe thành tích** 1920×1080 dựng ngay trong trang, xem được toàn màn hình và tải về
- **Bấm giờ hành trình** — bấm giờ chuyến đi, chọn điểm đi/đến để ra số ki-lô-mét, tách hẳn
  khỏi hệ điểm nên không ai khai khống để leo bảng được
- **Mã QR** tự sinh trong trang, kèm nút sao chép link
- **Nhiệm vụ chụp ảnh → nhân đôi điểm** — gửi một tấm ảnh thì **mọi ván chơi sau đó**
  được nhân hai; ván chơi trước giữ điểm gốc. Nhiệm vụ mở từ một thẻ thông báo
  dựng theo lối Zalo — **thẻ mô phỏng trong trang, chưa nối Zalo OA thật**

## Cấu trúc

Toàn bộ nằm trong **một file `index.html` duy nhất** (~246 KB, nén qua mạng còn ~83 KB).
Không build, không dependency, không backend.

- Art pixel vẽ bằng canvas ở độ phân giải **160×284** rồi phóng to nearest-neighbour. Chỉ
  phóng theo **bội số nguyên** — co theo tỉ lệ lẻ sẽ làm rơi điểm ảnh không đều và art bị mờ.
- Nền phức tạp (thành phố kẹo, sân đối kẹo, quầy ghép kẹo) render sẵn **một lần** ra canvas
  ngoài màn hình rồi blit mỗi khung hình, nên vẫn mượt trên Android tầm thấp.
- Sprite nhân vật nhúng thẳng vào file dạng data URI, không đọc file rời.
- **37 hiệu ứng âm thanh tổng hợp bằng WebAudio** — không tải một byte nào. Nhạc nền là file
  rời duy nhất, đặt `preload="none"` nên không làm chậm lúc mở trang.
- Bộ **mã hoá QR viết tay** ngay trong file (byte mode, mức sửa lỗi M, version 1–10,
  Reed–Solomon trên GF(256)), vì trang không tải thư viện ngoài.
- **Không có backend nên mọi dữ liệu nằm trong `localStorage` của máy người chơi**:
  số điện thoại, biệt danh, điểm, ảnh nhiệm vụ, hệ số nhân. Đổi máy hoặc xoá dữ liệu
  trình duyệt là mất hết. Ảnh nhiệm vụ thu về tối đa 640px trước khi lưu — ảnh gốc từ
  điện thoại nặng vài megabyte sẽ vượt hạn mức `localStorage` và làm mất sạch điểm.

Trang **khoá vào đúng một màn hình và không bao giờ cuộn**: trên iPhone, thao tác lướt làm
hiện thanh địa chỉ Safari đè lên game.

Phụ thuộc bên ngoài duy nhất: **Google Fonts**. Trước khi chạy chiến dịch thật nên nhúng font
vào file để game tự chứa hoàn toàn, chạy đúng cả khi mạng yếu.

## Tệp trong repo

| Tệp | Vai trò |
|---|---|
| `index.html` | toàn bộ game |
| `music.mp3` | nhạc nền (~2,8 MB, tải lười sau cú chạm đầu tiên) |
| `manifest.webmanifest` + `icon-*.png` | chế độ ứng dụng (PWA), khoá dọc |

## Triển khai

Đang chạy trên GitHub Pages từ nhánh `main`, không cần cấu hình thêm — đẩy lên là tự publish.

Tên miền `<tên>.github.io` lấy theo **tên chủ sở hữu** chứ không phải tên repo, nên repo này
nằm trong một GitHub Organization tên `bandocauvong`.

> **Pages cache 10 phút** (`max-age=600`). Sau khi đẩy lên, kiểm tra bằng tham số phá cache
> `?v=...`, nếu không sẽ thấy bản cũ và tưởng là lỗi.

## Đổi link mà mã QR trỏ tới

Sửa đúng một dòng trong `index.html` — tìm `var url =` trong khối QR ở cuối file.

**Lưu ý cho bản in thật: đừng để QR trỏ thẳng vào host.** Bảng in ra rồi thì không sửa được,
mà host thì có thể phải đổi. Trỏ QR vào một link ngắn thuộc domain bạn kiểm soát rồi cho
redirect sang nơi game đang nằm. Đổi host sau này chỉ cần sửa redirect, và bạn đếm được lượt
quét miễn phí ở lớp đó.

Link càng ngắn thì mã càng thưa ô, quét từ xa càng dễ — điều này quan trọng với bảng dán sau
xe đang chạy.

## Trạng thái

Chạy được đầy đủ. Ba việc còn lại đều chờ phía thương hiệu, không phải chờ code:

1. **Bộ nhận diện chính thức** — logo, font và tên vị kẹo. Màu và element hiện dựng theo
   hệ đỏ–vàng và dải sản phẩm trong pack shot.
2. **Duyệt nội dung** — danh sách địa điểm và hệ số đường bộ của bấm giờ hành trình,
   và chọn bản nhạc nền.
3. **Quyết định có dựng backend hay không.** Phần đăng nhập, ảnh nhiệm vụ và hệ số nhân
   đang là **bản demo chạy hoàn toàn trên máy người chơi**. Muốn chạy chiến dịch thật
   có giải thưởng thì cần server: xác thực thật, bảng xếp hạng chung, và **điểm phải
   tính ở server** — hiện điểm tính ở máy người chơi nên sửa được.
