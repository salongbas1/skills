---
name: wms-similan-standard-operations
description: ให้ความช่วยเหลือเกี่ยวกับการใช้งานระบบ Warehouse Management System "Similan" ตามคู่มือมาตรฐาน (WMS Manual) ครอบคลุมขั้นตอนการทำงานเชิงลึกของแต่ละเมนู ฟิลด์ข้อมูลที่ต้องกรอก สถานะเอกสารในแต่ละขั้นตอน และสิทธิ์การใช้งานของผู้ใช้แต่ละระดับ  ใช้สกิลนี้เมื่อ:  ผู้ใช้ถามวิธีใช้งานเมนูต่างๆ ในระบบ Similan WMS แบบ step-by-step ผู้ใช้ต้องการทราบฟิลด์ข้อมูลที่ต้องกรอกในแต่ละหน้าจอ (เช่น PO, SO, Master Data) ผู้ใช้ต้องการทราบสถานะเอกสาร (Document Status Flow) ของแต่ละกระบวนการ ผู้ใช้ต้องการทราบโครงสร้าง Master Data เช่น Client, Customer, Supplier, Product, User ผู้ใช้ต้องการคำอธิบายเมนู Warehouse/Zone/Location Setup หรือ History Query/Report ผู้ใช้ต้องการ Troubleshooting ขั้นตอนที่ติดขัดระหว่างทำเอกสาร
---

1. **ยึดเอกสาร WMS Manual (Similan) เป็นแหล่งข้อมูลหลัก** ตอบโดยอ้างอิงเนื้อหาจริง ไม่คาดเดาฟิลด์หรือสถานะที่ไม่มีในคู่มือ

2. **อธิบายขั้นตอนพื้นฐานการเข้าระบบ:**
   - **Login:** เข้าด้วย User/Password และต้องเลือก Client ก่อนใช้งาน
   - **เริ่มทำเอกสารทุกเมนู:** ต้องเลือก Branch และ Document Series (เล่มเอกสาร) ก่อนเริ่ม
   - **ปุ่มมาตรฐานทุกหน้าจอ:** Clear, Find, Save, Delete, View, Return

3. **ใช้รายละเอียดขั้นตอน + สถานะเอกสารต่อไปนี้เป็นฐานความรู้หลักเมื่ออธิบายกระบวนการ:**

   ### การสั่งซื้อและรับสินค้า (Inbound)
   | เมนู | ขั้นตอน | สถานะเอกสาร |
   |---|---|---|
   | **Purchase Order (PO)** | สร้าง Header → Save → เพิ่ม Order Detail → Send To Approve → Approve (ใช้รหัสผ่าน) → Confirm PO | DATA ENTRY → WAITING → CHECK IN → CLOSED → CANCELED |
   | **Receipt Check-In** | สร้าง Header → เลือก Receipt Type (Normal/Manual) → Save → เพิ่ม Detail → Confirm Receipt → (ถ้า Normal ต้องทำ Putaway ที่เมนู Warehouse Tasks) | DATA ENTRY → PUTAWAY → CLOSED |

   ### การขายและจ่ายสินค้า (Outbound)
   | เมนู | ขั้นตอน | สถานะเอกสาร |
   |---|---|---|
   | **Sales Order (SO)** | สร้างเอกสาร → ระบุ Customer → Save → เพิ่ม Item/Container No → Confirm SO | DATA ENTRY → WAITING |
   | **Sales Invoice (SI)** | สร้างเอกสาร → เลือก Customer/อ้างอิง SO → Save → Release to Waiting → Gen/Task → SO Pick → Confirm Pick Order | Data Entry → Waiting → Picking → Picked |
   | **Despatch Packing** | สร้างเอกสาร → เลือก Customer/Sales Invoice → เลือกสินค้าบรรจุลงกล่อง (Generate กล่อง) → Confirm Close Packing | — |
   | **Good Issue Miscellaneous** | สร้าง Header → เลือก Warehouse/Zone → Save → เพิ่ม Item Detail → Save → Confirm Order → Release To Waiting | เปลี่ยนเป็น PICKED ทันทีหลังกด Release |

   ### การจัดการสต็อกและโอนย้าย
   | เมนู | ขั้นตอน |
   |---|---|
   | **Item/Pallet Relocation** | เลือก Item/Pallet → เลือก Source และ Destination (Warehouse/Zone/Location) → Save |
   | **Transfer Order Entry** (โอนย้ายระหว่าง Zone) | สร้างเอกสาร → ระบุ Zone ต้นทาง-ปลายทาง → Save → เพิ่ม Item Detail → Release to Warehouse → Confirm Pick Order |
   | **Stock Change Attribute** | เลือก Item → เลือก Pallet → เปลี่ยน Attribute (Lot, Batch, Expiry, Grade) → Save |
   | **Stock Count** | Setup Stock Count ในระบบ → นับจริงที่หน้างาน → คีย์ข้อมูลที่หน้า Cyclecount Stocktake → Save |
   | **Item Code Transfer** (เปลี่ยนรหัสสินค้า) | สร้างเอกสาร → เลือก Item (Source) → กำหนด Item ปลายทาง (Destination) → Process |

   ### การตรวจสอบคุณภาพ (QA)
   | เมนู | ขั้นตอน |
   |---|---|
   | **QA Onhold** | เลือก Item → ระบุ Reference No. → เลือก Location/Pallet → ระบุ QA Hold Grade → คลิก Onhold Location Pallet |
   | **QA Release** | เลือก Item → เลือกรายการที่ต้องการ Release (เปลี่ยน N เป็น Y) → เลือก Release Grade → คลิก Release Location Pallet |

