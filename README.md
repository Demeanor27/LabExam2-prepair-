# คู่มือการสอบ Lab Exam

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

## **คู่มือการทำข้อสอบทีละขั้นตอน**

### **1. สร้าง Virtual Machine (VM)**
#### **ใช้ Multipass (แนะนำสำหรับ macOS)**
```sh
brew install --cask multipass
multipass launch -n my-vm -c 2 -m 2G -d 10G
multipass shell my-vm
```
#### **อัปเดตและอัปเกรดแพ็กเกจใน VM**
```sh
sudo apt update && sudo apt upgrade -y
```

### **2. ติดตั้ง Nginx (ไม่ใช้ Docker) และแสดงหน้าเว็บเริ่มต้น**
```sh
sudo apt install nginx -y
systemctl status nginx
curl localhost
```

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

### **4. ติดตั้ง Docker ใน VM และรัน Hello-World**
```sh
sudo apt install docker.io -y
docker --version
docker run hello-world
```

### **5. รัน Docker Image ของ Nginx และแสดงหน้าเว็บเริ่มต้น**
```sh
docker pull nginx
docker run -d -p 80:80 nginx
curl localhost
```

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

### **7. Push อิมเมจไปยัง Docker Hub**
```sh
docker login
docker tag custom-nginx <your-dockerhub-username>/custom-nginx
docker push <your-dockerhub-username>/custom-nginx
```

### **8. แมป Volume ของคอนเทนเนอร์ให้แสดงไฟล์ HTML จากข้อ 3**
```sh
docker run -d -p 80:80 -v /var/www/html/index.html:/usr/share/nginx/html/index.html custom-nginx
curl localhost
```

### **9. รัน docker-compose.yml ที่ได้รับจากอาจารย์ และแสดงผลในเบราว์เซอร์**
```sh
scp docker-compose.yml ubuntu@<VM-IP>:~
docker-compose up -d
curl localhost
```

### **10. สร้างไฟล์ HTML ใหม่ใน VM และเขียน docker-compose.yml**
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

