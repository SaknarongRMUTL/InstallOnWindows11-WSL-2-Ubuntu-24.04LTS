<div align="center">

# Windows 11 + WSL 2 + Ubuntu 24.04 LTS

### คู่มือติดตั้งและเตรียมเครื่องสำหรับพัฒนาซอฟต์แวร์

![Windows 11](https://img.shields.io/badge/Windows-11-0078D4?logo=windows11&logoColor=white)
![WSL 2](https://img.shields.io/badge/WSL-2-4D4D4D?logo=linux&logoColor=white)
![Ubuntu 24.04 LTS](https://img.shields.io/badge/Ubuntu-24.04%20LTS-E95420?logo=ubuntu&logoColor=white)
![Guide](https://img.shields.io/badge/Guide-Step%20by%20step-2ea44f)

**คู่มือสำหรับติดตั้ง ตั้งค่า ตรวจสอบ และสำรองระบบ Ubuntu ที่ใช้งานผ่าน WSL 2**

</div>

---

## 📌 เกี่ยวกับคู่มือนี้

คู่มือนี้อธิบายการเตรียมเครื่อง **Windows 11** ให้ใช้งาน Linux ผ่าน **Windows Subsystem for Linux (WSL) 2** โดยเลือก **Ubuntu 24.04 LTS** เป็น Linux distribution สำหรับพัฒนาซอฟต์แวร์

แนวทางที่ใช้ในคู่มือนี้:

- ใช้ **PowerShell** บน Windows สำหรับติดตั้งและจัดการ WSL
- ใช้ **Ubuntu Terminal** สำหรับติดตั้งแพ็กเกจและรันคำสั่ง Linux
- เก็บโปรเจกต์ไว้ใน Linux filesystem เช่น `~/workspace` เมื่อต้องใช้เครื่องมือ Linux ทำงานกับไฟล์
- ติดตั้ง **Visual Studio Code บน Windows** แล้วเชื่อมต่อเข้ากับ Ubuntu ผ่านส่วนเสริม WSL

> ตรวจสอบให้แน่ใจว่ากำลังใช้หน้าต่าง Terminal ให้ถูกระบบ คำสั่ง PowerShell ต้องรันบน Windows ส่วนคำสั่ง Bash ต้องรันใน Ubuntu เว้นแต่จะระบุไว้เป็นอย่างอื่น

## 📚 สารบัญ

- [1. สิ่งที่ต้องเตรียม](#1-สิ่งที่ต้องเตรียม)
- [2. ตรวจสอบ Virtualization](#2-ตรวจสอบ-virtualization)
- [3. ติดตั้ง WSL 2 และ Ubuntu 24.04](#3-ติดตั้ง-wsl-2-และ-ubuntu-2404)
- [4. ตั้งค่าบัญชี Ubuntu ครั้งแรก](#4-ตั้งค่าบัญชี-ubuntu-ครั้งแรก)
- [5. อัปเดต Ubuntu และติดตั้งเครื่องมือพื้นฐาน](#5-อัปเดต-ubuntu-และติดตั้งเครื่องมือพื้นฐาน)
- [6. ตั้งค่า Git และ SSH Key สำหรับ GitHub](#6-ตั้งค่า-git-และ-ssh-key-สำหรับ-github)
- [7. ติดตั้ง Visual Studio Code และเชื่อมต่อ WSL](#7-ติดตั้ง-visual-studio-code-และเชื่อมต่อ-wsl)
- [8. สร้างโฟลเดอร์เก็บโปรเจกต์](#8-สร้างโฟลเดอร์เก็บโปรเจกต์)
- [9. คำสั่ง WSL ที่ใช้บ่อย](#9-คำสั่ง-wsl-ที่ใช้บ่อย)
- [10. คำสั่ง Ubuntu ที่ใช้บ่อย](#10-คำสั่ง-ubuntu-ที่ใช้บ่อย)
- [11. ตรวจสอบว่าติดตั้งครบแล้ว](#11-ตรวจสอบว่าติดตั้งครบแล้ว)
- [12. สำรองข้อมูลและกู้คืน Ubuntu](#12-สำรองข้อมูลและกู้คืน-ubuntu)
- [13. ปัญหาที่พบบ่อย](#13-ปัญหาที่พบบ่อย)
- [แหล่งข้อมูล](#แหล่งข้อมูล)

---

## 1. สิ่งที่ต้องเตรียม

| รายการ | รายละเอียด |
|---|---|
| ระบบปฏิบัติการ | Windows 11 ที่ได้รับการอัปเดต |
| Linux distribution | Ubuntu 24.04 LTS |
| ระบบ Linux บน Windows | Windows Subsystem for Linux (WSL) 2 |
| สิทธิ์ที่ใช้ติดตั้ง | บัญชี Windows ที่มีสิทธิ์ Administrator |
| อินเทอร์เน็ต | ใช้ดาวน์โหลด WSL, Ubuntu และแพ็กเกจ |
| Virtualization | ต้องเปิดใช้งานใน BIOS/UEFI และ Windows ต้องรองรับ Virtual Machine Platform |
| โปรแกรมเขียนโค้ด | Visual Studio Code (แนะนำ) |

**หมายเหตุ:** หากเครื่องมี WSL หรือ Ubuntu ติดตั้งอยู่แล้ว ไม่จำเป็นต้องติดตั้งใหม่ ให้ตรวจสอบสถานะก่อนตามหัวข้อ 3.2

## 2. ตรวจสอบ Virtualization

WSL 2 ใช้ระบบ Virtualization ของเครื่อง หากปิดใช้งานอยู่ อาจเปิด WSL 2 ไม่ได้

1. กด `Ctrl + Shift + Esc` เพื่อเปิด Task Manager
2. เลือกเมนู **Performance**
3. เลือก **CPU**
4. มองหาคำว่า **Virtualization** ทางด้านขวา

ผลที่ควรเห็น:

```text
Virtualization: Enabled
```

หากแสดงเป็น `Disabled` ให้เข้า BIOS/UEFI ของเครื่องเพื่อเปิด Virtualization โดยชื่อเมนูอาจแตกต่างกันตามผู้ผลิต เช่น **Intel Virtualization Technology (VT-x)** หรือ **SVM Mode (AMD)** จากนั้นบันทึกการตั้งค่าและเปิด Windows ใหม่

## 3. ติดตั้ง WSL 2 และ Ubuntu 24.04

### 3.1 เปิด PowerShell แบบ Administrator

1. กดปุ่ม Start
2. ค้นหา `PowerShell`
3. คลิกขวาที่ PowerShell
4. เลือก **Run as administrator**

แนะนำให้ใช้ Windows Terminal ก็ได้ แต่หน้าต่างที่เปิดต้องมีสิทธิ์ Administrator ในขั้นตอนติดตั้งระบบ

### 3.2 ตรวจสอบ WSL และ Ubuntu ที่ติดตั้งอยู่

ตรวจสอบรายชื่อ distribution ที่ติดตั้งในเครื่องก่อน:

```powershell
wsl --list --verbose
```

ตัวอย่างผลลัพธ์:

```text
  NAME            STATE           VERSION
* Ubuntu-24.04    Stopped         2
```

- ถ้ามี `Ubuntu-24.04` และ `VERSION` เป็น `2` อยู่แล้ว ให้ข้ามขั้นตอนติดตั้งในหัวข้อ 3.3 แล้วไปหัวข้อ 3.4
- ถ้ามี `Ubuntu-24.04` แต่ `VERSION` เป็น `1` ให้ทำตามขั้นตอนแปลงเป็น WSL 2 ในหัวข้อ 3.4
- ถ้ายังไม่มี `Ubuntu-24.04` ให้ตรวจสอบชื่อ distribution ที่ติดตั้งได้ด้วยคำสั่งนี้:

```powershell
wsl --list --online
```

ตรวจสอบว่ามีชื่อ `Ubuntu-24.04` อยู่ในรายการก่อนดำเนินการต่อ

### 3.3 ติดตั้ง WSL และ Ubuntu 24.04

หากยังไม่มี Ubuntu 24.04 ให้รันคำสั่งนี้ใน PowerShell แบบ Administrator:

```powershell
wsl --install -d Ubuntu-24.04
```

คำสั่งนี้ใช้ติดตั้ง WSL และ distribution ที่ระบุ หากเป็นการติดตั้งครั้งแรก Windows อาจขอให้ Restart เครื่อง ให้บันทึกงานและรีสตาร์ตก่อนดำเนินการต่อ

หลังติดตั้งเสร็จ Ubuntu อาจเปิดขึ้นมาเพื่อให้สร้างบัญชีผู้ใช้ หากยังไม่เปิด สามารถเปิดจาก Start Menu โดยค้นหา `Ubuntu 24.04 LTS` หรือใช้คำสั่ง:

```powershell
wsl -d Ubuntu-24.04
```

> [!TIP]
> หากดาวน์โหลดผ่านช่องทางปกติไม่ได้ หรือติดตั้งค้างที่ `0.0%` ให้ลองคำสั่ง `wsl --install --web-download -d Ubuntu-24.04` เพื่อดาวน์โหลดผ่านช่องทางเว็บ

### 3.4 อัปเดต WSL และกำหนดให้ใช้ WSL 2

ใน PowerShell รันทีละคำสั่ง:

```powershell
wsl --update
wsl --set-default-version 2
```

คำสั่ง `wsl --set-default-version 2` กำหนดให้ distribution ที่ติดตั้งใหม่ใช้ WSL 2 เป็นค่าเริ่มต้น แต่ไม่ได้เปลี่ยน distribution เดิมที่เป็น WSL 1 ให้เอง

หากตรวจสอบในหัวข้อ 3.2 แล้วพบว่า Ubuntu 24.04 เป็นเวอร์ชัน 1 ให้แปลงด้วยคำสั่งนี้:

```powershell
wsl --set-version Ubuntu-24.04 2
```

การแปลงอาจใช้เวลาตามขนาดข้อมูล ควรสำรองข้อมูลสำคัญก่อนเริ่มแปลง

ตั้ง Ubuntu 24.04 เป็น distribution หลัก (ข้ามได้ หากไม่ต้องการเปลี่ยนค่าเริ่มต้น):

```powershell
wsl --set-default Ubuntu-24.04
```

### 3.5 ตรวจสอบเวอร์ชัน WSL

```powershell
wsl --list --verbose
```

ผลลัพธ์ของ `Ubuntu-24.04` ต้องแสดง `VERSION` เป็น `2` โดยค่า `STATE` อาจเป็น `Running` หรือ `Stopped` ก็ได้

ตัวอย่าง:

```text
  NAME            STATE           VERSION
* Ubuntu-24.04    Stopped         2
```

## 4. ตั้งค่าบัญชี Ubuntu ครั้งแรก

เปิด Ubuntu จาก Start Menu หรือใช้คำสั่งจาก PowerShell:

```powershell
wsl -d Ubuntu-24.04
```

เมื่อเปิดใช้งานครั้งแรก Ubuntu จะให้สร้างบัญชี Linux:

1. กรอกชื่อผู้ใช้ (Username) เป็นภาษาอังกฤษ ไม่เว้นวรรค
2. กำหนดรหัสผ่าน (Password)
3. กรอกรหัสผ่านอีกครั้งเพื่อยืนยัน

> [!NOTE]
> ขณะพิมพ์รหัสผ่าน หน้าจอจะไม่แสดงตัวอักษรหรือเครื่องหมาย `*` ถือเป็นพฤติกรรมปกติ ให้พิมพ์รหัสผ่านแล้วกด Enter ได้เลย

บัญชีนี้เป็นบัญชีของ Ubuntu แยกจากบัญชี Windows ใช้รหัสผ่านนี้เมื่อมีคำสั่งที่ต้องใช้ `sudo` ด้วย

## 5. อัปเดต Ubuntu และติดตั้งเครื่องมือพื้นฐาน

ขั้นตอนต่อไปนี้ให้รันใน **Ubuntu Terminal** เท่านั้น

### 5.1 อัปเดตรายการแพ็กเกจ

```bash
sudo apt update
```

### 5.2 อัปเกรดแพ็กเกจที่ติดตั้งอยู่

```bash
sudo apt upgrade -y
```

### 5.3 ติดตั้งเครื่องมือพื้นฐาน

```bash
sudo apt install -y \
  build-essential \
  ca-certificates \
  curl \
  wget \
  git \
  unzip \
  zip \
  gnupg \
  lsb-release \
  python3 \
  python3-pip \
  python3-venv
```

เครื่องมือที่ติดตั้งประกอบด้วย:

| เครื่องมือ | ใช้ทำอะไร |
|---|---|
| `build-essential` | คอมไพเลอร์และเครื่องมือสำหรับ build โปรแกรม |
| `ca-certificates` | ใบรับรองที่ใช้ตรวจสอบการเชื่อมต่อแบบ HTTPS |
| `curl`, `wget` | ดาวน์โหลดข้อมูลหรือไฟล์จากอินเทอร์เน็ต |
| `git` | จัดการเวอร์ชันของ source code |
| `unzip`, `zip` | แตกและสร้างไฟล์ ZIP |
| `gnupg` | จัดการกุญแจเข้ารหัสและตรวจสอบลายเซ็นดิจิทัลของแพ็กเกจ |
| `lsb-release` | ตรวจสอบข้อมูลรุ่นและรหัสเวอร์ชันของ Ubuntu |
| `python3` | ใช้งานภาษา Python |
| `python3-pip` | ติดตั้ง Python packages ภายใน virtual environment |
| `python3-venv` | สร้าง virtual environment สำหรับ Python |

### 5.4 วิธีติดตั้ง Python packages ที่แนะนำ

บน Ubuntu 24.04 ไม่ควรใช้ `pip` ติดตั้ง packages ลงใน Python ของระบบโดยตรง เพราะอาจพบข้อความ `externally-managed-environment` ให้สร้าง virtual environment แยกตามโปรเจกต์แทน

ตัวอย่างการสร้าง environment:

```bash
mkdir -p ~/workspace/python-demo
cd ~/workspace/python-demo
python3 -m venv .venv
source .venv/bin/activate
```

เมื่อต้องการติดตั้ง package ให้ทำหลังจาก activate environment แล้ว เช่น:

```bash
python -m pip install requests
```

เมื่อทำงานเสร็จและต้องการออกจาก environment:

```bash
deactivate
```

> [!WARNING]
> ไม่แนะนำให้แก้ปัญหาด้วย `sudo pip` หรือ `--break-system-packages` เพราะอาจกระทบ Python packages ที่ Ubuntu ใช้งานอยู่

## 6. ตั้งค่า Git และ SSH Key สำหรับ GitHub

### 6.1 ตั้งค่าข้อมูลประจำตัว (Git Config)

ตรวจสอบว่า Git ติดตั้งแล้ว:

```bash
git --version
```

ตั้งชื่อและอีเมลที่จะใช้บันทึก commit รวมถึงตั้งค่าพื้นฐานที่เหมาะสมกับสภาพแวดล้อม Linux บน WSL (แก้ไขตัวอย่างให้เป็นข้อมูลของคุณ):

```bash
git config --global user.name "YOUR_NAME"
git config --global user.email "YOUR_EMAIL@example.com"
git config --global init.defaultBranch main
git config --global core.autocrlf input
```

> [!NOTE]
> ค่าเหล่านี้เป็นข้อมูลผู้เขียนที่บันทึกอยู่ใน commit ไม่ใช่รหัสผ่านสำหรับเข้าสู่ระบบ GitHub

### 6.2 สร้างและเชื่อมต่อ SSH Key กับ GitHub

การใช้ SSH Key จะช่วยให้คุณ Push/Pull โค้ดกับ GitHub (โดยเฉพาะ Private Repository) ได้อย่างปลอดภัยโดยไม่ต้องกรอกรหัสผ่านทุกครั้ง

**1. สร้าง SSH Key ใหม่**
รันคำสั่งด้านล่างใน Ubuntu Terminal โดยเปลี่ยนอีเมลเป็นอีเมลบัญชี GitHub ของคุณ:

```bash
ssh-keygen -t ed25519 -C "YOUR_EMAIL@example.com"
```
เมื่อระบบถามหาตำแหน่งบันทึกไฟล์ ให้กด `Enter` เพื่อใช้ค่าเริ่มต้น หากระบบถาม Passphrase (รหัสผ่านชั้นที่สอง) คุณสามารถตั้งค่าเพิ่มความปลอดภัยหรือกด `Enter` ข้ามไปได้

**2. นำ SSH Key เข้าสู่ระบบ ssh-agent**
เริ่มการทำงานของ ssh-agent ใน background:
```bash
eval "$(ssh-agent -s)"
```
เพิ่ม Private Key ที่สร้างขึ้นเข้าสู่ระบบ:
```bash
ssh-add ~/.ssh/id_ed25519
```

**3. นำ Public Key ไปใส่ใน GitHub**
รันคำสั่งนี้เพื่อแสดงเนื้อหาของ Public Key บนหน้าจอ:
```bash
cat ~/.ssh/id_ed25519.pub
```
- ลากคลุมและคัดลอกข้อความทั้งหมดที่แสดงผล (จะขึ้นต้นด้วย `ssh-ed25519 ...`)
- ไปที่เว็บไซต์ GitHub มุมขวาบนคลิกรูปโปรไฟล์เลือก **Settings**
- เมนูด้านซ้ายเลือก **SSH and GPG keys**
- คลิกปุ่ม **New SSH key**
- ใส่ชื่อในช่อง Title (เช่น `WSL2 Ubuntu 24.04`) และวาง Key ที่คัดลอกมาลงในช่อง Key
- คลิก **Add SSH key**

**4. ทดสอบการเชื่อมต่อ**
รันคำสั่งทดสอบใน Terminal:
```bash
ssh -T git@github.com
```
หากเป็นการเชื่อมต่อครั้งแรก ระบบจะถามยืนยัน fingerprint ให้พิมพ์ `yes` แล้วกด `Enter` 
หากตั้งค่าสำเร็จ จะปรากฏข้อความ: `Hi [YourUsername]! You've successfully authenticated, but GitHub does not provide shell access.`

## 7. ติดตั้ง Visual Studio Code และเชื่อมต่อ WSL

### 7.1 ติดตั้ง Visual Studio Code บน Windows

1. เปิดเว็บไซต์ [Visual Studio Code](https://code.visualstudio.com/)
2. ดาวน์โหลดตัวติดตั้งสำหรับ Windows
3. ติดตั้งโปรแกรมตามขั้นตอน
4. ระหว่างติดตั้ง ให้เลือก **Add to PATH** หากมีตัวเลือกนี้

ติดตั้ง VS Code บน Windows ไม่จำเป็นต้องติดตั้งโปรแกรม VS Code ซ้ำภายใน Ubuntu

### 7.2 ติดตั้งส่วนเสริม WSL

1. เปิด Visual Studio Code
2. เปิดหน้า Extensions ด้วย `Ctrl + Shift + X`
3. ค้นหา `WSL`
4. ติดตั้งส่วนเสริมชื่อ **WSL** ที่เผยแพร่โดย Microsoft

### 7.3 เปิดโปรเจกต์จาก Ubuntu

ใน Ubuntu Terminal ให้เข้าไปยังโฟลเดอร์โปรเจกต์ แล้วใช้คำสั่ง:

```bash
code .
```

ครั้งแรก VS Code อาจติดตั้ง VS Code Server ภายใน WSL โดยอัตโนมัติ เมื่อเชื่อมต่อสำเร็จ แถบสถานะของ VS Code จะระบุว่าเชื่อมต่อกับ WSL อยู่

หากขึ้นข้อความ `code: command not found` ให้ตรวจสอบว่า VS Code ติดตั้งบน Windows และเลือก Add to PATH แล้ว จากนั้นปิดและเปิด Terminal ใหม่ก่อนลองอีกครั้ง

## 8. สร้างโฟลเดอร์เก็บโปรเจกต์

แนะนำให้เก็บโปรเจกต์ที่ใช้เครื่องมือ Linux ไว้ใน Linux filesystem เช่น `/home/<username>/workspace` เพื่อให้เครื่องมือ Linux อ่านและเขียนไฟล์ได้มีประสิทธิภาพดี ไม่ควรเก็บโปรเจกต์ Linux ไว้ใต้ `/mnt/c/` หากไม่มีเหตุผลจำเป็น

สร้างโฟลเดอร์ `workspace`:

```bash
mkdir -p ~/workspace
cd ~/workspace
```

ตรวจสอบตำแหน่งปัจจุบัน:

```bash
pwd
```

ตัวอย่างผลลัพธ์:

```text
/home/your-username/workspace
```

### 8.1 Clone โปรเจกต์จาก GitHub

ตัวอย่างนี้ใช้งานร่วมกับ SSH Key ที่ตั้งค่าไว้ในหัวข้อ 6.2 (สังเกตการใช้ `git@github.com:` แทน `https://`):

```bash
cd ~/workspace
git clone git@github.com:USERNAME/REPOSITORY.git
cd REPOSITORY
code .
```

### 8.2 เปิดโฟลเดอร์ปัจจุบันใน Windows File Explorer

```bash
explorer.exe .
```

### 8.3 เข้าถึงไฟล์ Windows จาก Ubuntu

ไดรฟ์ Windows จะอยู่ภายใต้ `/mnt/` เช่น:

| Windows | Ubuntu (WSL) |
|---|---|
| `C:\` | `/mnt/c/` |
| `D:\` | `/mnt/d/` |

ตัวอย่างการเข้าโฟลเดอร์ Downloads ของ Windows:

```bash
cd /mnt/c/Users/YOUR_WINDOWS_USER/Downloads
```

เปลี่ยน `YOUR_WINDOWS_USER` ให้เป็นชื่อโฟลเดอร์ผู้ใช้ Windows ของเครื่องนั้น

## 9. คำสั่ง WSL ที่ใช้บ่อย

คำสั่งในตารางนี้ให้รันจาก **PowerShell หรือ Windows Terminal ฝั่ง Windows**

| คำสั่ง | รายละเอียด |
|---|---|
| `wsl --status` | ดูสถานะและค่าเริ่มต้นของ WSL |
| `wsl --version` | ดูเวอร์ชัน WSL และส่วนประกอบ |
| `wsl -l -v` | ดู Linux distributions และเวอร์ชัน WSL |
| `wsl -d Ubuntu-24.04` | เปิด Ubuntu 24.04 |
| `wsl --shutdown` | ปิดการทำงานของ WSL ทุก distribution |
| `wsl --terminate Ubuntu-24.04` | ปิดเฉพาะ Ubuntu 24.04 |
| `wsl --update` | อัปเดต WSL |
| `wsl --set-default Ubuntu-24.04` | ตั้ง Ubuntu 24.04 เป็น distribution หลัก |

**หมายเหตุ:** หาก `wsl --version` ไม่รองรับ ให้ลอง `wsl --update` และตรวจสอบ Windows Update จากนั้นใช้ `wsl --status` หรือ `wsl -l -v` เพื่อตรวจสอบข้อมูลที่จำเป็น

## 10. คำสั่ง Ubuntu ที่ใช้บ่อย

คำสั่งในตารางนี้ให้รันจาก **Ubuntu Terminal**

| คำสั่ง | รายละเอียด |
|---|---|
| `pwd` | แสดงตำแหน่งโฟลเดอร์ปัจจุบัน |
| `ls -la` | แสดงไฟล์ทั้งหมดในโฟลเดอร์ |
| `cd folder-name` | เข้าโฟลเดอร์ที่ต้องการ |
| `cd ..` | ย้อนกลับไปโฟลเดอร์ก่อนหน้า |
| `cd ~` | กลับไป Home directory |
| `mkdir folder-name` | สร้างโฟลเดอร์ |
| `touch file-name` | สร้างไฟล์เปล่า |
| `clear` | ล้างข้อความบน Terminal |
| `sudo apt update` | อัปเดตรายการแพ็กเกจ |
| `sudo apt upgrade -y` | อัปเกรดแพ็กเกจ |
| `exit` | ออกจาก Ubuntu session |

## 11. ตรวจสอบว่าติดตั้งครบแล้ว

### ตรวจสอบจาก PowerShell

```powershell
wsl --status
wsl --list --verbose
```

ในผลลัพธ์ของ `wsl --list --verbose` ให้ตรวจว่า `Ubuntu-24.04` มีค่า `VERSION` เป็น `2`

### ตรวจสอบจาก Ubuntu Terminal

```bash
cat /etc/os-release
uname -r
git --version
python3 --version
ssh -T git@github.com
```

ในไฟล์ `/etc/os-release` ควรพบ `VERSION_ID="24.04"` และ `VERSION_CODENAME=noble`

รายการตรวจสอบก่อนเริ่มทำงาน:

- [ ] Windows 11 เปิดใช้งาน Virtualization แล้ว
- [ ] ติดตั้ง WSL สำเร็จ
- [ ] Ubuntu 24.04 LTS ติดตั้งแล้ว
- [ ] Ubuntu ใช้ WSL Version 2
- [ ] สร้าง Ubuntu username และ password แล้ว
- [ ] อัปเดต Ubuntu และติดตั้งเครื่องมือพื้นฐานแล้ว
- [ ] ตั้งค่า Git config (username, email) แล้ว
- [ ] **(ใหม่)** เชื่อมต่อ SSH Key กับ GitHub สำเร็จแล้ว
- [ ] ติดตั้ง Visual Studio Code และ WSL extension แล้ว
- [ ] เปิดโปรเจกต์จาก Ubuntu ด้วย `code .` ได้
- [ ] สร้าง `~/workspace` สำหรับเก็บโปรเจกต์แล้ว

## ปัญหาที่พบบ่อย

<details>
<summary><strong>1. พิมพ์คำสั่ง wsl แล้วไม่พบคำสั่ง</strong></summary>

1. ตรวจสอบว่าเปิด PowerShell บน Windows ไม่ใช่ Ubuntu Terminal
2. ติดตั้ง Windows Update ให้เป็นปัจจุบัน แล้ว Restart เครื่อง
3. เปิด PowerShell แบบ Administrator และตรวจสอบอีกครั้ง:

```powershell
wsl.exe --help
```

หากยังไม่พบคำสั่ง ให้เปิด Windows Features ตามข้อ 2 ด้านล่าง แล้ว Restart เครื่อง หากยังแก้ไม่ได้ ให้ตรวจสอบคู่มือแก้ปัญหาของ Microsoft
</details>

<details>
<summary><strong>2. แจ้งว่า Virtual Machine Platform หรือ Virtualization ยังไม่พร้อม</strong></summary>

- ตรวจสอบว่า Virtualization เปิดอยู่ใน BIOS/UEFI และแสดงเป็น Enabled ใน Task Manager
- เปิด PowerShell แบบ Administrator แล้วรันคำสั่งต่อไปนี้เพื่อเปิด Windows Features ที่จำเป็น:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

จากนั้น Restart Windows แล้วตรวจสอบ WSL อีกครั้ง
</details>

<details>
<summary><strong>3. ติดตั้งค้างที่ 0.0%</strong></summary>

ลองใช้ตัวเลือกดาวน์โหลดจากเว็บโดยตรงใน PowerShell:

```powershell
wsl --install --web-download -d Ubuntu-24.04
```
</details>

<details>
<summary><strong>4. Ubuntu แสดง VERSION เป็น 1</strong></summary>

สำรองข้อมูลสำคัญก่อน แล้วรันคำสั่งนี้ใน PowerShell:

```powershell
wsl --set-version Ubuntu-24.04 2
```

รอให้การแปลงเสร็จ จากนั้นตรวจสอบด้วย `wsl -l -v`
</details>

<details>
<summary><strong>5. ไม่แน่ใจว่า Ubuntu ใช้ชื่อ distribution อะไร</strong></summary>

รันคำสั่งใน PowerShell:

```powershell
wsl --list --verbose
```

นำชื่อที่แสดงในคอลัมน์ `NAME` ไปใช้กับคำสั่ง WSL
</details>

<details>
<summary><strong>6. พิมพ์รหัสผ่าน Ubuntu แล้วไม่มีตัวอักษรแสดง</strong></summary>

เป็นเรื่องปกติของ Linux ขณะพิมพ์รหัสผ่านจะไม่มีตัวอักษรหรือเครื่องหมายใดแสดงบนหน้าจอ ให้พิมพ์รหัสผ่านแล้วกด Enter
</details>

<details>
<summary><strong>7. ติดตั้ง Python package แล้วขึ้น externally-managed-environment</strong></summary>

Ubuntu ป้องกันการติดตั้ง packages ลงใน Python ของระบบโดยตรง ให้สร้างและเปิดใช้งาน virtual environment ตามหัวข้อ 5.4 แล้วติดตั้ง package ภายใน environment นั้น
</details>

<details>
<summary><strong>8. ใช้คำสั่ง code . แล้วขึ้น command not found</strong></summary>

ตรวจสอบว่า VS Code ติดตั้งบน Windows และได้เลือก **Add to PATH** ระหว่างติดตั้งแล้ว ปิดและเปิด Ubuntu Terminal ใหม่ จากนั้นลอง `code .` อีกครั้ง
</details>

---

## แหล่งข้อมูล

เอกสารอ้างอิงจากผู้พัฒนาโดยตรง:

- [Microsoft Learn: Install WSL](https://learn.microsoft.com/windows/wsl/install)
- [Microsoft Learn: Manual installation steps for WSL](https://learn.microsoft.com/windows/wsl/install-manual)
- [Microsoft Learn: Troubleshooting WSL](https://learn.microsoft.com/windows/wsl/troubleshooting)
- [Microsoft Learn: Basic commands for WSL](https://learn.microsoft.com/windows/wsl/basic-commands)
- [Microsoft Learn: Set up a WSL development environment](https://learn.microsoft.com/windows/wsl/setup/environment)
- [Microsoft Learn: Get started using VS Code with WSL](https://learn.microsoft.com/windows/wsl/tutorials/wsl-vscode)
- [Microsoft Learn: Working across Windows and Linux file systems](https://learn.microsoft.com/windows/wsl/filesystems)
- [Ubuntu Documentation: Install Ubuntu on WSL 2](https://documentation.ubuntu.com/wsl/latest/howto/install-ubuntu-wsl2/)
- [Ubuntu for Developers: Develop with Python on Ubuntu](https://documentation.ubuntu.com/ubuntu-for-developers/tutorials/python-use/)
- [GitHub Docs: Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

---

<div align="center">

**Environment Setup Notes** · Windows 11 / WSL 2 / Ubuntu 24.04 LTS

</div>