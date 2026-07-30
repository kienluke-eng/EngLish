# EnglishTHPT — bản đóng gói Android (APK)

Thư mục này chứa toàn bộ app **EnglishTHPT** (PWA gốc, nay đã thêm tính năng **Nhắc học**),
được bọc bằng [Capacitor](https://capacitorjs.com) thành một dự án Android. Bạn không cần
cài Android Studio — chỉ cần đẩy thư mục này lên GitHub, **GitHub Actions sẽ tự build ra
file `.apk`** để tải về điện thoại.

## 1. Tính năng mới: Nhắc học ⏰

Trong app, vào menu **⏰ Nhắc học** (thanh điều hướng trên cùng), bạn có thể cài:

- **Bật/tắt** nhắc học.
- **Giờ bắt đầu nhắc** trong ngày (ví dụ 19:00).
- **Những ngày trong tuần** sẽ nhắc (chọn T2–CN tuỳ ý).
- **Số phút cần học mỗi ngày** (mục tiêu, ví dụ 30 phút).
- **Khoảng cách nhắc lại** nếu chưa học đủ (mặc định 60 phút).

Cơ chế hoạt động: đến giờ đã đặt, nếu hôm đó là ngày được chọn, app sẽ báo. Nếu lúc đó bạn
**chưa học đủ** số phút đã cài, app sẽ **báo lại mỗi 60 phút** (hoặc số phút bạn chọn) cho
đến khi đủ giờ học trong ngày hoặc hết ngày thì thôi. Khi đã học đủ, các lần nhắc còn lại
trong ngày tự động huỷ.

> Lưu ý kỹ thuật: thông báo **đầu tiên** trong ngày luôn đến đúng giờ, kể cả khi bạn không mở
> app (đây là thông báo hẹn giờ ở cấp hệ điều hành). Các lần **nhắc lại** trong cùng ngày được
> app tính toán lại mỗi khi bạn mở app (kể cả khi bạn chỉ mở app từ chính thông báo đầu tiên) —
> đây là cách duy nhất để app biết bạn đã học đủ hay chưa mà không cần máy chủ. Nếu dùng app
> qua trình duyệt (chưa cài .apk), thông báo chỉ hoạt động khi app đang mở.

## 2. Đưa code lên GitHub

### Cách A — dùng Git (khuyên dùng, nhanh và chắc chắn nhất)

Mở terminal tại đúng thư mục này rồi chạy:

```bash
git init
git add .
git commit -m "EnglishTHPT: them tinh nang Nhac hoc + dong goi Android"
```

Sau đó vào [github.com/new](https://github.com/new), tạo một **repository mới** (để trống,
**không** tick "Add a README file"). GitHub sẽ cho bạn 2-3 dòng lệnh, đại loại:

```bash
git remote add origin https://github.com/<ten-cua-ban>/<ten-repo>.git
git branch -M main
git push -u origin main
```

### Cách B — kéo-thả trên trình duyệt

Nếu không quen dùng Git, bạn có thể vào repo mới tạo → **Add file → Upload files** → kéo
toàn bộ nội dung thư mục này vào. Cách này vẫn hoạt động, nhưng vì có khá nhiều file con
(trong `android/`), dùng Git ở Cách A sẽ nhanh và ít lỗi hơn.

## 3. Lấy file .apk

1. Sau khi push code, vào tab **Actions** trên GitHub repo — sẽ thấy workflow
   **"Build Android APK"** đang chạy (mất khoảng 2–4 phút).
2. Khi chạy xong (dấu ✔ xanh), có **2 cách tải APK**:
   - **Dễ nhất trên điện thoại:** vào tab **Releases** (cột phải trang chủ repo, hoặc
     `github.com/<ten>/<repo>/releases`) → bấm vào file `app-debug.apk` để tải trực tiếp.
   - **Trên máy tính:** vào tab **Actions** → chọn lần chạy vừa xong → mục **Artifacts** →
     tải `EnglishTHPT-debug-apk.zip` → giải nén lấy file `.apk` → chuyển sang điện thoại.

Mỗi lần bạn sửa code và `git push` lên nhánh `main`, workflow sẽ tự chạy lại và tạo bản
APK mới (kèm Release mới).

## 4. Cài đặt vào điện thoại Android

1. Mở file `.apk` vừa tải (trực tiếp trong Chrome trên điện thoại, hoặc qua ứng dụng Quản lý
   file nếu bạn tải trên máy tính rồi chuyển qua).
2. Android sẽ hỏi cho phép **"Cài đặt ứng dụng không rõ nguồn gốc" (Install unknown apps)**
   — bấm **Cho phép/Cài đặt** (chỉ cần làm 1 lần cho Chrome hoặc app Quản lý file).
3. Bấm **Cài đặt (Install)** → **Mở (Open)**.
4. Khi vào phần **⏰ Nhắc học** và bật tính năng, app sẽ xin **quyền Thông báo** — bấm
   **Cho phép** để nhắc học hoạt động được.

Đây là bản build **debug** (ký bằng khoá debug mặc định của Android) — dùng để cài trực tiếp
lên điện thoại cá nhân là hoàn toàn ổn định. Các lần build sau (khi bạn sửa code) đều dùng
chung khoá debug này nên cài đè lên bản cũ sẽ không bị lỗi "chữ ký không khớp". Nếu sau này
muốn phát hành chính thức lên Google Play, cần build bản "release" có khoá ký riêng (có thể
làm thêm khi cần).

## 5. Cấu trúc dự án

```
├── www/                     PWA gốc (index.html, sw.js, manifest, icons) — mở trực tiếp
│                            bằng trình duyệt cũng chạy được như trước
├── android/                 Dự án Android do Capacitor tạo (đừng chỉnh tay trừ khi cần)
├── resources/               Ảnh icon.png / splash.png gốc, dùng để tạo lại icon app nếu cần:
│                            npx capacitor-assets generate --android
├── capacitor.config.json    Cấu hình Capacitor (tên app, App ID, plugin)
├── package.json             Khai báo các gói Capacitor cần cài
└── .github/workflows/       Workflow GitHub Actions build file .apk tự động
```

## 6. Muốn sửa nội dung app?

Toàn bộ nội dung, giao diện và logic app nằm trong **`www/index.html`** — sửa file này rồi
`git add . && git commit -m "..." && git push` là GitHub sẽ tự build lại APK mới.
