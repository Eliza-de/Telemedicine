# GitHub Pages Setup - Vibharam Hospital

Wrapper สำหรับ Apps Script Web Apps 2 ระบบ:
- ระบบ A: ลงทะเบียนผู้ป่วย (Patient Registration)
- ระบบ B: เภสัช (Tele-Pharmacy)

---

## โครงสร้างไฟล์

```
Telemedicine/                    ← GitHub repo
├── index.html                   ← หน้า Landing (เลือก 2 ระบบ)
├── patient.html                 ← Wrapper ระบบ A
└── pharmacy.html                ← Wrapper ระบบ B
```

User flow:
```
https://eliza-de.github.io/Telemedicine/
                  ↓
        [Landing — 2 cards]
        ┌─────────┬─────────┐
        │ 📋 Patient │ 💊 Pharmacy │
        └─────────┴─────────┘
            ↓           ↓
       /patient.html  /pharmacy.html
            ↓           ↓
     [iframe → ระบบ A]  [iframe → ระบบ B]
```

---

## 🚀 Setup ทีละขั้น

### Step 1: ลบไฟล์เก่าใน GitHub repo

ใน `eliza-de/Telemedicine` — ลบทุกไฟล์ที่ไม่ใช่ 3 ไฟล์ใหม่:

```
ลบ:
❌ Index.html       (ของ Apps Script)
❌ Style.html
❌ Script.html
❌ Code.gs
❌ Auth.gs
❌ Setup.gs
❌ ไฟล์อื่น ๆ

เหลือ (ถ้ามี):
✅ README.md
```

⚠️ **ไฟล์ Apps Script ทั้งหมดต้องอยู่ใน Apps Script editor เท่านั้น** ไม่ใช่ใน GitHub

### Step 2: Upload 3 ไฟล์ใหม่

Upload จาก folder `github-pages/` ลง root ของ repo:
- `index.html`
- `patient.html`
- `pharmacy.html`

### Step 3: แก้ URL ใน 2 ไฟล์

#### A. patient.html

หา line นี้:
```html
src="https://script.google.com/macros/s/PASTE_PATIENT_DEPLOYMENT_ID/exec"
```

แทนที่ด้วย URL ของระบบ A (Patient Registration):
```html
src="https://script.google.com/macros/s/AKfycbXXXXXXX.../exec"
```

#### B. pharmacy.html

หา line นี้:
```html
src="https://script.google.com/macros/s/PASTE_PHARMACY_DEPLOYMENT_ID/exec"
```

แทนที่ด้วย URL ของระบบ B (Pharmacy):
```html
src="https://script.google.com/macros/s/AKfycbYYYYYYY.../exec"
```

### Step 4: Push to GitHub

```bash
git add index.html patient.html pharmacy.html
git rm Index.html Style.html Script.html Code.gs Auth.gs Setup.gs
git commit -m "Setup wrapper for Patient + Pharmacy systems"
git push
```

หรือทำผ่านเว็บ GitHub โดยตรง (Add files → Upload files)

### Step 5: รอ GitHub Pages deploy (1-2 นาที)

เปิด: `https://eliza-de.github.io/Telemedicine/`

---

## วิธีหา Web App URL

### ระบบ A (Patient)
1. เปิด Apps Script editor ของ **ระบบ A**
2. **Deploy** → **Manage deployments**
3. คลิก deployment ที่ active
4. Copy **Web app URL**
5. paste ลง `patient.html`

### ระบบ B (Pharmacy)
1. เปิด Apps Script editor ของ **ระบบ B**
2. **Deploy** → **Manage deployments**
3. คลิก deployment ที่ active
4. Copy **Web app URL**
5. paste ลง `pharmacy.html`

---

## Features

### 🎨 Landing Page (index.html)
- Gradient background สีน้ำเงิน → ม่วง
- 2 cards เด่น: Patient (น้ำเงิน) + Pharmacy (ม่วง)
- Mobile responsive
- คลิก card → ไปหน้าระบบ

### 📋 Patient Wrapper (patient.html)
- Loading screen สีน้ำเงิน
- ปุ่ม Back (←) มุมซ้ายบน → กลับหน้าหลัก
- Error fallback ถ้าโหลดไม่ได้
- PWA support (Add to Home Screen)
- Theme: `#1677ff` (น้ำเงิน Ant Design)