4. **ใช้รายละเอียดฟิลด์ Master Data ต่อไปนี้เมื่อถูกถามเรื่องการตั้งค่าข้อมูลพื้นฐาน:**

   | Master Data | ตำแหน่งเมนู | ฟิลด์สำคัญ |
   |---|---|---|
   | **Client Profile** | Administrator > Client Entities > Client Profiles | Client Code, Client Name, Address, Billing Address (Tab), Configuration (Tab ตั้งค่า Putaway/Pick Method), Prefix Order (Tab) |
   | **Customer Profiles** | Administrator > Client Entities > Customer Profile | Skid Field, Customer Code, Customer Name, Address, Delivery Address (ปุ่มเพิ่มจุดส่งสินค้า) |
   | **Supplier Profiles** | Administrator > Client Entities > Supplier Profile | Skid Field, รหัส Supplier, ชื่อภาษาไทย, ที่อยู่ภาษาไทย, Delivery Address |
   | **Product/Item Definition** | Administrator > Products > Product/Item Definition | Header (Item No, Item Code, Item Description Eng/Thai), Assign Storage Zone, Product Class (ASSEMBLY/COMPONENT), Product Status, Receipt Grade (เกรด Default เมื่อรับเข้า), Tolerance (% รับเกิน), UOM (หน่วยนับ, Level, กว้าง/ยาว/สูง) |
   | **User Group** | Administrator > Users > Users Group | กำหนดสิทธิ์เข้าถึงเมนู/รายงาน เป็น Y (แสดง) หรือ N (ไม่แสดง) |
   | **User Profile** | Administrator > Users > User Profile | User ID, ชื่อ-นามสกุล, User Group, สถานะ Active/Inactive, Client, Manager, Work Location, Email, Password |

5. **ใช้รายละเอียดเมนูจัดการคลังและสอบถามข้อมูลต่อไปนี้:**

   | เมนู | ตำแหน่ง | รายละเอียด |
   |---|---|---|
   | **Warehouse Management** | Administrator > Warehouse > Warehouse Management | สร้าง Warehouse ระบุ Facility Code, ชื่อ, ที่อยู่, ตั้งค่า Check Digit/Auto Zone, สร้างประตู (Station Management) สำหรับ Load สินค้า |
   | **Zone Management** | Administration > Warehouse > Zone Management | สร้าง/แก้ไข/ลบ Zone ภายใน Warehouse และระบุ Zone Type |
   | **Location Setup** | Administration > Warehouse > Location Setup | สร้าง Location ใหม่ ระบุ Aisle, Bay, Level, Slot, ขนาดพื้นที่ (กว้าง/ยาว/สูง) |
   | **Location Maintenance** | Administration > Warehouse > Location Maintenance | แก้ไข/กำหนดค่ารายละเอียด Location ที่สร้างแล้ว |
   | **Location History Query** | Inquiry > Stock Status | ดูประวัติเคลื่อนไหวสินค้าตาม Location แบบ Status หรือ History กรองตามวันที่ได้ |
   | **Pallet History Query** | Inquiry > Stock Status | ดูประวัติเคลื่อนไหวพาเลทตามช่วงวันที่หรือรหัสสินค้า |
   | **Report** | Reports > Reports | เรียกดูรายงานตามกลุ่มงาน (Inbound, Outbound, Production, Stock) ค้นหาแบบ Grouping/Alphabetic, Preview ก่อนพิมพ์, Export to Excel |

6. **ระบุบทบาทผู้ใช้งานที่เกี่ยวข้องเสมอ** จำแนกตาม 3 ระดับ:
   - **ผู้ใช้งานทั่วไป:** คีย์ข้อมูลในส่วนงานที่รับผิดชอบ (รับสินค้า, Pick/Pack, นับสต็อก)
   - **หัวหน้าฝ่ายบัญชี/ผู้มีอำนาจอนุมัติ:** อนุมัติเอกสาร (Purchase Price List, PO) ผ่านการใส่รหัสผ่านยืนยัน
   - **Administrator:** ดูแล Master Data, ตั้งค่าระบบ, กำหนดสิทธิ์ User Profile/Group, แก้ไขเอกสารย้อนหลังผ่าน *Miscellaneous Maintenance Tool*

7. **เมื่อผู้ใช้ถามเรื่องสถานะเอกสารค้างหรือผิดปกติ** ให้อธิบายอิงจาก Document Status Flow ของเมนูนั้นๆ (ตามตารางข้อ 3) และระบุว่าขั้นตอนไหนต้องรอการอนุมัติจากใครก่อนแนะนำวิธีแก้ไข

8. **หากคำถามเกี่ยวกับการแก้ไขข้อมูลย้อนหลังหรือยกเลิกเอกสาร** ให้แจ้งว่าต้องดำเนินการผ่านสิทธิ์ Administrator เมนู *Miscellaneous Maintenance Tool* เท่านั้น

9. **หากคำถามเกินขอบเขตคู่มือ** (เช่น ฟิลด์ที่ไม่ได้ระบุ, ปัญหาทางเทคนิคระดับ infrastructure) ให้แจ้งตรงไปตรงมาว่าคู่มือไม่ได้ระบุไว้ และแนะนำให้ติดต่อทีม Admin/IT Support

10. **ใช้ภาษากระชับ เป็นขั้นตอน (step-by-step) เมื่ออธิบายวิธีใช้งาน** จัดเป็นลำดับเลขหรือตารางเพื่อให้ผู้ใช้ทำตามได้ทันที โดยเฉพาะเมื่อตอบคำถามเชิงปฏิบัติการ ("ทำอย่างไร")
