[README.md](https://github.com/user-attachments/files/35471453/README.md)
# ChatNow – Phần mềm chat online bằng Java (Swing + Socket + MySQL)

Ứng dụng chat nhiều người dùng viết bằng **Java Swing**, giao tiếp qua **Socket (TCP)** và lưu dữ liệu trong **MySQL** (chạy được với XAMPP). Gồm hai phần chạy riêng:

| Thành phần | Chạy ở đâu | File chạy (main) | Việc chính |
|---|---|---|---|
| **Máy host** (quản trị + máy chủ) | 1 máy | `ServerApp.java` | Bật/tắt máy chủ, quản lý người dùng, bạn bè, phòng chat, tin nhắn |
| **Máy người dùng** (client) | nhiều máy | `LoginFrame.java` | Đăng ký, đăng nhập, chat, kết bạn, tạo nhóm... |

---

## Bắt đầu nhanh (không cần NetBeans)

1. Cài **Java 8 trở lên** và bật **MySQL** trong XAMPP.
2. Mở `http://localhost/phpmyadmin` → tab **Import** → chọn `chat_db.sql` → *Import*.
3. Driver MySQL (`mysql-connector-java-5.1.49.jar`) **đã kèm sẵn** trong `ChatNow-Apps/lib/`, không cần làm gì thêm.
4. **Nhấp đúp `ChatNow-Apps/ChatServer.jar`** (hoặc `Chay-Server.bat`) → *Kiểm tra CSDL* → *Khởi động server*.
5. **Nhấp đúp `ChatNow-Apps/ChatClient.jar`** → đăng nhập bằng `an` / `123456` (hoặc đăng ký tài khoản mới).

Muốn xem hoặc sửa mã nguồn: xem [mục 4](#4-cài-đặt-chi-tiết) (tạo project NetBeans với các file trong `src/`).

---

## Mục lục

1. [Tính năng](#1-tính-năng)
2. [Yêu cầu hệ thống](#2-yêu-cầu-hệ-thống)
3. [Cấu trúc thư mục và các file](#3-cấu-trúc-thư-mục-và-các-file)
4. [Cài đặt chi tiết](#4-cài-đặt-chi-tiết)
5. [Chạy chương trình](#5-chạy-chương-trình)
6. [Hướng dẫn sử dụng máy host](#6-hướng-dẫn-sử-dụng-máy-host)
7. [Hướng dẫn sử dụng máy người dùng](#7-hướng-dẫn-sử-dụng-máy-người-dùng)
8. [Giao diện sáng/tối, ảnh đại diện, logo](#8-giao-diện-sángtối-ảnh-đại-diện-logo)
9. [Chat giữa nhiều máy (LAN / Internet)](#9-chat-giữa-nhiều-máy-lan--internet)
10. [Cơ sở dữ liệu](#10-cơ-sở-dữ-liệu)
11. [Giao thức giữa client và server](#11-giao-thức-giữa-client-và-server)
12. [Chạy không cần NetBeans (file ứng dụng .jar / .exe)](#12-chạy-không-cần-netbeans-file-ứng-dụng-jar--exe)
13. [Xử lý lỗi thường gặp](#13-xử-lý-lỗi-thường-gặp)
14. [Bảo mật và giới hạn](#14-bảo-mật-và-giới-hạn)
15. [Hướng phát triển thêm](#15-hướng-phát-triển-thêm)

---

## 1. Tính năng

### Máy người dùng
- **Đăng ký / đăng nhập** có kiểm tra từng ô (tên đăng nhập, họ tên, email, số điện thoại, mật khẩu), nút *Hiện mật khẩu*, nhấn **Enter** để chuyển sang ô kế tiếp (ô cuối thì gửi form).
- **Chat nhóm và chat riêng 1-1**, giao diện bong bóng kiểu Messenger/Zalo, có giờ gửi, tải 200 tin gần nhất khi mở phòng.
- **Sửa / xóa / sao chép tin nhắn** của chính mình (chuột phải vào tin nhắn). Tin đã sửa có nhãn "đã sửa".
- **Emoji** (nút mặt cười cạnh ô nhập).
- **Tìm kiếm tin nhắn** trong phòng hiện tại hoặc trong tất cả các cuộc trò chuyện; nhấp đúp kết quả để mở phòng.
- **Tạo chat riêng, tạo nhóm** (công khai hoặc riêng tư), **khám phá và tham gia phòng công khai**, xem thành viên, rời nhóm.
- **Kết bạn**: gửi / chấp nhận / từ chối / thu hồi lời mời, hủy kết bạn, tìm người để kết bạn, nhắn tin nhanh với bạn bè.
- **Thông báo** khi có lời mời kết bạn mới, **huy hiệu số tin chưa đọc** ở từng cuộc trò chuyện, danh sách **người đang online**.
- **Tài khoản**: sửa hồ sơ (kể cả *tên đăng nhập*), đổi mật khẩu, đổi/đặt lại **ảnh đại diện**, đổi/đặt lại **ảnh nhóm** (chủ nhóm hoặc quản trị viên), đăng xuất.
- **Giao diện sáng / tối**, ghi nhớ lựa chọn cho lần sau.

### Máy host
- **Tổng quan**: thống kê (đang online, số người dùng, số phòng, số tin nhắn), cấu hình MySQL và cổng, nút khởi động/dừng server, kiểm tra kết nối CSDL, nhật ký hoạt động theo thời gian thực.
- **Người dùng**: *Thêm, Sửa (kể cả tên đăng nhập), Xóa, Khóa/Mở khóa, Đặt lại mật khẩu, Reset avatar*, tìm kiếm ngay khi gõ.
- **Bạn bè**: xem, thêm, đổi trạng thái (pending / accepted / blocked), xóa quan hệ giữa các người dùng.
- **Phòng chat**: *Thêm, Sửa, Xóa, Reset avatar nhóm*, quản lý thành viên (thêm / xóa khỏi phòng).
- **Tin nhắn**: tìm theo nội dung hoặc người gửi, xóa tin nhắn vi phạm.
- Mọi thay đổi của host được **đẩy tức thì** tới các máy đang online (khóa tài khoản thì người đó bị ngắt kết nối, xóa tin nhắn thì tin biến mất khỏi khung chat...).

---

## 2. Yêu cầu hệ thống

| Thành phần | Yêu cầu |
|---|---|
| Java | Chạy ứng dụng: **Java 8 trở lên** (hai file `.jar` dựng sẵn đã biên dịch ở mức Java 8 nên chạy được trên Java 8, 11, 17, **21**...). Mã nguồn đã biên dịch và kiểm tra với JDK 21. Tạo file `.exe` bằng `Tao-EXE.bat` cần **JDK 14 trở lên** (khuyến nghị 17 hoặc 21) |
| IDE | **NetBeans** (bản bất kỳ có hỗ trợ Java with Ant) |
| CSDL | **MySQL 5.7+ / MariaDB 10.2+** – dùng luôn MySQL trong **XAMPP** |
| Thư viện | **MySQL Connector/J** (file `.jar`). Khuyến nghị bản **5.1.49** khi dùng XAMPP |
| Hệ điều hành | Windows / Linux / macOS (giao diện dùng font *Segoe UI*, máy không có font này sẽ tự dùng font thay thế) |
| Mạng | Các máy cùng mạng LAN/Wi-Fi (hoặc đã mở cổng, xem [mục 9](#9-chat-giữa-nhiều-máy-lan--internet)) |

---

## 3. Cấu trúc thư mục và các file

```
ChatNow/
├── README.md              ← file này
├── chat_db.sql            ← script tạo CSDL + dữ liệu mẫu (nạp bằng phpMyAdmin)
├── ChatNow-Apps/          ← ỨNG DỤNG CHẠY SẴN, không cần NetBeans (xem mục 12)
│   ├── ChatServer.jar        ← nhấp đúp để mở máy host
│   ├── ChatClient.jar        ← nhấp đúp để mở máy người dùng
│   ├── Chay-Server.bat · Chay-Client.bat   ← mở nhanh, tự tìm Java
│   ├── Tao-EXE.bat           ← tạo ChatServer.exe và ChatClient.exe (tùy chọn)
│   ├── logo.ico · logo.png   ← icon cho file .exe / logo
│   └── lib/                  ← driver MySQL đã kèm sẵn: mysql-connector-java-5.1.49.jar (chỉ máy host cần)
└── src/                   ← MÃ NGUỒN JAVA (chép vào project NetBeans nếu muốn sửa code)
    ├── Theme.java         ← màu sắc, font, giao diện sáng/tối, nút/ô nhập/bảng tự vẽ, thanh cuộn
    ├── ImageUtil.java     ← cắt vuông, thu nhỏ, nén JPEG cho ảnh đại diện
    ├── Dao.java           ← toàn bộ truy vấn JDBC (người dùng, phòng, tin nhắn, bạn bè, avatar)
    ├── ChatServer.java    ← động cơ máy chủ socket đa luồng + giao thức
    ├── ServerApp.java     ← giao diện máy host (chạy file này để làm máy chủ)  [main]
    ├── LoginFrame.java    ← đăng nhập / đăng ký (chạy file này để làm máy người dùng)  [main]
    ├── ChatClient.java    ← cửa sổ chat chính của người dùng
    └── logo.png           ← logo/icon mặc định (có thể thay bằng ảnh của bạn)
```

Toàn bộ code khoảng 4.800 dòng, dùng **package mặc định** (không có dòng `package ...;`).

| File | Vai trò chi tiết |
|---|---|
| `Theme.java` | Bảng màu sáng/tối (`LIGHT`, `DARK`), chuyển giao diện lúc đang chạy (`Theme.toggleDark()`), `RoundButton`, `RoundField`, `RoundPassword`, `RoundedPanel`, định dạng bảng, thanh cuộn mỏng, biểu tượng avatar tròn, `appIcon()` (logo ứng dụng), hộp thoại nhập liệu `Theme.form(...)` |
| `ImageUtil.java` | `squareScale` (cắt vuông giữa ảnh + thu nhỏ từng bước cho mượt), `toJpeg`, `fromBytes` |
| `Dao.java` | Kết nối MySQL (`Dao.url`, `Dao.dbUser`, `Dao.dbPass`), băm mật khẩu SHA-256, đăng nhập/đăng ký, CRUD người dùng, phòng, thành viên, tin nhắn, tìm kiếm, bạn bè, avatar; tự thêm cột `avatar` nếu CSDL cũ chưa có (`ensureSchema`) |
| `ChatServer.java` | Mở `ServerSocket`, mỗi client một luồng (`Handler`), xác thực, xử lý lệnh, phát tin nhắn cho thành viên phòng đang online, các hàm cho host (`kick`, `pushRoom`, `pushFriends`, `pushAvatar`...) |
| `ServerApp.java` | Giao diện host: Tổng quan, Người dùng, Bạn bè, Phòng chat, Tin nhắn |
| `LoginFrame.java` | Màn hình đăng nhập/đăng ký, kết nối tới server |
| `ChatClient.java` | Danh sách hội thoại, khung chat bong bóng, các hộp thoại tìm kiếm / bạn bè / chat mới / khám phá, avatar |

---

## 4. Cài đặt chi tiết

### Bước 1 – Cài MySQL bằng XAMPP và nạp CSDL

1. Mở **XAMPP Control Panel**, bấm **Start** ở dòng **MySQL** (nên Start cả **Apache** để dùng phpMyAdmin).
2. Mở trình duyệt, vào `http://localhost/phpmyadmin`.
3. Ở màn hình chính (chưa chọn database nào) bấm tab **Import** → *Choose File* → chọn **`chat_db.sql`** → kéo xuống cuối bấm **Import**.
   - Có thể thay bằng tab **SQL**: mở `chat_db.sql` bằng Notepad, dán toàn bộ nội dung rồi bấm *Go*.
   - Hoặc dùng dòng lệnh: `C:\xampp\mysql\bin\mysql -u root < chat_db.sql`
4. Kiểm tra: bên trái xuất hiện database **`chat_db`** gồm 8 bảng; bảng `users` có 3 tài khoản mẫu.

> ⚠ Script bắt đầu bằng `DROP DATABASE IF EXISTS chat_db` – **chạy lại sẽ xóa sạch dữ liệu cũ**. Nếu đã có dữ liệu thật, hãy xuất (Export) sao lưu trước.

Mặc định XAMPP dùng tài khoản `root`, **mật khẩu trống**, cổng **3306**. Đây cũng là giá trị mặc định trong ServerApp nên thường không cần sửa gì.

### Bước 2 – Tải thư viện JDBC (MySQL Connector/J)

NetBeans bản mới không còn kèm sẵn "MySQL JDBC Driver". **Gói này đã kèm sẵn file driver** tại `ChatNow-Apps/lib/mysql-connector-java-5.1.49.jar` – bạn dùng luôn ở bước 3. Nếu cần tải lại:

- Bản khuyến nghị (tương thích tốt với XAMPP/MariaDB):  
  `https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.49/mysql-connector-java-5.1.49.jar`
- Hoặc vào `https://dev.mysql.com/downloads/connector/j/` → *Platform Independent* → giải nén lấy `.jar` (mục *Archives* để chọn bản 5.1.49).

### Bước 3 – Tạo project trong NetBeans

1. **File → New Project → Java with Ant → Java Application**. Bỏ chọn *Create Main Class*. Đặt tên project (ví dụ `ChatApp`).
2. Chép **7 file `.java`** (`Theme, ImageUtil, Dao, ChatServer, ServerApp, LoginFrame, ChatClient` – có sẵn trong thư mục `src/` của gói này) vào thư mục `src` của project (nằm trong *Source Packages → `<default package>`*).
   - Nếu NetBeans tự thêm dòng `package ten_package;` vào đầu file thì **xóa dòng đó đi** (hoặc thêm đúng dòng đó vào cả 7 file). Các file phải cùng package, nếu không sẽ báo lỗi *"package Theme does not exist"*.
3. Chép **`logo.png`** vào cùng thư mục `src` (không bắt buộc – nếu không có sẽ dùng biểu tượng vẽ sẵn).
4. Chuột phải **Libraries → Add JAR/Folder...** → chọn file `mysql-connector-java-5.1.49.jar` (có sẵn trong gói tại `ChatNow-Apps/lib/`) → *Open*.
5. Chuột phải project → **Properties → Sources → Encoding = UTF-8** (để hiển thị đúng tiếng Việt và emoji).
6. Bấm **Clean and Build** (Shift + F11). Không còn gạch đỏ là xong.

---

## 5. Chạy chương trình

Luôn chạy **theo thứ tự**: MySQL → máy host → máy người dùng.

1. **Bật MySQL** trong XAMPP (nút *Start*).
2. **Chạy máy host**: chuột phải `ServerApp.java` → **Run File** (Shift + F6).
   - Ở trang **Tổng quan**, kiểm tra *Chuỗi kết nối*, *Tài khoản*, *Mật khẩu*, *Cổng server* (mặc định `5000`).
   - Bấm **Kiểm tra CSDL** → thấy "Kết nối CSDL thành công!" là đạt.
   - Bấm **Khởi động server**. Nhật ký hiện *"Server khởi động tại cổng 5000 – IP máy chủ: ..."*, nút đổi thành **Dừng server**. **Giữ cửa sổ này mở.**
3. **Chạy máy người dùng**: chuột phải `LoginFrame.java` → **Run File**. Chạy nhiều lần để mở nhiều cửa sổ thử nhiều tài khoản cùng lúc.
   - Ô *Máy chủ*: `localhost` nếu cùng máy, hoặc IP máy host nếu khác máy. Ô cổng: `5000`.
   - Đăng nhập thử bằng `an` / `123456`.

### Tài khoản và dữ liệu mẫu (có sẵn trong `chat_db.sql`)

| Tên đăng nhập | Mật khẩu | Ghi chú |
|---|---|---|
| `an` | `123456` | Chủ phòng "Lớp CNTT"; đã là bạn của `binh`; có lời mời kết bạn từ `chi` |
| `binh` | `123456` | Bạn của `an` |
| `chi` | `123456` | Đã gửi lời mời kết bạn cho `an` |

Dữ liệu mẫu còn có phòng công khai **"Lớp CNTT"** (cả 3 người đều tham gia), một phòng chat riêng `an` ↔ `binh` và vài tin nhắn.

---

## 6. Hướng dẫn sử dụng máy host

Thanh bên trái có 5 mục. Góc dưới thanh bên: trạng thái server (xanh = đang chạy, đỏ = đang dừng) và nút **Chế độ tối / Chế độ sáng**.

### 6.1 Tổng quan
- **4 thẻ thống kê**: Đang online, Người dùng, Phòng chat, Tin nhắn.
- **Cấu hình**: chuỗi kết nối MySQL, tài khoản, mật khẩu, cổng server. Các ô này bị khóa khi server đang chạy.
- **Khởi động server / Dừng server**; **Kiểm tra CSDL**.
- Khi server chạy, dòng thông tin hiển thị *"Máy khác kết nối tới IP: ... | cổng ..."* – đưa IP này cho máy người dùng.
- **Nhật ký hoạt động**: đăng nhập/thoát, tin nhắn gửi đi, tạo nhóm, lời mời kết bạn, lỗi CSDL...

### 6.2 Người dùng
Gõ vào ô tìm kiếm (góc phải) để lọc theo tên đăng nhập, họ tên hoặc email.

| Nút | Tác dụng |
|---|---|
| **+ Thêm** | Tạo tài khoản mới (tên đăng nhập, mật khẩu, họ tên, email, số điện thoại). Người mới tự vào phòng công khai đầu tiên |
| **Sửa** | Sửa **tên đăng nhập**, họ tên, email, số điện thoại. Nếu đổi tên đăng nhập mà người đó đang online, họ bị ngắt kết nối và phải đăng nhập lại bằng tên mới; các người liên quan được cập nhật tên mới ngay |
| **Xóa** | Xóa người dùng. Nếu người đó **đã có dữ liệu** (tin nhắn, phòng, quan hệ bạn bè) thì **không xóa hẳn được** (ràng buộc khóa ngoại) – phần mềm tự chuyển sang trạng thái **bị khóa** để giữ lịch sử và báo cho bạn biết |
| **Khóa / Mở khóa** | Khóa thì người đó bị ngắt kết nối ngay và không đăng nhập được |
| **Đặt lại mật khẩu** | Đặt mật khẩu mới (4–50 ký tự, không chứa `\|`) |
| **Reset avatar** | Đưa ảnh đại diện của người được chọn về mặc định (các máy online cập nhật ngay) |
| **Làm mới** | Tải lại bảng |

Bảng có các cột: ID, Tên đăng nhập, Họ tên, Email, Điện thoại, Trực tuyến, Trạng thái (Hoạt động/Bị khóa), Avatar (Tùy chỉnh/Mặc định), Ngày tạo. Bấm tiêu đề cột để sắp xếp.

### 6.3 Bạn bè
Quản lý các dòng trong bảng `friendships`.
- **+ Thêm quan hệ**: nhập 2 tên đăng nhập và trạng thái (`pending` = chờ chấp nhận, `accepted` = đã là bạn, `blocked` = chặn).
- **Đổi trạng thái**, **Xóa**: áp dụng cho dòng đang chọn. Người dùng liên quan đang online được cập nhật danh sách bạn bè ngay.

### 6.4 Phòng chat
Bảng trên: danh sách phòng. Bảng dưới: thành viên của phòng đang chọn.
- **+ Thêm phòng**: nhập tên, mô tả, loại (`public` hoặc `private`) và **chủ phòng** (tên đăng nhập).
- **Sửa**: đổi tên và mô tả (không áp dụng cho chat riêng).
- **Xóa**: xóa phòng **cùng toàn bộ tin nhắn** trong phòng; thành viên đang online bị gỡ phòng khỏi danh sách.
- **Reset avatar nhóm**: đưa ảnh nhóm về mặc định.
- **+ Thêm thành viên** / **Xóa khỏi phòng** (ở bảng dưới). Phòng không còn thành viên nào sẽ tự bị xóa. Không thêm thành viên vào chat riêng 1-1.

### 6.5 Tin nhắn
- Gõ từ khóa để tìm theo **nội dung hoặc người gửi** (tối đa 500 kết quả mới nhất).
- **Xóa tin nhắn**: xóa **vĩnh viễn** khỏi CSDL và khỏi khung chat của mọi người. (Khác với người dùng tự xóa tin của mình – đó chỉ là xóa mềm, cột `is_deleted`.)
- Cột *Trạng thái* cho biết tin nhắn nào đã bị người dùng xóa mềm.

---

## 7. Hướng dẫn sử dụng máy người dùng

### 7.1 Đăng nhập và đăng ký
- **Đăng nhập**: nhập tên đăng nhập, mật khẩu → **Enter** ở ô tên để chuyển xuống ô mật khẩu, **Enter** ở ô mật khẩu để đăng nhập.
- **Đăng ký**: bấm **Đăng ký ngay**. Các ô: tên đăng nhập, họ tên, email, số điện thoại (không bắt buộc), mật khẩu, nhập lại mật khẩu. **Enter** chuyển lần lượt qua từng ô, ở ô cuối thì tạo tài khoản. Đăng ký xong **tự đăng nhập**.
- Nút **Chế độ tối / Chế độ sáng** nằm cạnh ô cổng; **Hiện mật khẩu** để xem mật khẩu đang gõ.

Quy tắc nhập liệu:

| Mục | Quy tắc |
|---|---|
| Tên đăng nhập | 1–20 ký tự: chữ **không dấu**, số, `_` `.` `-` |
| Mật khẩu | 4–50 ký tự, **không chứa dấu `\|`** |
| Họ tên | Không trống, tối đa 100 ký tự, không chứa `\|` |
| Email | Đúng dạng `ten@ten-mien.com` |
| Số điện thoại | 8–15 ký tự gồm số, `+`, dấu cách (không bắt buộc) |

### 7.2 Màn hình chính

```
┌────────────────┬───────────────────────────────────────┬──────────────────┐
│ [Avatar] Tên    │ [Avatar nhóm] Tên phòng               │ Đang online (n)  │
│ @username       │ trạng thái    [Tìm tin nhắn][Thành viên][Rời nhóm]      │
│ [Chat mới][Khám phá]│                                    │ ● an  (Bạn)     │
│ [Bạn bè][Tài khoản] │      ...  bong bóng tin nhắn ...    │ ● binh          │
│ [Tìm cuộc trò chuyện]│                                   │ (nhấp đúp để    │
│ ─ Danh sách phòng ─ │                                   │  nhắn riêng)     │
│  (huy hiệu chưa đọc)│ [😊] [Nhập tin nhắn...      ] [Gửi]│                  │
└────────────────┴───────────────────────────────────────┴──────────────────┘
```

- **Cột trái**: ảnh đại diện + tên của bạn; 4 nút *Chat mới, Khám phá, Bạn bè, Tài khoản*; ô lọc hội thoại; danh sách hội thoại với **huy hiệu đỏ** cho số tin chưa đọc. Chat riêng hiện "● Đang online" khi người kia online. Khi có thông báo (lời mời kết bạn...) một **dải vàng** hiện ở dưới cùng cột trái trong 7 giây.
- **Giữa**: thanh tiêu đề phòng, khung tin nhắn, ô nhập.
- **Cột phải**: người đang online – **nhấp đúp** một người để nhắn riêng.

### 7.3 Gửi, sửa, xóa tin nhắn
- Gõ nội dung rồi **Enter** hoặc bấm **Gửi** (tối đa 2000 ký tự mỗi tin). Nút 😊 mở bảng emoji.
- **Chuột phải** vào bong bóng tin nhắn:
  - *Sao chép* – dùng được với mọi tin.
  - *Sửa tin nhắn* – chỉ tin của bạn. Nội dung được đưa lên ô nhập kèm dải vàng "Đang sửa tin nhắn"; sửa xong bấm Enter/Gửi, bấm **Esc** (hoặc *Hủy*) để thôi sửa.
  - *Xóa tin nhắn* – chỉ tin của bạn; tin biến mất khỏi khung chat của mọi người.

### 7.4 Tìm kiếm tin nhắn
Bấm **Tìm tin nhắn** (thanh tiêu đề). Nhập từ khóa, tích *Tìm trong tất cả cuộc trò chuyện* nếu muốn tìm mọi nơi, bấm **Tìm** hoặc Enter. **Nhấp đúp** một kết quả để mở phòng chứa tin đó. Chỉ tìm trong các phòng bạn là thành viên.

### 7.5 Tạo hội thoại mới và khám phá phòng
- **Chat mới**: gõ để tìm người dùng, rồi
  - chọn **đúng 1 người** → **Nhắn riêng với người đã chọn**;
  - hoặc giữ **Ctrl** chọn nhiều người, nhập *Tên nhóm*, chọn *Nhóm công khai / Nhóm riêng tư* → **Tạo nhóm với những người đã chọn**. Bạn trở thành **chủ nhóm**.
- **Khám phá**: danh sách phòng **công khai** (có ô tìm theo tên). Chọn phòng → **Tham gia phòng đã chọn**.
- **Thành viên** (trong nhóm): xem danh sách, người online hiện màu xanh, có nút *Nhắn riêng*.
- **Rời nhóm**: rời khỏi nhóm (không áp dụng cho chat riêng).

### 7.6 Kết bạn
Bấm **Bạn bè**, cửa sổ có 3 tab:

| Tab | Chức năng |
|---|---|
| **Bạn bè (n)** | Danh sách bạn kèm trạng thái online. **Nhắn tin** (hoặc nhấp đúp), **Hủy kết bạn** |
| **Lời mời (n)** | *Lời mời đã nhận*: **Chấp nhận / Từ chối**. *Lời mời đã gửi*: **Thu hồi lời mời** |
| **Thêm bạn** | Gõ tên để tìm; cột *Quan hệ* cho biết "Chưa kết bạn / Bạn bè / Đã gửi lời mời / Đã nhận lời mời". Chọn người chưa kết bạn → **Gửi lời mời kết bạn** |

Nút *Bạn bè* ở cột trái hiện số lời mời chưa xử lý, ví dụ **Bạn bè (2)**. Người nhận lời mời được báo ngay nếu đang online. Phần mềm chặn: tự kết bạn với mình, gửi lời mời trùng, gửi cho người đã gửi lời mời cho bạn.

### 7.7 Tài khoản
Bấm **Tài khoản**:
- **Sửa hồ sơ**: đổi tên đăng nhập, họ tên, email, số điện thoại. Đổi tên đăng nhập không làm bạn bị đăng xuất; bạn bè và người trong cùng phòng thấy tên mới ngay.
- **Đổi mật khẩu**: nhập mật khẩu hiện tại, mật khẩu mới (2 lần).
- **Chuyển sang giao diện tối / sáng**.
- **Đăng xuất**: quay về màn hình đăng nhập.

---

## 8. Giao diện sáng/tối, ảnh đại diện, logo

### Giao diện sáng / tối
- Đổi ở: màn hình đăng nhập (nút cạnh ô cổng), máy người dùng (*Tài khoản*), máy host (góc dưới thanh bên).
- Đổi **ngay lập tức** trên mọi cửa sổ đang mở. Lựa chọn lưu bằng `java.util.prefs.Preferences` (trên Windows nằm trong registry của người dùng hiện tại) nên lần sau mở lại vẫn giữ nguyên.
- Muốn đổi màu chủ đạo: sửa `LIGHT` / `DARK` (hai bảng màu) và các hằng `PRIMARY`, `DANGER`, `SUCCESS` ở đầu `Theme.java`.

### Ảnh đại diện cá nhân
- Bấm vào **avatar ở góc trên bên trái** → *Đổi ảnh đại diện...* hoặc *Đặt lại ảnh mặc định*.
- Chọn file `png / jpg / jpeg / gif / bmp`; phần mềm tự **cắt vuông ở giữa**, thu nhỏ còn **128×128**, nén JPEG rồi hiển thị hộp **xem trước hình tròn** để bạn xác nhận.
- Ảnh lưu trong cột `users.avatar` (MEDIUMBLOB), tự gửi tới mọi máy đang online. Khi đăng nhập, máy người dùng nhận ảnh của tối đa 300 người có ảnh tùy chỉnh.
- Người chưa đặt ảnh hiển thị chữ cái đầu trên nền màu. Host có thể **Reset avatar** của bất kỳ ai.

### Ảnh nhóm
- Mở nhóm → bấm vào **avatar ở thanh tiêu đề** → *Đổi ảnh nhóm...* / *Đặt lại ảnh nhóm mặc định*.
- Chỉ **chủ nhóm hoặc quản trị viên** (`role` = `owner` / `admin` trong `room_members`) mới đổi được. Chat riêng 1-1 không có ảnh nhóm (dùng avatar của người kia).
- Ảnh lưu trong cột `chat_rooms.avatar`. Host: mục *Phòng chat* → **Reset avatar nhóm**.

### Logo / icon của ứng dụng
- Hiển thị ở: icon cửa sổ + thanh tác vụ (cả 3 cửa sổ), đầu màn hình đăng nhập, đầu thanh bên máy host.
- Mặc định là biểu tượng bong bóng chat vẽ bằng code. **Muốn dùng ảnh riêng**: đặt file tên đúng là **`logo.png`** vào:
  - **cạnh file `.jar`** (cùng thư mục với `ChatServer.jar` / `ChatClient.jar`) – áp dụng ngay lần mở sau, **không cần build lại**; hoặc
  - thư mục `src` của project NetBeans rồi *Clean and Build*.
  
  Nên dùng ảnh **vuông, PNG, từ 128×128 trở lên**. File `logo.png` đi kèm chính là ảnh mặc định. (Thứ tự ưu tiên: file `logo.png` ở thư mục chạy → `logo.png` bên trong `.jar`/`src` → hình vẽ sẵn.)

---

## 9. Chat giữa nhiều máy (LAN / Internet)

### Trong mạng LAN / Wi-Fi
1. Máy host chạy `ServerApp` và bấm *Khởi động server*. Xem IP máy host ở dòng *"Máy khác kết nối tới IP ..."* trên trang Tổng quan. Nếu máy có nhiều card mạng / VPN và IP hiển thị không đúng, mở **cmd** gõ `ipconfig` và lấy **IPv4 Address** của card Wi-Fi/Ethernet.
2. Trên máy người dùng, ở màn hình đăng nhập điền *Máy chủ* = IP đó, cổng `5000`.
3. Nếu **không kết nối được** (báo *Connection refused / timed out*), nhiều khả năng **tường lửa** đang chặn cổng. Trên máy host mở **cmd với quyền Administrator** và chạy:

   ```
   netsh advfirewall firewall add rule name="ChatNow" dir=in action=allow protocol=TCP localport=5000
   ```
   (hoặc *Windows Defender Firewall → Advanced settings → Inbound Rules → New Rule → Port → TCP 5000 → Allow*).
4. Chỉ cần **máy host** kết nối được MySQL; máy người dùng **không cần** cài MySQL hay thư viện JDBC khi chạy bản đóng gói (nhưng project NetBeans chung vẫn cần để biên dịch).

### Qua Internet
Máy người dùng phải tới được máy host qua cổng 5000. Các cách thường dùng:
- **Port-forward** cổng 5000 trên router về máy host, rồi dùng IP công cộng (hoặc tên miền DDNS).
- Đặt `ServerApp` trên một **VPS**.
- Dùng dịch vụ **tunnel TCP** (ví dụ ngrok) hoặc **VPN** cùng mạng ảo.

Lưu ý: đường truyền socket **chưa được mã hóa** (xem [mục 14](#14-bảo-mật-và-giới-hạn)).

---

## 10. Cơ sở dữ liệu

Database **`chat_db`** (MySQL/MariaDB, `utf8mb4`), 8 bảng:

| Bảng | Mục đích | Dùng trong code? |
|---|---|---|
| `users` | Tài khoản: `username` (UNIQUE), `password_hash` (SHA-256), `full_name`, `email` (UNIQUE), `phone`, `avatar`, `status`, `is_active`, `created_at`, `last_login` | Có |
| `chat_rooms` | Phòng chat: `room_name`, `description`, `avatar`, `room_type` (`public` / `private` / `direct`), `created_by` | Có |
| `room_members` | Thành viên phòng (quan hệ N-N): khóa chính (`room_id`, `user_id`), `role` (`owner` / `admin` / `member`), `joined_at` | Có |
| `messages` | Tin nhắn: `room_id`, `sender_id`, `content`, `reply_to_id`, `sent_at`, `edited_at`, `is_deleted` | Có |
| `friendships` | Kết bạn: `user_id` (người gửi lời mời), `friend_id` (người nhận), `status` (`pending` / `accepted` / `blocked`) | Có |
| `login_history` | Ghi lại mỗi lần đăng nhập/đăng xuất (IP, thời gian) | Có (ghi) |
| `message_status` | Trạng thái đã đọc từng tin | Chưa dùng |
| `attachments` | File đính kèm tin nhắn | Chưa dùng |

Các quan hệ chính:
- `users` ↔ `chat_rooms`: N-N qua `room_members`.
- `chat_rooms` → `messages`: 1-N, xóa phòng thì xóa tin nhắn (`ON DELETE CASCADE`).
- `users` → `messages`: 1-N, **không cho xóa hẳn** người đã có tin nhắn (`ON DELETE RESTRICT`) – vì vậy host chuyển sang *khóa* tài khoản.
- `users` ↔ `users`: N-N qua `friendships`.
- **Chat riêng 1-1** được lưu như một phòng `room_type = 'direct'` có đúng 2 thành viên → dùng chung bảng `messages` với chat nhóm.
- Xóa tin nhắn phía người dùng là **xóa mềm** (`is_deleted = 1`); xóa phía host là **xóa cứng**.

Ràng buộc (`CHECK`, `UNIQUE`, khóa ngoại) có trong `chat_db.sql`, ví dụ: tên đăng nhập đúng định dạng, không tự kết bạn với chính mình, `file_size > 0`, tin `text` phải có nội dung.

**Tự nâng cấp CSDL**: nếu bạn dùng CSDL tạo từ bản `chat_db.sql` cũ (chưa có cột `avatar`), khi host kết nối CSDL lần đầu phần mềm tự chạy `ALTER TABLE` thêm cột `users.avatar` và `chat_rooms.avatar`. Không cần nạp lại script.

**Sao lưu**: phpMyAdmin → chọn `chat_db` → tab **Export** → *Go*.

---

## 11. Giao thức giữa client và server

Văn bản thuần, mỗi lệnh **một dòng** (UTF-8), các trường ngăn cách bằng `|`, trường cuối là văn bản tự do. Vì vậy mật khẩu, họ tên không được chứa `|`.

**Client → Server**

| Lệnh | Ý nghĩa |
|---|---|
| `LOGIN\|user\|pass` · `REGISTER\|user\|pass\|họ tên\|email\|sđt` | Đăng nhập / đăng ký (dòng đầu tiên của mỗi kết nối) |
| `ROOMS` · `HISTORY\|roomId` | Danh sách phòng · 200 tin gần nhất của phòng |
| `MSG\|roomId\|nội dung` · `EDIT\|msgId\|nội dung` · `DEL\|msgId` | Gửi · sửa · xóa tin nhắn |
| `SEARCH\|roomId(0=tất cả)\|từ khóa` | Tìm tin nhắn |
| `NEWROOM\|public/private\|tên\|mô tả\|u1,u2` · `DM\|user` | Tạo nhóm · mở chat riêng |
| `FINDROOMS\|từ khóa` · `JOINROOM\|id` · `LEAVE\|id` · `MEMBERS\|id` · `ALLUSERS\|từ khóa` | Khám phá, tham gia, rời phòng, thành viên, tìm người |
| `PROFILE\|username\|họ tên\|email\|sđt` · `PASSWD\|cũ\|mới` | Sửa hồ sơ · đổi mật khẩu |
| `AVATAR\|base64` · `AVRESET` · `RAVATAR\|roomId\|base64` · `RAVRESET\|roomId` | Ảnh đại diện / ảnh nhóm |
| `FRIENDS` · `FSEARCH\|kw` · `FADD\|user` · `FACCEPT\|id` · `FDECLINE\|id` · `FCANCEL\|id` · `FREMOVE\|user` | Bạn bè |
| `QUIT` | Thoát |

**Server → Client** (chính): `OK`, `ERR|lý do`, `INFO|…`, `NOTI|…`, `SYS|…`, `ROOM|id|type|tên`, `MSG|id|roomId|người gửi|giờ|đã sửa|nội dung`, `HIST|…`, `EDITED|…`, `DELETED|…`, `SRES|…`, `USERS|a,b,c`, `AV|user|base64`, `RAV|roomId|base64`, `FRIEND|…`, `FREQ|…`, `FSENT|…`, `OPEN|roomId`, `LEFT|roomId`… Danh sách đầy đủ nằm trong phần chú thích đầu file `ChatServer.java`.

Mỗi client được xử lý trên **một luồng riêng**; server giữ bảng `username → kết nối` của những người đang online và chỉ phát tin nhắn cho thành viên của phòng tương ứng.

---

## 12. Chạy không cần NetBeans (file ứng dụng .jar / .exe)

Thư mục **`ChatNow-Apps/`** chứa sẵn hai ứng dụng đã biên dịch – chỉ cần cài **Java (JRE/JDK 8 trở lên)**, không cần NetBeans, không cần mở project.

| File | Dùng để |
|---|---|
| `ChatServer.jar` | **Máy host**: nhấp đúp để mở (tương đương chạy `ServerApp`) |
| `ChatClient.jar` | **Máy người dùng**: nhấp đúp để mở (tương đương chạy `LoginFrame`) |
| `Chay-Server.bat`, `Chay-Client.bat` | Mở nhanh bằng nhấp đúp; **tự tìm Java** dù chưa có trong PATH, báo lỗi dễ hiểu nếu máy chưa cài Java hoặc host thiếu thư viện MySQL |
| `lib/` (hoặc để cạnh `ChatServer.jar`) | Nơi đặt file driver MySQL (chỉ máy host cần) |
| `Tao-EXE.bat` | Tạo `ChatServer.exe` và `ChatClient.exe` (tùy chọn) |
| `logo.ico`, `logo.png` | Icon cho file `.exe` và logo ứng dụng |

### 12.1 Mở máy host (ChatServer.jar)
1. Cài Java nếu chưa có (kiểm tra bằng cách mở **cmd** gõ `java -version`).
2. Driver MySQL **đã kèm sẵn** tại `ChatNow-Apps/lib/mysql-connector-java-5.1.49.jar` – không cần làm gì thêm. Chương trình **tự tìm và nạp** file `.jar` trong thư mục `lib` hoặc cạnh `ChatServer.jar`. Nếu bạn lỡ xóa file này, chép lại một file `mysql-connector-java` vào một trong hai nơi đó (tải ở mục 4, bước 2).
   > Nếu bạn tải từng file về nên **không thấy thư mục `lib`** thì không sao: `lib` chỉ là một thư mục (không phải file). Cứ để `mysql-connector-java-5.1.49.jar` nằm cạnh `ChatServer.jar`, hoặc tự tạo thư mục tên `lib` rồi bỏ file vào.
3. Bật **MySQL** trong XAMPP (và đã nạp `chat_db.sql`, mục 4).
4. **Nhấp đúp `ChatServer.jar`** (hoặc `Chay-Server.bat`) → cửa sổ Chat Host mở ra → *Kiểm tra CSDL* → *Khởi động server*.

### 12.2 Mở máy người dùng (ChatClient.jar)
- **Nhấp đúp `ChatClient.jar`** (hoặc `Chay-Client.bat`) → màn hình đăng nhập.
- Máy người dùng **chỉ cần file `ChatClient.jar` + Java**, không cần MySQL, không cần thư mục `lib`. Bạn có thể chép riêng file này sang các máy khác trong lớp.
- Nhập IP máy host vào ô *Máy chủ* (mục 9).

### 12.3 Nếu nhấp đúp file .jar không mở
- Máy chưa cài Java → cài JRE/JDK (Java 8 trở lên). Hoặc mở bằng `Chay-Server.bat` / `Chay-Client.bat`: hai file này **tự tìm Java** (trong `PATH`, biến `JAVA_HOME`, rồi các thư mục cài phổ biến như `C:\Program Files\Java`, `Eclipse Adoptium`, `Microsoft`, `Amazon Corretto`, `Zulu`...) nên **không cần chỉnh PATH**; nếu không thấy Java sẽ hướng dẫn cài.
- File `.jar` đang được mở bằng chương trình khác (WinRAR, 7-Zip...) → chuột phải → *Open with* → *Java(TM) Platform SE binary*.
- Chạy bằng dòng lệnh để xem lỗi chi tiết: `java -jar ChatServer.jar` hoặc `java -jar ChatClient.jar`.

### 12.4 Tạo file .exe (tùy chọn – máy khác không cần cài Java)
Cần **JDK 14 trở lên** (có lệnh `jpackage`; JDK 8 và 11 **không có**, JDK 17 và 21 đều dùng tốt) trên máy build, không cần cài thêm gì khác. Script tự dò `jpackage` trong `PATH`, biến `JAVA_HOME` và các thư mục cài JDK phổ biến (`C:\Program Files\Java`, `Eclipse Adoptium`, `Microsoft`, `Amazon Corretto`...). Nếu báo *"Khong tim thay jpackage"*, hãy cài **JDK 17** (ví dụ Temurin tại https://adoptium.net) rồi chạy lại:
1. Chép driver MySQL vào `ChatNow-Apps/lib/` (như 12.1).
2. Nhấp đúp **`Tao-EXE.bat`**, đợi vài phút.
3. Kết quả nằm trong thư mục `ChatNow-Apps/exe/`:
   ```
   exe/ChatServer/ChatServer.exe     ← máy host
   exe/ChatClient/ChatClient.exe     ← máy người dùng
   ```
   Mỗi thư mục đã **kèm sẵn Java** (nặng khoảng vài chục MB), chép cả thư mục sang máy khác là chạy được, kể cả máy chưa cài Java. Icon của file `.exe` lấy từ `logo.ico`.
4. Với `ChatServer.exe`, nếu muốn đổi driver MySQL thì đặt file `.jar` vào `exe/ChatServer/app/lib/`.

### 12.5 Khi bạn sửa code
Hai file `.jar` trong `ChatNow-Apps/` được build từ đúng bộ code kèm theo. Nếu bạn **sửa code** trong NetBeans, hãy tạo lại bản chạy:
1. **Run → Clean and Build Project**. NetBeans tạo thư mục `dist/` gồm `ChatApp.jar` và `lib/mysql-connector-java-....jar`.
2. Chạy **máy host**: `java -cp "dist\ChatApp.jar;dist\lib\*" ServerApp`
3. Chạy **máy người dùng**: `java -cp "dist\ChatApp.jar;dist\lib\*" LoginFrame`  
   (Linux/macOS: thay `;` bằng `:` và `\` bằng `/`.)
4. Có thể chép nguyên thư mục `dist` sang máy khác. Muốn **nhấp đúp được `dist/ChatApp.jar`**: vào *Project Properties → Run → Main Class* chọn `ServerApp` (máy host) hoặc `LoginFrame` (máy người dùng), rồi Clean and Build lại; thư mục `lib` phải nằm cạnh `ChatApp.jar`.

---

## 13. Xử lý lỗi thường gặp

| Hiện tượng / thông báo | Nguyên nhân | Cách xử lý |
|---|---|---|
| `Uncompilable code - package Theme does not exist` | Thiếu file `Theme.java`, hoặc các file khác package | Đủ **7 file** `.java`, cùng package (xóa dòng `package ...;`), rồi *Clean and Build* |
| `Không kết nối được máy chủ: Connection refused` | Chưa bật server, sai IP/cổng, hoặc tường lửa chặn | Chạy `ServerApp` → *Khởi động server*; kiểm tra IP/cổng; mở cổng 5000 (mục 9) |
| `No suitable driver` / "Chưa thêm thư viện MySQL JDBC Driver" | Chưa thêm file Connector/J vào *Libraries* | *Libraries → Add JAR/Folder...* chọn file `.jar` (mục 4, bước 2–3) |
| Chạy bằng `.jar`: "Chưa có MySQL JDBC Driver..." | Thiếu file driver cạnh `ChatServer.jar` (hoặc trong thư mục `lib`) | Kiểm tra file `mysql-connector-java-5.1.49.jar` còn trong `ChatNow-Apps/lib/` (hoặc cạnh `ChatServer.jar`), chép lại nếu bị xóa (mục 12.1) rồi mở lại |
| `Tao-EXE.bat` báo "Khong tim thay jpackage" | Máy chưa có JDK 14+ (JDK 8/11 không có `jpackage`) hoặc JDK không nằm ở thư mục quen thuộc | Cài JDK 17 (adoptium.net) rồi chạy lại; hoặc mở CMD tại thư mục `ChatNow-Apps`, gõ `set "PATH=<thư mục JDK>\bin;%PATH%"` rồi `Tao-EXE.bat`. Hai file `.jar` vẫn dùng bình thường, `.exe` chỉ là tùy chọn |
| Nhấp đúp `.jar` không có phản ứng gì | Chưa cài Java, hoặc `.jar` đang gắn với chương trình khác | Mục 12.3: cài Java, *Open with → Java(TM) Platform SE binary*, hoặc chạy `java -jar ...` để xem lỗi |
| `Communications link failure` | MySQL chưa Start hoặc sai cổng | Start MySQL trong XAMPP; sửa cổng trong chuỗi kết nối nếu khác 3306 |
| `Access denied for user 'root'` | Sai mật khẩu MySQL | Điền đúng mật khẩu ở trang *Tổng quan* (XAMPP mặc định để trống) |
| `Unknown database 'chat_db'` | Chưa nạp `chat_db.sql` | Import `chat_db.sql` bằng phpMyAdmin (mục 4, bước 1) |
| `Unknown system variable 'transaction_isolation'` | Dùng Connector/J 8.x với MariaDB đời cũ (XAMPP) | Đổi sang **mysql-connector-java 5.1.49** |
| `Address already in use` / "Không mở được cổng" | Cổng 5000 đã có chương trình khác dùng, hoặc đã chạy host 2 lần | Tắt host cũ hoặc đổi cổng ở *Tổng quan* (và ô cổng bên client) |
| "Tài khoản đang đăng nhập ở nơi khác" | Tài khoản đó đã online (kể cả cửa sổ khác của bạn) | Đăng xuất cửa sổ kia, hoặc dùng tài khoản khác |
| "Sai tên đăng nhập hoặc mật khẩu" | Nhập sai, hoặc dữ liệu mẫu chưa được nạp | Thử `an` / `123456`; kiểm tra bảng `users` |
| "Tài khoản đã bị khóa" | Host đã khóa tài khoản | Host: *Người dùng → Khóa / Mở khóa* |
| Chữ tiếng Việt / emoji bị lỗi, hiện ô vuông | Project chưa để encoding UTF-8, hoặc font thiếu ký tự | *Properties → Sources → Encoding = UTF-8*, Clean and Build; không dùng ký tự lạ trong tên nút |
| Thêm file mới nhưng chương trình vẫn chạy bản cũ | NetBeans còn dùng file đã biên dịch cũ | *Clean and Build*, tắt hẳn cửa sổ cũ rồi chạy lại |
| Sửa code `ChatServer`/`Dao` nhưng không thấy tác dụng | Đang chạy server bản cũ | **Tắt và khởi động lại `ServerApp`** sau mỗi lần cập nhật code phía server |
| Ảnh đại diện không hiện ở máy khác | Máy khác chưa nhận được ảnh (mới đăng nhập) hoặc CSDL thiếu cột `avatar` | Đăng nhập lại; đảm bảo host đã "Kiểm tra CSDL" thành công (tự thêm cột) |

Cách chẩn đoán nhanh: xem **Nhật ký hoạt động** ở trang Tổng quan của host, và cửa sổ *Output* của NetBeans.

---

## 14. Bảo mật và giới hạn

- Mật khẩu được lưu dạng **SHA-256** (không lưu mật khẩu thô). Bản này **chưa dùng salt**; ứng dụng thực tế nên dùng **bcrypt / Argon2**.
- Đường truyền socket **không mã hóa** (mật khẩu gửi lên server dạng văn bản trong mạng). Phù hợp bài tập và mạng LAN tin cậy; muốn dùng thật cần TLS (`SSLServerSocket`) hoặc đặt sau VPN.
- Tài khoản MySQL `root` không mật khẩu chỉ nên dùng khi học tập. Hãy tạo user riêng cho `chat_db` nếu triển khai thật.
- Mỗi lần thao tác, server mở một kết nối JDBC mới (đơn giản, đủ cho lớp học); chưa có connection pool.
- Mỗi client dùng một luồng riêng → phù hợp vài chục người dùng đồng thời.
- Khi mở một phòng chỉ tải **200 tin nhắn gần nhất**; chưa có phân trang xem tin cũ hơn.
- Ảnh đại diện nhận tối đa khoảng 400.000 ký tự base64 và luôn được server chuẩn hóa về 128×128 JPEG. Chưa hỗ trợ gửi file/ảnh trong khung chat (bảng `attachments` đã sẵn cho việc này).
- Trạng thái "đã đọc" từng tin chưa được dùng (bảng `message_status` đã sẵn).

---

## 15. Hướng phát triển thêm

- Gửi **ảnh / file** trong chat (dùng bảng `attachments`).
- Trạng thái **đã xem**, **đang nhập...**, trả lời tin nhắn (cột `reply_to_id` đã có).
- Cho phép **chỉ bạn bè mới nhắn riêng**, chặn người dùng (`friendships.status = 'blocked'`).
- **Mã hóa TLS**, băm mật khẩu bcrypt, connection pool.
- Phân quyền quản trị viên nhóm (đổi vai trò `member` ↔ `admin`), xóa tin của người khác trong nhóm.
- Gọi thoại/video, thông báo đẩy, bản chạy trên Android.

---

*Chúc bạn chạy thành công! Nếu gặp lỗi, hãy xem lại [mục 13](#13-xử-lý-lỗi-thường-gặp) trước, rồi gửi kèm thông báo lỗi trong cửa sổ Output của NetBeans.*
