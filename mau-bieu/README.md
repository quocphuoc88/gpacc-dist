# Template XML để import vào HTKK

Chương trình tải file trong thư mục này về, chép số liệu vào rồi lưu ra cho
người dùng import vào HTKK. Đây chỉ là **khuôn rỗng** — mọi giá trị đều là 0.

## Hai file hiện có đã CŨ

| File | Sinh bởi HTKK | Mẫu ban hành theo | pbanTKhaiXML |
|---|---|---|---|
| `01_GTGT_TT80.xml` | 4.3.8 | Thông tư 26/2015/TT-BTC | 2.1.2 |
| `03_TNDN_TT80.xml` | 4.3.8 | Thông tư 151/2014/TT-BTC | 2.3.1 |

Sheet tờ khai trong file GPAcc đã theo mẫu **mới hơn** hai khuôn này. Hệ quả:

- `01/GTGT` — khuôn không có thẻ `ct23a`, `ct24a` (hàng hoá dịch vụ nhập khẩu).
  Hai chỉ tiêu đó có trên sheet nhưng **không xuất ra được**.
- `03/TNDN` — khuôn thiếu 21 thẻ so với sheet: `ctA`, `ctB`, `ctB15`, `ctC`,
  `ctC17`, `ctC8a`, `ctD4`–`ctD8`, `ctE4`–`ctE6`, `ctG4`, `ctG5`,
  `ctH1`–`ctH3`, `ctI1`, `ctI2`.
- `03-1A` — khuôn có `ct01`–`ct19`, sheet có chỉ tiêu `[04]`–`[22]`.
  Cùng 19 chỉ tiêu nhưng **đánh số lệch nhau 3**. Xem mục dưới.

Thẻ không có trong khuôn thì bị bỏ qua lặng lẽ, không báo lỗi.

## Cách lấy khuôn mới

1. Mở HTKK bản mới nhất
2. Tạo tờ khai tương ứng, **để trống hết**, chỉ điền mã số thuế và kỳ
3. Kết xuất ra XML
4. Chép file đó vào đây, ghi đè
5. Chạy `ModMap.MapDoiChieuXML "01/GTGT", "TT80", "<đường dẫn file>"` trong GPAcc
   để xem còn thẻ nào trong bảng MAP mà khuôn không có

## Việc cần xác minh cho 03-1A

Sheet `PL_03TNDN` đánh số `[04]`–`[22]`, khuôn đánh số `ct01`–`ct19`.
Cùng 19 chỉ tiêu, nghi là cùng nội dung nhưng lệch 3 số.

Kiểm chứng trong 2 phút:

1. Mở HTKK, tạo tờ khai 03/TNDN
2. Ở phụ lục 03-1A, điền chỉ tiêu "Doanh thu bán hàng và cung cấp dịch vụ"
   một số dễ nhận, ví dụ `111`
3. Kết xuất XML, mở bằng Notepad, tìm `111` nằm trong thẻ nào

- Nếu nằm trong `ct01` → bảng MAP đang sai, phải dời toàn bộ TagXML của
  03-1A xuống 3 số (`[04]`→`ct01`, `[05]`→`ct02`, …, `[22]`→`ct19`)
- Nếu nằm trong `ct04` → bảng MAP đang đúng, chỉ thiếu khuôn mới cho
  `ct20`–`ct22`
