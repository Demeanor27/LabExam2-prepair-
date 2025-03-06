# Lab Final DevOps (macOS + AWS EC2)

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

## **คู่มือการทำข้อสอบทีละขั้นตอน (macOS + AWS EC2)**

### **1. สร้าง Virtual Machine (EC2) บน AWS**
```sh
ssh -i "TestLab.pem" ubuntu@<EC2-Public-IP>
```
```sh
sudo apt update && sudo apt upgrade -y
```

### **2. ติดตั้ง Nginx และแสดงหน้าเว็บเริ่มต้น**
```sh
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
curl localhost
```

### **3. แก้ไข HTML เพื่อแสดง index.html ใหม่ใน Nginx**
```sh
sudo nano /var/www/html/index.nginx-debian.html
```
**แก้ไขเนื้อหา แล้วบันทึก (`CTRL + O → Enter → CTRL + X`)**
```sh
sudo systemctl reload nginx
```

### **4. ติดตั้ง Docker และรัน Hello-World**
```sh
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```

### **5. รัน Docker Image ของ Nginx และแสดงหน้าเว็บเริ่มต้น**
```sh
docker run -d -p 81:80 nginx
curl localhost:81
```

### **6. รับไฟล์ HTML จากอาจารย์ สร้างอิมเมจใหม่ และรันคอนเทนเนอร์**
```sh
nano custom.html
```
เพิ่มเนื้อหาที่ได้รับจากอาจารย์ จากนั้นบันทึกไฟล์ (`CTRL + O → Enter → CTRL + X`)
```sh
nano Dockerfile
```
เพิ่มเนื้อหานี้:
```Dockerfile
FROM nginx:latest
COPY custom.html /usr/share/nginx/html/index.html
```
สร้างอิมเมจและรันคอนเทนเนอร์:
```sh
docker build -t my-nginx-image .
docker run -d -p 82:80 my-nginx-image
curl localhost:82
```

### **7. Push อิมเมจไปยัง Docker Hub**
```sh
docker login
docker tag my-nginx-image <your-dockerhub-username>/my-nginx-image:latest
docker push <your-dockerhub-username>/my-nginx-image:latest
```

### **8. แมป Volume ของคอนเทนเนอร์ให้แสดงไฟล์ HTML จากข้อ 3**
```sh
docker run -d -p 83:80 -v /var/www/html:/usr/share/nginx/html nginx
curl localhost:83
```

### **9. รัน docker-compose.yml ที่ได้รับจากอาจารย์ และแสดงผลในเบราว์เซอร์**
```sh
nano docker-compose.yml
```
เพิ่มเนื้อหานี้:
```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "84:80"
    volumes:
      - ./html:/usr/share/nginx/html
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
```
```sh
mkdir ~/html
nano ~/html/index.html
```
เพิ่มเนื้อหานี้:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Proctor Test</title>
</head>
<body>
    <h1>Proctor’s Docker Compose Test</h1>
    <p>This is running via docker-compose.yml!</p>
</body>
</html>
```
```sh
sudo apt install docker-compose -y
docker-compose up -d
curl localhost:84
```

### **10. สร้างไฟล์ HTML ใหม่ และเขียน docker-compose.yml**
```sh
mkdir ~/question10
cd ~/question10
nano new-page.html
```
เพิ่มเนื้อหานี้:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Question 10 Test</title>
</head>
<body>
    <h1>My New Docker Compose Page</h1>
    <p>This is for Question 10, running my custom image!</p>
</body>
</html>
```
```sh
nano docker-compose.yml
```
เพิ่มเนื้อหานี้:
```yaml
version: '3'
services:
  web:
    image: my-nginx-image
    ports:
      - "85:80"
    volumes:
      - ./new-page.html:/usr/share/nginx/html/index.html
```
```sh
docker-compose up -d
curl localhost:85
```

---

✅ **ตอนนี้คุณพร้อมสำหรับการสอบแล้ว! ฝึกทำทุกขั้นตอนให้คล่องเพื่อให้สามารถทำได้ภายใน 15 นาที** 🚀

