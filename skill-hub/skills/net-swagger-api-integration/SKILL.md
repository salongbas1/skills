---
name: net-swagger-api-integration
description: "ใช้สำหรับเชื่อมต่อ จัดการ และเรียกใช้งาน .NET Web API ผ่านเอกสาร Swagger/OpenAPI เหมาะสำหรับใช้เมื่อต้องการดึงข้อมูลหรือเพิ่มทักษะ (Skills) เข้าสู่ระบบหลังบ้านที่เป็น .NET"
---

1. **การตรวจสอบโครงสร้าง API (Swagger Definition)**
   - ตรวจสอบ URL ของ Swagger JSON (เช่น `/swagger/v1/swagger.json`) เพื่อทำความเข้าใจ Endpoint และ Payload ล่าสุดของ .NET API

2. **การดึงข้อมูลทักษะทั้งหมด (Get Skills)**
   - ส่งคำขอแบบ `GET` ไปยัง `/api/skills`
   - นำข้อมูล JSON Array ที่ได้รับมาแสดงผลหรือประมวลผลต่อตามที่ผู้ใช้ต้องการ

3. **การเพิ่มทักษะใหม่ (Add Skill)**
   - ส่งคำขอแบบ `POST` ไปยัง `/api/skills` พร้อมแนบ JSON Body ที่ประกอบด้วยโครงสร้างดังนี้:
     ```json
     {
       "name": "ชื่อทักษะ",
       "description": "คำอธิบายทักษะ",
       "category": "หมวดหมู่"
     }
     ```
