# LabExam2-prepair-

#### Sec 702 Time(9.30-9.45)
----
💡 ก่อนเริ่มต้น:
1.ติดตั้ง Homebrew (ถ้ายังไม่มี) → ใช้คำสั่งนี้เพื่อติดตั้ง:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
2.อัปเดตแพ็กเกจ
```
brew update
```
----
1. สร้าง Virtual Machine (VM)
   สร้าง Virtual Machine (VM) MacOS สามารถใช้ Multipass หรือ VirtualBox เพื่อสร้าง VM ได้:
   
   วิธีติดตั้ง Multipass (แนะนำสำหรับ macOS)
   ```
   brew install --cask multipass
   ```
   สร้าง Ubuntu VM ขนาด 2GB RAM, 10GB Disk:
   ```
   multipass launch -n my-vm -c 2 -m 2G -d 10G
   ```
   เข้าไปใน VM:
   ```
   multipass shell my-vm
   ```
   อัปเดตแพ็กเกจใน VM:
   ```
   sudo apt update && sudo apt upgrade -y
   ```
----
2. ติดตั้ง Nginx และแสดงหน้าเว็บ

   ติดตั้ง Nginx ใน VM
   ```
   sudo apt install nginx -y
   ```
   ตรวจสอบว่า Nginx ทำงานอยู่
   ```
   systemctl status nginx
   ```
   (กด ```q``` เพื่อออกจาก status log)

   เปิดหน้าเว็บใน VM
   ```
   curl localhost
   ```
   ถ้าคุณต้องการดูผ่านเบราว์เซอร์ใน macOS:
   
   ต้องหาหมายเลข IP ของ VM ก่อน:
   ```
   multipass list
   ```
   แล้วใช้ IP นั้นเปิด ```http://<VM-IP>``` ในเบราว์เซอร์

----
