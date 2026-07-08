# Lab#2: การทดสอบระบบด้วย Postman และ JMeter (2.5 คะแนน)

## 2.1 Postman API Testing (1.5 คะแนน)
ไฟล์ `6800XXXX-Lab2.postman_collection.json` ในโฟลเดอร์นี้ คือ Collection พร้อมใช้งาน:

**วิธีใช้**
1. เปิด Postman → คลิก **Import** → เลือกไฟล์ `6800XXXX-Lab2.postman_collection.json`
2. จะเห็น Collection ชื่อ `6800XXXX-Lab2` (แก้ชื่อเป็นรหัสนักศึกษาจริงของคุณ) ที่มี Request 5 รายการ:
   - `GET - Public API (list posts)`
   - `GET - Post by ID`
   - `POST - Create new post`
   - `PUT - Update post`
   - `DELETE - Delete post`
3. กด **Send** ทีละ Request แล้วดูผลลัพธ์ในแท็บ **Test Results** (ทุก Request มี Test Script อัตโนมัติติดมาให้แล้ว ตรวจสอบ Status Code / เนื้อหา Response / เวลาตอบสนอง)
4. Run ทั้ง Collection พร้อมกันได้ด้วยปุ่ม **Run collection** (Collection Runner) เพื่อดูผลรวมทุก Test

## 2.2 JMeter Load Testing (1.0 คะแนน)
JMeter เป็นโปรแกรม Desktop (GUI) ทำตามขั้นตอนนี้:

1. **ติดตั้ง**: ต้องมี Java (JDK 8+) ก่อน แล้วดาวน์โหลด Apache JMeter จาก https://jmeter.apache.org/download_jmeter.cgi แตกไฟล์และรัน `bin/jmeter.bat` (Windows) หรือ `bin/jmeter.sh` (Mac/Linux)
2. **สร้าง Test Plan ใหม่**: คลิกขวาที่ Test Plan → Rename เป็น `6800XXXX-LoadTest`
3. **เพิ่ม Thread Group**: คลิกขวา Test Plan → Add → Threads (Users) → Thread Group
   - Number of Threads (users): `50`
   - Ramp-up period: `10` วินาที
   - Loop Count: `5`
4. **เพิ่ม HTTP Request Sampler**: คลิกขวา Thread Group → Add → Sampler → HTTP Request
   - Protocol: `https`
   - Server Name: `jsonplaceholder.typicode.com`
   - Path: `/posts`
   - Method: `GET`
5. **เพิ่ม Listener สำหรับดูผลลัพธ์**: คลิกขวา Thread Group → Add → Listener → เพิ่มทั้ง
   - `View Results Tree`
   - `Summary Report`
6. **รัน Test**: กดปุ่ม ▶ (Start) แล้วดูผลใน Summary Report — บันทึกภาพหน้าจอผลลัพธ์เก็บไว้ส่งอาจารย์
7. **วิเคราะห์ผล**: ดูค่า Average, Min, Max, Error % ใน Summary Report

### ตัวอย่างผลการวิเคราะห์ (สำหรับเป็นแนวทางเขียนรายงาน)
> จากการทดสอบ Load Testing ด้วย Apache JMeter โดยจำลองผู้ใช้งาน 50 คน กำหนด Loop Count = 5
> ระบบประมวลผลคำขอทั้งหมด 250 Requests โดยไม่เกิดข้อผิดพลาด (Error Rate = 0.00%)
> Average Response Time ≈ 671 ms, Min ≈ 51 ms, Max ≈ 7,076 ms, Throughput ≈ 15.9 Requests/วินาที
> สรุปว่าระบบรองรับการใช้งานในระดับที่กำหนดได้อย่างมีประสิทธิภาพ

> **หมายเหตุ**: ตัวเลขข้างต้นเป็นตัวอย่างเท่านั้น ให้แทนที่ด้วยผลลัพธ์จริงที่ได้จากการรันบนเครื่องของคุณ