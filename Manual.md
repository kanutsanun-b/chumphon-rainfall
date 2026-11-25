# RainfallChumphon: คู่มือออกเวอร์ชันและอัปเดต

คู่มือนี้ใช้สำหรับแก้โค้ด สร้าง EXE สร้างตัวติดตั้ง อัปโหลด GitHub Release และอัปเดต `version.json`

## ค่าโปรเจกต์

| รายการ | ค่า |
|---|---|
| GitHub owner | `kanutsanun-b` |
| GitHub repository | `chumphon-rainfall` |
| Update manifest | `https://raw.githubusercontent.com/kanutsanun-b/chumphon-rainfall/main/release/version.json` |
| โฟลเดอร์โปรเจกต์ | `D:\kanutsanunProj\chumphon_thaiwater` |
| โฟลเดอร์ปล่อยไฟล์ | `D:\kanutsanunProj\chumphon_thaiwater\release` |

---

## ภาพรวมการอัปเดต

ตัวอัปเดตของ RainfallChumphon ใช้ **ไฟล์ตัวติดตั้ง `.exe` ตัวเดียว**

ผู้ใช้กดปุ่ม **Update** แล้วโปรแกรมจะ:

1. อ่าน `release/version.json` จาก GitHub
2. เปรียบเทียบเวอร์ชันปัจจุบันกับเวอร์ชันล่าสุด
3. ดาวน์โหลดตัวติดตั้งรุ่นใหม่
4. ตรวจสอบ SHA-256
5. เปิดตัวติดตั้ง
6. ติดตั้งทับระบบเดิม

ไม่จำเป็นต้องอัปโหลด standalone `RainfallChumphon.exe` แยกลง GitHub Release

---

## เพิ่มเวอร์ชันใหม่

ใช้เวอร์ชันรูปแบบ `x.y.z` เช่น `1.3.0` → `1.4.0`

### 1. แก้เวอร์ชันในโปรแกรม

เปิดไฟล์:

```text
D:\kanutsanunProj\chumphon_thaiwater\rainfall_gui.py
```

แก้:

```python
APP_VERSION = "1.4.0"
```

### 2. แก้เวอร์ชันตัวติดตั้ง

เปิดไฟล์:

```text
D:\kanutsanunProj\chumphon_thaiwater\installer.iss
```

แก้:

```ini
#define AppVersion "1.4.0"
OutputBaseFilename=RainfallChumphon_Setup_1.4.0
```

ตัวติดตั้งที่ได้มีชื่อ:

```text
RainfallChumphon_Setup_1.4.0.exe
```

---

## สร้าง EXE และตัวติดตั้ง

### สร้าง standalone EXE

เปิด PowerShell หรือ Command Prompt ในโฟลเดอร์โปรเจกต์ แล้วรันคำสั่งที่บันทึกไว้ในไฟล์ build หรือคำสั่งหลักของ PyInstaller

ไฟล์ผลลัพธ์อยู่ที่:

```text
D:\kanutsanunProj\chumphon_thaiwater\dist\RainfallChumphon.exe
```

คัดลอกไฟล์นั้นไปไว้ที่โฟลเดอร์ `release` ก่อนสร้างตัวติดตั้ง

```text
D:\kanutsanunProj\chumphon_thaiwater\release\RainfallChumphon.exe
```

### สร้างตัวติดตั้ง

ดับเบิลคลิก:

```text
build_installer.bat
```

หรือรัน:

```powershell
& "C:\Program Files (x86)\Inno Setup 6\ISCC.exe" "D:\kanutsanunProj\chumphon_thaiwater\installer.iss"
```

ตัวติดตั้งจะอยู่ที่:

```text
D:\kanutsanunProj\chumphon_thaiwater\release\RainfallChumphon_Setup_1.4.0.exe
```

---

## คำนวณ SHA-256 ของตัวติดตั้ง

ใช้ตัวติดตั้งรุ่นใหม่ไม่ใช่ไฟล์ EXE standalone

```powershell
Get-FileHash `
  "D:\kanutsanunProj\chumphon_thaiwater\release\RainfallChumphon_Setup_1.4.0.exe" `
  -Algorithm SHA256
```

คัดลอกค่าในฟิลด์ `Hash` เช่น:

```text
D99C22CA86CE40B64BD1C154ADC795B8092BBEA759C739485285115216ED0258
```

ในการบันทึกใน `version.json` ใช้ตัวพิมพ์เล็กหรือพิมพ์ใหญ่ได้แต่ควรใช้ตัวพิมพ์เล็ก

---

## ไฟล์ version.json

ไฟล์ต้นฉบับอยู่ที่:

```text
D:\kanutsanunProj\chumphon_thaiwater\release\version.json
```

แก้เป็นแบบนี้สำหรับเวอร์ชัน 1.4.0:

