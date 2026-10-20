# OOP Library Management

Dự án quản lý thư viện cho bài cuối kỳ Lập trình hướng đối tượng. Repo gồm ba phần: ứng dụng console C++ (`main.cpp`), web server C++ trên Winsock (`web_server.cpp`), và bản web tĩnh trong thư mục `web/` chạy trên trình duyệt bằng `localStorage` với workflow deploy tự động lên GitHub Pages.

## 🔗 Xem web trực tiếp

[👉 Mở Library Management System](https://phonq-loandin.github.io/Library-Management-System-LMS-/)

Địa chỉ trực tiếp để sao chép:

```text
https://phonq-loandin.github.io/Library-Management-System-LMS-/
```

Web được triển khai bằng GitHub Pages từ repo [phonq-loandin/Library-Management-System-LMS-](https://github.com/phonq-loandin/Library-Management-System-LMS-). Trang `index.html` được mở tự động khi truy cập địa chỉ trên.

Nếu vừa cập nhật mã nguồn, đợi workflow **Deploy to GitHub Pages** hoàn tất trong tab **Actions**, rồi tải lại trang bằng `Ctrl+F5`.

Mở link trên trình duyệt để dùng app ngay mà không cần cài đặt gì. Đăng nhập bằng tài khoản admin mặc định (`admin` / `admin`) hoặc bấm **Đăng ký độc giả** để tạo tài khoản mới.

## Cấu trúc dự án

```text
Library-Management-System-LMS-/
├── main.cpp                 # Ứng dụng console C++ (các lớp OOP: Date, Document, Reader, ...)
├── web_server.cpp           # Web server C++ dùng Winsock (chỉ chạy trên Windows)
├── web/                     # Bản web tĩnh (HTML/CSS/JS) chạy bằng localStorage
│   ├── index.html
│   ├── app.js
│   ├── styles.css
│   └── gutenberg-catalog.js # Dữ liệu sách mẫu từ Project Gutenberg
├── scripts/                 # Công cụ Python tạo/cập nhật dữ liệu sách
├── docs/                    # Báo cáo đồ án và tài liệu thiết kế OOP
├── SoDo_Flowchart/           # Sơ đồ luồng xử lý và file nguồn draw.io
├── report_assets/           # Hình minh họa và nguồn sơ đồ thiết kế hệ thống
├── .github/workflows/       # GitHub Actions deploy lên GitHub Pages
└── README.md
```

Mã nguồn chính là `main.cpp`, `web_server.cpp` và toàn bộ thư mục `web/` (gồm dữ liệu sách, ảnh bìa, logo và giao diện). Giữ `.github/workflows/deploy.yml` để triển khai web và `.gitignore` để loại tệp sinh tự động.

Thư mục `scripts/` hỗ trợ cập nhật dữ liệu, không cần chạy để khởi động ứng dụng:

- `harvest_books.py` thu thập dữ liệu sách và ảnh bìa; `harvested_books.json` là dữ liệu đầu vào cho `splice_books.py` và `finalize_catalog.py`.
- `splice_books.py` thay danh sách sách mẫu trong `web/app.js`; `finalize_catalog.py` chọn 10 sách mẫu và dọn ảnh bìa theo danh sách đó.
- `build_gutenberg_catalog.py`, `build_mixed_catalog.py`, `add_vn_books.py` tạo các bộ dữ liệu khác nhau cho `web/gutenberg-catalog.js`.

Các script này có thể ghi đè dữ liệu mẫu hoặc ảnh bìa; chỉ chạy khi chủ động cập nhật danh mục. Báo cáo trong `docs/` và sơ đồ trong `SoDo_Flowchart/`, `report_assets/` được giữ để tra cứu thiết kế và phục vụ đồ án. Bản mã nguồn lồng trùng, thư mục tạm xử lý Word, bản thử chuyển đổi Word, log, tệp sao lưu draw.io và công cụ tạo/chuyển đổi báo cáo đã được dọn.

## Build và chạy phần C++

Yêu cầu có trình biên dịch C++ (g++/MinGW hoặc MSVC) hỗ trợ C++17.

### Ứng dụng console

```powershell
g++ -std=c++17 -O2 main.cpp -o library.exe
.\library.exe
```

### Web server C++ (Windows)

`web_server.cpp` dùng Winsock nên cần link thêm `ws2_32`:

```powershell
g++ -std=c++17 -O2 web_server.cpp -o library_web.exe -lws2_32
.\library_web.exe
```

## Cách chạy web local

Chạy static server trong thư mục `web`:

```powershell
cd web
python -m http.server 4173
```

Mở trình duyệt tại:

```text
http://127.0.0.1:4173/index.html
```

## Tài khoản mặc định

- Quản trị viên: `admin`
- Mật khẩu: `admin`

Độc giả có thể tự đăng ký tại màn hình **Đăng ký độc giả**. Admin có thể tạo hồ sơ độc giả, nhân viên, và quản trị viên trong mục **Con người**.

## Hướng dẫn sử dụng

### Đăng nhập

1. Mở app tại `http://127.0.0.1:4173/index.html`.
2. Đăng nhập bằng tài khoản admin mặc định (`admin` / `admin`), hoặc bấm **Đăng ký độc giả** để tạo tài khoản độc giả mới.
3. Sau khi đăng nhập, menu bên trái sẽ hiển thị các trang tương ứng với quyền của tài khoản.

### Dành cho quản trị viên / nhân viên

- **Tài liệu**: thêm, sửa, xóa sách; tìm kiếm và lọc theo loại (giáo trình, sách tham khảo, sách khác, báo/tạp chí, bài nghiên cứu) và theo chủ đề.
- **Con người**: tạo hồ sơ độc giả, nhân viên, quản trị viên; tạo tài khoản đăng nhập kèm username/password.
- **Mượn / Trả**: ghi nhận mượn sách, trả sách; hệ thống tự cập nhật số lượng còn trong kho.
- **Nhập / Xuất**: nhập thêm số lượng tài liệu vào kho hoặc xuất tài liệu khỏi kho.
- Có thể xem lịch sử giao dịch để theo dõi hoạt động mượn/trả.

### Dành cho độc giả

- **Trang độc giả**: xem bảng **Sách trong thư viện** (các tài liệu còn số lượng), danh sách tài liệu đang mượn, và lịch sử giao dịch của mình.
- **Tài liệu**: tra cứu, tìm kiếm và lọc tài liệu theo loại/chủ đề.
- Trạng thái mỗi tài liệu:
  - `Mượn qua thủ thư` — sách có thể mượn, liên hệ thủ thư để mượn.
  - `Đọc tại chỗ` — báo/tạp chí hoặc bài nghiên cứu, chỉ đọc tại thư viện.
- Độc giả không thể tự thêm/sửa tài liệu hay tự mượn/trả; các thao tác này do nhân viên/admin thực hiện.

### Mẹo

- Dùng ô **Tìm trong chủ đề** để tìm sâu theo chủ đề/mô tả, khác với ô tìm chung theo mã, tên, tác giả.
- Dữ liệu lưu trong `localStorage` của trình duyệt; xóa dữ liệu trình duyệt sẽ đưa app về trạng thái mẫu ban đầu.

## Các thay đổi đã làm

### Phân quyền độc giả

- Độc giả chỉ thấy được hai trang: **Trang độc giả** và **Tài liệu**.
- Ẩn trang/nút **Mượn / Trả** với tài khoản độc giả.
- Chặn cứng các form quản lý nếu độc giả cố gắng submit bằng devtools.
- Các hành động bị chặn gồm: thêm tài liệu, thêm độc giả/nhân viên/admin, mượn, trả, nhập, xuất, cập nhật file sách.

### Trang độc giả

- Thêm bảng **Sách trong thư viện** vào trang độc giả.
- Độc giả xem được sách/tài liệu còn số lượng trong kho.
- Bảng hiển thị mã sách, tên sách, loại, số lượng, và trạng thái:
  - `Mượn qua thủ thư` với sách có thể mượn.
  - `Đọc tại chỗ` với báo/tạp chí hoặc bài nghiên cứu.
- Độc giả vẫn xem được danh sách tài liệu đang mượn và lịch sử giao dịch của mình.

### Tìm kiếm và lọc sách theo chủ đề

- Cải tiến màn hình **Tài liệu** để tìm kiếm đúng hơn theo từng loại sách.
- Bộ lọc **Chủ đề theo loại** tự đổi nội dung theo loại tài liệu đang chọn:
  - **Giáo trình**: lọc theo bộ môn, mã môn học, tên môn.
  - **Sách tham khảo**: lọc theo chủ đề/ngành/lĩnh vực tham khảo.
  - **Sách khác**: lọc theo thể loại như truyện, văn học, kỹ năng, ngoại ngữ.
  - **Báo/tạp chí**: lọc theo báo, tạp chí, số phát hành, tháng.
  - **Bài nghiên cứu**: lọc theo lĩnh vực nghiên cứu, cơ quan chủ quản.
- Thêm ô **Tìm trong chủ đề** để tìm sâu trong chủ đề/mô tả của tài liệu, riêng với ô tìm chung theo mã, tên, tác giả.

### Tạo tài khoản độc giả

- Sửa lỗi admin/nhân viên tạo người đọc nhưng không tạo được tài khoản đăng nhập.
- Form **Đăng ký người đọc** trong trang quản lý này đã tạo luôn account role `reader` từ username/password.
- Sửa luồng đăng ký công khai:
  - Đăng ký độc giả xong tự động đăng nhập vào app.
  - Nếu hồ sơ độc giả đã tồn tại nhưng chưa có account, hệ thống tạo account cho hồ sơ đó.
  - Nếu mã độc giả đã có account, hệ thống báo mã độc giả đã tồn tại.

### Ảnh bìa sách

- Dữ liệu Project Gutenberg trong `web/gutenberg-catalog.js` không có ảnh bìa thật (`coverImage` rỗng).
- App tự lấy ảnh bìa thật từ Project Gutenberg bằng `gutenbergId` theo mẫu URL `https://www.gutenberg.org/cache/epub/{id}/pg{id}.cover.medium.jpg`.
- Nếu Project Gutenberg không có ảnh bìa hoặc ảnh bị lỗi, app fallback về bìa giả.
- Bìa giả hiển thị tên sách rút gọn thay vì chỉ hiển mã `TVS...`.
- Mã `TVS...` vẫn được hiển nhỏ ở dòng phụ để người dùng biết mã tài liệu.

### Màn hình đăng nhập

- Sửa lỗi khung đăng nhập bị tràn và hiện thanh cuộn ngang/dọc.
- Giảm kích thước logo và padding của card đăng nhập.
- Thêm responsive cho màn hình nhỏ/thấp.
- Ẩn phần gợi ý tài khoản khi màn hình thấp để form đăng nhập gọn hơn.

### Kiểm tra đã thực hiện

- Chạy `node --check web\app.js` để kiểm tra cú pháp JavaScript.
- Test luồng admin tạo độc giả có username/password, đăng xuất, đăng nhập bằng tài khoản độc giả.
- Test độc giả chỉ thấy **Trang độc giả** và **Tài liệu**.
- Test bảng **Sách trong thư viện** render danh sách tài liệu còn trong kho.

## Deploy

Repo có workflow `.github/workflows/deploy.yml`. Khi push lên nhánh `main`, GitHub Actions sẽ deploy thư mục `web/` lên GitHub Pages.

Nếu cần deploy lại thủ công, vào tab **Actions** trên GitHub, chọn **Deploy to GitHub Pages**, rồi bấm **Run workflow**.
