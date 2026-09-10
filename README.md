# OverTheWire — Bandit Writeup (Level 0 → 30)

Báo cáo Task 1 – Tuần 4. Ghi chép quá trình giải wargame **Bandit** của OverTheWire:
hướng tiếp cận, các lệnh đã dùng, giải thích output và phần checklist lý thuyết cho từng level.

> Lưu ý: đây là bài tự làm, mục đích học tập. 

## Cấu trúc repo

```
.
├── README.md          # mục lục
├── levels/            # mỗi level một file markdown
│   ├── level-00.md
│   └── ...
└── images/            # ảnh chụp màn hình terminal
```

## Mục lục

| Level | Nội dung |
|---|---|
| [Level 0](levels/level-00.md) | Kết nối SSH lần đầu: cú pháp `ssh -p`, host key fingerprint, banner của server |
| [Level 1](levels/level-01.md) | `ls` liệt kê file, `cat` đọc nội dung file readme trong thư mục home |
| [Level 2](levels/level-02.md) | Đọc file có tên là dấu `-`: dùng đường dẫn `./-` hoặc `cat -- -` |
| [Level 3](levels/level-03.md) | Đọc file có khoảng trắng trong tên: escape `\ `, dấu nháy, hoặc tab completion |
| [Level 4](levels/level-04.md) | Tìm file ẩn bằng `ls -la`, đọc hiểu cột quyền `rwxr-xr-x`, owner và group |
| [Level 5](levels/level-05.md) | Phân biệt file text và binary, dùng `file` để lọc ra file human-readable |
| [Level 6](levels/level-06.md) | `find` với các option lọc theo kích thước, quyền và loại file |
| [Level 7](levels/level-07.md) | `find` toàn hệ thống theo user, group, size; lọc bỏ lỗi permission denied |
| [Level 8](levels/level-08.md) | `grep` tìm dòng chứa từ khóa trong file dữ liệu lớn |
| [Level 9](levels/level-09.md) | `sort` kết hợp `uniq -u` để tìm dòng chỉ xuất hiện đúng một lần |
| [Level 10](levels/level-10.md) | `strings` lọc chuỗi human-readable trong file binary, kết hợp `grep` |
| [Level 11](levels/level-11.md) | Giải mã Base64 bằng `base64 -d` |
| [Level 12](levels/level-12.md) | Giải mã ROT13 bằng `tr` để dịch ngược 13 vị trí bảng chữ cái |
| [Level 13](levels/level-13.md) | Hexdump ngược bằng `xxd -r`, giải nén nhiều lớp gzip/bzip2/tar |
| [Level 14](levels/level-14.md) | Đăng nhập bằng SSH private key với option `-i`, xử lý quyền file key |
| [Level 15](levels/level-15.md) | Gửi password qua TCP tới port nội bộ bằng `nc` / `telnet` |
| [Level 16](levels/level-16.md) | Kết nối TLS bằng `openssl s_client`, đọc thông tin certificate và session |
| [Level 17](levels/level-17.md) | Quét port bằng `nmap -sV`, xác định port có TLS rồi kết nối lấy key |
| [Level 18](levels/level-18.md) | So sánh hai file bằng `diff` để tìm dòng khác biệt |
| [Level 19](levels/level-19.md) | Vượt `.bashrc` tự logout bằng cách chạy lệnh trực tiếp qua SSH |
| [Level 20](levels/level-20.md) | Khai thác binary setuid `bandit20-do` để đọc file password của user khác |
| [Level 21](levels/level-21.md) | Kết hợp `nc` listener với binary setuid để nhận password qua socket |
| [Level 22](levels/level-22.md) | Đọc cronjob trong `/etc/cron.d/` và script mà nó thực thi |
| [Level 23](levels/level-23.md) | Cronjob dùng `md5sum` sinh tên file đích, tự tính hash để lấy password |
| [Level 24](levels/level-24.md) | Ghi script vào `/var/spool` để cronjob chạy với quyền user khác |
| [Level 25](levels/level-25.md) | Brute-force mã PIN 4 chữ số, sinh dải input và gửi qua `nc` |
| [Level 26](levels/level-26.md) | Thoát shell hạn chế: lợi dụng `more` và `vi` để mở shell thật |
| [Level 27](levels/level-27.md) | Dùng binary setuid `bandit27-do` để đọc password |
| [Level 28](levels/level-28.md) | `git clone` repo qua SSH và tìm password trong nội dung file |
| [Level 29](levels/level-29.md) | `git log` và `git show` để tìm dữ liệu đã bị xóa trong lịch sử commit |
| [Level 30](levels/level-30.md) | Duyệt các branch và tag trong git để tìm password ẩn |

## Kiến thức chính đã dùng

`ssh` · `ls` / `cat` với tên file đặc biệt · `file`, `du`, `find` với các option lọc ·
`grep`, `sort`, `uniq`, `strings`, `tr`, `base64`, `xxd` · `tar` / `gzip` / `bzip2` ·
`openssl s_client`, `nc`, `nmap` · `cron`, `setuid` · `git` (log, show, branch, tag, push)

## Nguồn

- https://overthewire.org/wargames/bandit/