```json
{
  "app": {
    "latest": "1.4.0",
    "url": "https://github.com/kanutsanun-b/chumphon-rainfall/releases/download/v1.4.0/RainfallChumphon_Setup_1.4.0.exe",
    "sha256": "ใส่ SHA-256 ของไฟล์ตัวติดตั้ง",
    "notes": "สรุปสิ่งที่เพิ่มหรือแก้ไข"
  }
}
```

ต้องแก้ให้ครบทุกฟิลด์:

| ฟิลด์ | ความหมาย |
|---|---|
| `latest` | เวอร์ชันล่าสุด |
| `url` | ลิงก์ดาวน์โหลดตัวติดตั้งรุ่นใหม่ |
| `sha256` | SHA-256 ของตัวติดตั้ง |
| `notes` | รายละเอียดที่ผู้ใช้เห็นตอน Update |

---

## สร้าง GitHub Release

1. เปิด GitHub repository `kanutsanun-b/chumphon-rainfall`
2. เลือก **Releases → Draft a new release**
3. ช่อง **Tag** ใส่ `v1.4.0`
4. ช่อง **Release title** ใส่ `v1.4.0`
5. แนบไฟล์:

```text
RainfallChumphon_Setup_1.4.0.exe
```

ตัวนี้ถูกวางที่กรอบ **Attach binaries** ด้านล่างช่องคำอธิบาย

6. กด **Publish release**
7. คัดลอกลิงก์ไฟล์:

```text
https://github.com/kanutsanun-b/chumphon-rainfall/releases/download/v1.4.0/RainfallChumphon_Setup_1.4.0.exe
```

---

## อัปโหลด version.json

หลัง Publish Release แล้ว ให้อัปโหลดไฟล์ `release/version.json` ไปที่ GitHub repository

ชื่อและตำแหน่งไฟล์:

```text
release/version.json
```

อัปโหลดโดย:

1. เปิด repository
2. กดโฟลเดอร์ `release`
3. เปิด `version.json`
4. กด **Edit**
5. วางเนื้อหาที่แก้เวอร์ชัน 1.4.0
6. กด **Commit changes**

ลิงก์ที่โปรแกรมอ่าน:

```text
https://raw.githubusercontent.com/kanutsanun-b/chumphon-rainfall/main/release/version.json
```

---

## ตรวจผลหลังอัปโหลด

### ตรวจ manifest

เปิดลิงก์:

```text
https://raw.githubusercontent.com/kanutsanun-b/chumphon-rainfall/main/release/version.json
```

ต้องเห็น JSON ของเวอร์ชันล่าสุด

### ตรวจตัวติดตั้ง

เปิดลิงก์จากฟิลด์ `url` ในเบราว์เซอร์ ต้องเริ่มดาวน์โหลดไฟล์:

```text
RainfallChumphon_Setup_1.4.0.exe
```

### ตรวจ SHA-256 อีกครั้ง

```powershell
Get-FileHash `
  "RainfallChumphon_Setup_1.4.0.exe" `
  -Algorithm SHA256
```

ค่า `Hash` ต้องตรงกับฟิลด์ `sha256` ใน `version.json`

---

## ทดสอบปุ่ม Update

1. ติดตั้งหรือเปิดโปรแกรมเวอร์ชันเก่า
2. กดปุ่ม **Update**
3. โปรแกรมต้องอ่าน `latest` ใหม่
4. ถ้า `latest` > `APP_VERSION` จะถามยืนยัน
5. ดาวน์โหลดตัวติดตั้ง
6. ตรวจ SHA-256
7. เปิดตัวติดตั้ง
8. ติดตั้งทับเวอร์ชันเก่า

หากแสดงว่าเป็นเวอร์ชันล่าสุด แสดงว่า:

- `latest` ยังไม่สูงกว่า `APP_VERSION`
- หรือ manifest ยังไม่อัปเดต

---

## การตรวจเวอร์ชันในโปรแกรม

ไฟล์:

```text
rainfall_gui.py
```

ต้องตรงกับตัวติดตั้งล่าสุด:

```python
APP_VERSION = "1.4.0"
```

ตัวควบคุมตรวจเวอร์ชัน:

```python
UPDATE_URL = "https://raw.githubusercontent.com/kanutsanun-b/chumphon-rainfall/main/release/version.json"
```

---

## ระวัง

1. ทุกเวอร์ชันต้องบวกเพิ่มเล็กน้อย เช่น `1.3.0` → `1.4.0`
2. Tag, ชื่อไฟล์ และ `latest` ต้องตรงกัน
3. `url` ต้องชี้ไปตัวติดตั้ง ไม่ชี้ไป standalone EXE
4. SHA-256 ต้องคำนวณจากไฟล์ตัวติดตั้งตัวที่แนบ Release
5. อัปโหลดไฟล์ตัวติดตั้งให้เสร็จก่อนค่อยอัปเดต `version.json`
6. GitHub Raw อาจใช้เวลาไม่กี่นาทีหลัง Commit เพื่ออัปเดตข้อมูล
7. ตัวติดตั้งควรเก็บไว้ด้วย ไม่ต้องอัปโหลด standalone EXE แยก