### 💊 Pharmacy Wrapper (pharmacy.html)
- Loading screen สีม่วง
- ปุ่ม Back (←) มุมซ้ายบน → กลับหน้าหลัก
- Error fallback ถ้าโหลดไม่ได้
- PWA support (Add to Home Screen)
- Theme: `#722ed1` (ม่วง Ant Design)

### 🔒 ทั้ง 2 wrapper
- ซ่อน banner "Google Apps Script user"
- iframe เต็มจอ (no scroll outside)
- Allow camera (สำหรับถ่ายรูปพัสดุในระบบ B)
- Timeout 30 วิ ถ้าโหลดไม่ขึ้น

---

## ทดสอบ

หลัง deploy ลองทำ:

| ขั้น | ทดสอบ | คาดหวัง |
|---|---|---|
| 1 | เปิด `eliza-de.github.io/Telemedicine/` | เห็น landing page 2 cards |
| 2 | คลิก card "ลงทะเบียนผู้ป่วย" | ไปหน้า patient.html → loading สีน้ำเงิน → ระบบ A เปิด |
| 3 | กด Back (←) | กลับ landing page |
| 4 | คลิก card "ระบบเภสัช" | ไปหน้า pharmacy.html → loading สีม่วง → ระบบ B เปิด |
| 5 | กด Back (←) | กลับ landing page |
| 6 | Banner "Google Apps Script user" | ✅ ไม่เห็น |
| 7 | Mobile (iPhone/Android) | Responsive, full-screen |

---

## ⚠️ ปัญหาที่เจอบ่อย

### "Sorry, unable to open the file"
User login Gmail หลายบัญชี → Apps Script สับสน

**แก้:**
- ใช้ Incognito mode
- หรือเพิ่ม `/u/0/` ใน URL:
  ```
  https://script.google.com/u/0/macros/s/AKfycb.../exec
                          ↑↑↑↑
  ```

### iframe โหลดไม่ขึ้น (ขาว)
Apps Script `setXFrameOptionsMode` ต้อง = `ALLOWALL`

ตรวจใน Code.gs:
```javascript
function doGet(e) {
  return HtmlService.createTemplateFromFile('Index')
    .evaluate()
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);  // ⭐
}
```

ระบบ A และ B ของคุณมีอยู่แล้ว ✅

### Loading ค้างเกิน 30 วิ
- ตรวจ URL Apps Script ถูกไหม
- ตรวจ Apps Script deployed แล้วยัง
- ตรวจ permission "ทุกคนที่มีบัญชี Google"

---

## 📱 Add to Home Screen

### iPhone (Safari)
1. เปิด `eliza-de.github.io/Telemedicine/patient.html` หรือ `pharmacy.html`
2. Share → Add to Home Screen
3. ตั้งชื่อ → Add

App จะเปิดเต็มจอ ไม่มี address bar — เหมือน native app

### Android (Chrome)
1. เปิด URL ใน Chrome
2. ⋮ → Install app
3. Add to home screen

---

## 🎯 ทำไมต้อง wrapper ที่ GitHub Pages?

| ใช้ Apps Script ตรง ๆ | ผ่าน GitHub Pages wrapper |
|---|---|
| ❌ มี banner "Google Apps Script user" | ✅ ไม่มี banner |
| ❌ URL ยาว `script.google.com/macros/...` | ✅ URL สั้น `eliza-de.github.io/Telemedicine/` |
| ❌ ใส่ใน LINE OA / QR ดูแย่ | ✅ ดูเป็นทางการ |
| ❌ Add to Home Screen ลำบาก | ✅ PWA-like behavior |
| ❌ เลือกระบบไม่ได้ | ✅ Landing page รวม 2 ระบบ |

---

## ขั้นตอนถัดไป

หลัง wrapper ใช้งานได้แล้ว Phase 2 features ที่ค้างอยู่:

- 🖨️ **ปริ้นสติ๊กเกอร์** Label printer (Xprinter, Argox)
- 📲 LINE notification เมื่อสถานะพัสดุเปลี่ยน
- 📊 Dashboard / Report
