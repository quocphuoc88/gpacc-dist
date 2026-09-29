# gpacc-dist

Kênh phát hành của phần mềm kế toán **GPAcc TT99**.

Repo này chỉ chứa dữ liệu phát hành, không chứa mã nguồn. File GPAcc trên máy
khách đọc `manifest.txt` để biết có bản mới hay không và tải về từ đây.

## Cấu trúc

```
manifest.txt        khai báo phiên bản + địa chỉ + SHA256 của mọi thứ
MauBieu.xlsx        gói mẫu biểu (sheet MAP + các sheet tờ khai mới)
bin/
  vbaProject.bin    mã VBA đã biên dịch
  customUI.xml      định nghĩa Ribbon
mau-bieu/
  01_GTGT_TT80.xml  template XML để import vào HTKK
  ...
```

## Phát hành một bản mới

```powershell
# 1. chép vbaProject.bin mới vào bin\
# 2. chạy:
..\tools\PhatHanh.ps1 -Repo . -Ver 260901 -Push
```

Script tự kiểm tra định dạng `vbaProject.bin`, tính lại SHA256 và ghi vào
`manifest.txt`. Thêm `-DoiUI` nếu có sửa Ribbon, `-TangSchema` nếu `MauBieu.xlsx`
có thêm sheet mới.

## Thêm một mẫu biểu theo thông tư mới

1. Thêm sheet mẫu (đặt tên mới, ví dụ `GTGT_89`) vào `MauBieu.xlsx`
2. Thêm các dòng chỉ tiêu tương ứng vào sheet `MAP` trong `MauBieu.xlsx`
3. Bỏ dấu `#` ở dòng `MAU|...|TT89|...` trong `manifest.txt`
4. Chép template XML vào `mau-bieu/`
5. `PhatHanh.ps1 -TangSchema`

Không phải sửa một dòng code nào.

## Lấy `vbaProject.bin` ra từ file xlsm

```powershell
Copy-Item "GPAcc TT99.xlsm" tmp.zip
Expand-Archive tmp.zip -DestinationPath tmp -Force
Copy-Item tmp\xl\vbaProject.bin bin\vbaProject.bin -Force
Remove-Item tmp, tmp.zip -Recurse -Force
```
