# คู่มือการสอบ Lab Exam (AWS EC2)

## รายละเอียดการสอบ
- **วันที่:** 7 มีนาคม
- **เวลา:** 10:00 น. - 12:00 น.
- **ระยะเวลาต่อคน:** 15 นาที
- **คำแนะนำ:**
  - เป็นการสอบเดี่ยว
  - คุณต้องนำเสนอผลลัพธ์ให้กับอาจารย์
  - เตรียมคอมพิวเตอร์ของคุณให้พร้อมสำหรับการแสดงผลลัพธ์ของทุกข้อ
  - มาถึงห้องสอบก่อนเวลาสอบ **5 นาที**
  - หลังจากครบ 15 นาที คุณต้องออกจากห้องสอบ
----
## **คู่มือการทำข้อสอบทีละขั้นตอน**

### **1. สร้าง Virtual Machine (EC2) บน AWS**
#### **1.1 เปิดใช้งาน EC2 Instance**
1. ล็อกอินเข้า AWS และไปที่ [AWS EC2 Console](https://aws.amazon.com/)  
2. คลิก **Launch Instance** เพื่อสร้างเซิร์ฟเวอร์ใหม่  
3. ตั้งชื่อ **my-ec2-vm**
4. **เลือก Amazon Machine Image (AMI):**
   - Ubuntu 22.04 LTS (หรือ Ubuntu 20.04 LTS)
5. **เลือกขนาดเครื่อง (Instance Type):**
   - `t2.micro` (ถ้าใช้ Free Tier)
6. **สร้าง Key Pair** (ใช้เชื่อมต่อ SSH)
7. **Security Group:**
   - อนุญาต `SSH (port 22)`, `HTTP (port 80)`, `HTTPS (port 443)`
8. คลิก **Launch Instance** และรอให้สถานะเป็น `Running`
9. คัดลอก **Public IPv4** ของ EC2

ใช้นี้ก่อนกันไม่ได้
```
    chmod 400 your-key.pem
```

#### **1.2 เชื่อมต่อไปยัง EC2 ผ่าน SSH**
```sh
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```
🔹 **your-key.pem** = ไฟล์ Key Pair ที่ดาวน์โหลดจาก AWS  
🔹 **your-ec2-public-ip** = Public IPv4 ของ EC2  

#### **1.3 อัปเดตแพ็กเกจใน EC2**
```sh
sudo apt update && sudo apt upgrade -y
```
----
### **2. ติดตั้ง Nginx และแสดงหน้าเว็บเริ่มต้น**
```sh
sudo apt install nginx -y
systemctl status nginx
curl localhost
```
----
### **3. แก้ไข HTML เพื่อแสดง index.html ใหม่ใน Nginx**
```sh
sudo nano /var/www/html/index.html
```
แทนที่ด้วย:
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Custom Page</title>
</head>
<body>
    <h1>Hello from My Custom Nginx Page!</h1>
</body>
</html>
```
บันทึกไฟล์และรีสตาร์ท Nginx:
```sh
sudo systemctl restart nginx
```
----
### **4. ติดตั้ง Docker และรัน Hello-World**
```sh
sudo apt install docker.io -y
docker --version
docker run hello-world
```
----
### **5. รัน Docker Image ของ Nginx และแสดงหน้าเว็บเริ่มต้น**
```sh
docker pull nginx
docker run -d -p 80:80 nginx
curl localhost
```
----
### **6. สร้างอิมเมจใหม่ที่มีไฟล์ HTML และรันคอนเทนเนอร์**
```sh
nano Dockerfile
```
เพิ่มเนื้อหานี้:
```Dockerfile
FROM nginx
COPY index.html /usr/share/nginx/html/index.html
```
สร้างและรัน:
```sh
docker build -t custom-nginx .
docker run -d -p 80:80 custom-nginx
```
----
### **7. Push อิมเมจไปยัง Docker Hub**
```sh
docker login
docker tag custom-nginx <your-dockerhub-username>/custom-nginx
docker push <your-dockerhub-username>/custom-nginx
```
----
### **8. แมป Volume ของคอนเทนเนอร์ให้แสดงไฟล์ HTML จากข้อ 3**
```sh
docker run -d -p 80:80 -v /var/www/html/index.html:/usr/share/nginx/html/index.html custom-nginx
curl localhost
```
----
### **9. รัน docker-compose.yml ที่ได้รับจากอาจารย์ และแสดงผลในเบราว์เซอร์**
```sh
scp docker-compose.yml ubuntu@<EC2-Public-IP>:~
docker-compose up -d
curl localhost
```
----
### **10. สร้างไฟล์ HTML ใหม่ใน EC2 และเขียน docker-compose.yml**
```sh
nano index.html
```
เพิ่มเนื้อหานี้:
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Custom Page</title>
</head>
<body>
    <h1>Hello from Docker Compose!</h1>
</body>
</html>
```
บันทึกและสร้าง `docker-compose.yml`:
```sh
nano docker-compose.yml
```
เพิ่มเนื้อหานี้:
```yaml
version: '3'
services:
  web:
    image: custom-nginx
    ports:
      - "80:80"
    volumes:
      - ./index.html:/usr/share/nginx/html/index.html
```
รันคอนเทนเนอร์:
```sh
docker-compose up -d
curl localhost
```

---

✅ **ตอนนี้คุณพร้อมสำหรับการสอบแล้ว! ฝึกทำทุกขั้นตอนให้คล่องเพื่อให้สามารถทำได้ภายใน 15 นาที** 🚀

