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
| [Level 0](levels/level-00.md) | Kết nối SSH lần đầu tới server, đọc hiểu host key fingerprint và banner |
| [Level 1](levels/level-01.md) | Sau khi ssh vào server thì ở thư mục home luôn, nên hướng làm sẽ là ls để liệt kê các file… |
| [Level 2](levels/level-02.md) | Ở đây tên file chứa password là - , vì đây là kí tự đặc biệt nên em nghĩ lệnh cat sẽ không… |
| [Level 3](levels/level-03.md) | Đề bài nói rằng tên file là --spaces in this file name-- ở thư mục home nên chắc chắn dùng… |
| [Level 4](levels/level-04.md) | Đề bài nói rằng password nằm trong 1 hidden file bên trong inhere directory, nên đầu tiên … |
| [Level 5](levels/level-05.md) | Tiếp tục cd vào thư mục inhere, dùng lệnh ls -la để liệt kê các file, vì ở đây đề bảo pass… |
| [Level 6](levels/level-06.md) | Đề bài nói rằng file chứa password nằm dưới thư mục inhere và cho các thông số của file đó… |
| [Level 7](levels/level-07.md) | Đầu tiên dùng câu lệnh find . -user bandit7 -group bandit6 -size 33c và không thấy kết quả… |
| [Level 8](levels/level-08.md) | Dùng lệnh grep để tìm dòng có chữ millionth trong file data.txt… |
| [Level 9](levels/level-09.md) | Vì đề bài bảo password nằm trong file data.txt và là dòng chỉ xuất hiện đúng 1 lần nên sẽ … |
| [Level 10](levels/level-10.md) | Dùng lệnh strings + tên file để lọc ra các dòng human readable, rồi tìm dòng nào bắt đầu b… |
| [Level 11](levels/level-11.md) | File chứa password bị mã hóa Base64, dùng lệnh base64 -d + tên file… |
| [Level 12](levels/level-12.md) | Đề bài bảo rằng nội dung trong file data.txt bị dịch 13 vị trí, kể cả chữ in hoa và in thư… |
| [Level 13](levels/level-13.md) | Tạo 1 thư mục trong thư mục tạm, cp file chứa pass sang thư mục đó, rồi reverse file hexdu… |
| [Level 14](levels/level-14.md) | Đề bài bảo rằng không có pass cho level tiếp theo, mà phải dùng sshkey nên sẽ scp file ssh… |
| [Level 15](levels/level-15.md) | Sử dụng telnet, nc hoặc openssl để kết nối vào port 30000 trên localhost và nhập password … |
| [Level 16](levels/level-16.md) | Đầu tiên, tạo 1 cặp ssh key ở máy windows, lưu public key vào máy kali ở trong file author… |
| [Level 17](levels/level-17.md) | Dùng nmap để quét các port đang mở, sử dụng options -sV để nmap đoán port có dùng TLS khôn… |
| [Level 18](levels/level-18.md) | Dùng lệnh diff để xem các dòng khác nhau giữa 2 file.… |
| [Level 19](levels/level-19.md) | Vì .bashrc được cấu hình để ngay sau khi ssh vào lv 18 mình sẽ bị logout nên em sẽ thử chạ… |
| [Level 20](levels/level-20.md) | Vì đề bài bảo có một file có setuid và hãy thử chạy nó không cần tham số để xem cách sử dụ… |
| [Level 21](levels/level-21.md) | Mở một kết nối của mình tại cổng bất kì sử dụng nc, echo password lv20 vào làm stdin rồi d… |
| [Level 22](levels/level-22.md) | Đọc nội dung trong file cron bài cho, xem nó dùng script gì rồi đọc nội dung trong script … |
| [Level 23](levels/level-23.md) | Vào đọc file script rồi thực hiện đọc và chạy thử script.… |
| [Level 24](levels/level-24.md) | Đọc file cronjob và từ đó đọc script bên trong để làm theo và lấy password.… |
| [Level 25](levels/level-25.md) | Tạo 1 script ghi các input để bruteforce mã PIN từ 0000 đến 9999 rồi sau đó truyền vào por… |
| [Level 26](levels/level-26.md) | Kết nối vào bandit26 nhờ sshkey rồi thu nhỏ terminal, dùng vi để sửa shell nhờ vào lỗi của… |
| [Level 27](levels/level-27.md) | Sau khi mở được shell con ở level 26, ls -la thấy file bandit27-do được setuid nên sẽ dùng… |
| [Level 28](levels/level-28.md) | Clone git về máy mình, cd vào thư mục repo rồi tìm mật khẩu.… |
| [Level 29](levels/level-29.md) | Clone git về Clone git về máy mình, cd vào thư mục repo rồi tìm mật khẩu.… |
| [Level 30](levels/level-30.md) | Clone git về và check git log, check các branch để tìm password.… |

## Kiến thức chính đã dùng

`ssh` · `ls` / `cat` với tên file đặc biệt · `file`, `du`, `find` với các option lọc ·
`grep`, `sort`, `uniq`, `strings`, `tr`, `base64`, `xxd` · `tar` / `gzip` / `bzip2` ·
`openssl s_client`, `nc`, `nmap` · `cron`, `setuid` · `git` (log, show, branch, tag, push)

## Nguồn

- https://overthewire.org/wargames/bandit/
