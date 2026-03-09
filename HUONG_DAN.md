# 🎉 HƯỚNG DẪN DEPLOY — HẢI KADIMA BIRTHDAY PAGE

Stack: **Firebase Realtime Database** (lưu data) + **Vercel** (host web)
Tổng thời gian: ~15 phút | Hoàn toàn **miễn phí**

---

## BƯỚC 1 — Tạo Firebase Project (5 phút)

1. Vào **https://console.firebase.google.com**
2. Đăng nhập bằng Google account
3. Bấm **"Add project"** → đặt tên (vd: `hai-kadima-birthday`) → Next → Next → Create project
4. Sau khi tạo xong, bấm vào **"Realtime Database"** ở menu trái
5. Bấm **"Create Database"**
   - Chọn vị trí: `asia-southeast1` (Singapore, gần VN nhất)
   - Chọn **"Start in test mode"** → Enable
6. Copy **Database URL** hiển thị (dạng: `https://xxx-default-rtdb.asia-southeast1.firebasedatabase.app`)

---

## BƯỚC 2 — Lấy Firebase Config (3 phút)

1. Ở Firebase Console, bấm vào **⚙️ Settings** (bánh răng góc trái) → **Project settings**
2. Kéo xuống phần **"Your apps"** → bấm icon **`</>`** (Web)
3. Đặt nickname app (vd: `birthday-web`) → Register app
4. Bạn sẽ thấy đoạn code như thế này — **copy toàn bộ phần trong `firebaseConfig = { ... }`**:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "hai-kadima-birthday.firebaseapp.com",
  databaseURL: "https://hai-kadima-birthday-default-rtdb...",
  projectId: "hai-kadima-birthday",
  storageBucket: "hai-kadima-birthday.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

> Nếu sau đó Firebase hiện thêm bước **"Install Firebase CLI"** hoặc gợi ý setup Hosting bằng CLI, bạn có thể **bỏ qua** (không cần chạy) vì project này chỉ cần config để chạy file HTML trên Vercel.
>
> Tiếp theo: sang **BƯỚC 3 — Dán Config vào file HTML**.

---

## BƯỚC 3 — Dán Config vào file HTML (2 phút)

Mở file `index.html`, tìm đoạn này (khoảng dòng 15):

```js
// ⚠️  THAY THẾ BẰNG CONFIG CỦA BẠN TỪ FIREBASE CONSOLE
const firebaseConfig = {
  apiKey:            "YOUR_API_KEY",
  authDomain:        "YOUR_PROJECT.firebaseapp.com",
  ...
};
```

Xoá hết và **dán config của bạn vào**, ví dụ:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "hai-kadima-birthday.firebaseapp.com",
  databaseURL: "https://hai-kadima-birthday-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "hai-kadima-birthday",
  storageBucket: "hai-kadima-birthday.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

**Lưu file lại.**

---

## BƯỚC 4 — Deploy lên Vercel (5 phút)

### Cách 1: Kéo thả (dễ nhất, không cần code)

1. Vào **https://vercel.com** → đăng nhập / tạo tài khoản (dùng GitHub hoặc Google)
2. Bấm **"Add New..."** → **"Project"**
3. Chọn **"Import Third-Party Git Repository"** — hoặc dùng Vercel CLI

### Cách 2: Dùng Vercel CLI (nhanh hơn)

Cài Node.js nếu chưa có: https://nodejs.org

```bash
# Cài Vercel CLI
npm install -g vercel

# Vào thư mục chứa index.html
cd /đường/dẫn/tới/thư/mục

# Deploy
vercel

# Làm theo hướng dẫn:
# ? Set up and deploy "..."? → Y
# ? Which scope? → chọn account của bạn
# ? Link to existing project? → N
# ? What's your project's name? → hai-kadima-birthday
# ? In which directory is your code located? → ./
# ✅ Done! URL của bạn sẽ hiện ra ngay
```

### Cách 3: Drag & Drop (không cần CLI)

1. Vào **https://vercel.com/new**
2. Kéo thả **thư mục** chứa `index.html` vào trang
3. Vercel tự deploy — xong!

---

## BƯỚC 5 — Test và chia sẻ 🎉

1. Vercel sẽ cho bạn URL dạng: `https://hai-kadima-birthday.vercel.app`
2. Mở URL đó, thử submit một lần
3. Mở tab khác cũng vào URL đó — bạn sẽ thấy data cập nhật **realtime!**
4. Chia sẻ link cho bạn bè 🌼

---

## 🔒 Bảo mật (tuỳ chọn — nên làm trước ngày tiệc)

Firebase "test mode" hết hạn sau 30 ngày. Để dữ liệu tồn tại lâu hơn:

1. Firebase Console → **Realtime Database** → **Rules**
2. Thay bằng rules này:

```json
{
  "rules": {
    "guests": {
      ".read": true,
      ".write": true
    }
  }
}
```

3. Bấm **Publish**

---

## 📊 Xem dữ liệu

Vào **Firebase Console → Realtime Database** để xem toàn bộ danh sách điểm danh và lời chúc theo thời gian thực!

---

## ❓ Troubleshooting

| Lỗi | Cách fix |
|-----|---------|
| Trang load nhưng không lưu được | Kiểm tra lại `databaseURL` trong config |
| Lỗi "permission denied" | Kiểm tra Rules Firebase (xem bước Bảo mật) |
| Vercel báo lỗi | Đảm bảo chỉ có 1 file `index.html` trong thư mục |
| Data không realtime | Tắt ad-blocker, thử trình duyệt khác |

---

## 🗂️ Cấu trúc thư mục (chỉ cần 1 file!)

```
📁 hai-kadima-birthday/
└── 📄 index.html    ← Tất cả trong 1 file này!
```

Không cần `package.json`, không cần `node_modules`, không cần build step gì cả. **Một file HTML duy nhất!**

---

*Made with 🌼 for Hải Kadima · PEACEMINUSONE · 2025*
