---
name: data-dictionary-wms-tobacco
description: "ใช้อธิบายโครงสร้างฐานข้อมูล SQL Server (schema dbo) ของระบบคลังสินค้า (WMS) โครงการการยาสูบแห่งประเทศไทย ครอบคลุม 33 ตาราง ได้แก่ ข้อมูลหลัก (เจ้าของสินค้า ผู้จัดจำหน่าย สินค้า คลัง โซน โลเคชั่น), สต็อกและพาเลท, เอกสารรับสินค้า (PO/Receipt), เอกสารเบิก/โอน/เคลม (Transfer/Sales) และ log ของระบบ เรียกใช้เมื่อผู้ใช้ถามว่าตารางหรือคอลัมน์ไหนเก็บข้อมูลอะไร, ต้องการเขียนหรือตรวจสอบ SQL query, ต้องการรู้ความสัมพันธ์ระหว่างตาราง (join key), ออกแบบรายงาน/Interface, หรือสอบถามความหมายของ field เช่น qty_avail, qty_booked, pick_hold, seq_group"
---

1. ภาพรวมระบบ
   - ตารางทั้งหมดอยู่ใน dbo ทุกตารางส่วนใหญ่มี row_id (bigint, identity) เป็นเลขลำดับแถว
   - คอลัมน์ audit ที่พบบ่อย: maker (ผู้แก้ไขล่าสุด), last_update, create_by/created_by, create_date
   - ตอบเป็นภาษาไทย คงชื่อตารางและคอลัมน์เป็นภาษาอังกฤษตามต้นฉบับ

2. กลุ่มตาราง
   - ระบบ: application_log (log การทำงาน, activity = ชื่อ Stored Procedure, error_flag = สถานะ),
     user_profile (ผู้ใช้งาน/สิทธิ์), parameter_tbl (พารามิเตอร์), menu_report (รายงานและพารามิเตอร์ 1-9),
     last_number_counter (running number ตาม seq_group + doc_type + order_type + prefix),
     unit_of_measure (หน่วยนับ)
   - ข้อมูลหลัก: client_master (เจ้าของสินค้า มีค่าบริการ Storage/Handling และ counter ของเอกสารต่างๆ),
     Suppliers (ผู้จัดจำหน่าย), warehouses (คลัง), zones (โซนในคลัง), locations (โลเคชั่น: aisle/bay/level/slot, ขนาด, ความจุพาเลท)
   - สินค้า: product_master (ข้อมูลสินค้า), product_barcode, product_uom (ลำดับชั้นหน่วย/pack_hierarchy),
     product_prefer_location, product_zoning (โซนที่กำหนดให้สินค้า เรียงตาม search_seq)
   - สต็อก/พาเลท: pallet_master, pallet_location (สต็อกคงเหลือ ณ โลเคชั่น), pallet_history, pallet_qa (กัก/ปล่อย QA),
     stock movement และ movements_history (ประวัติการเคลื่อนไหว)
   - ขาเข้า: purchase_order + Order_lines (ใบรอรับ Interface จาก Oracle), Receipt_master + Receipt_detail (ใบรับสินค้าจริง)
   - ขาออก/โอน: Transfer_order + Transfer_Detail (ขอเบิกใช้), Sales_orders + Sales_detail (เบิกเพื่อส่งเคลม),
     hanel_master + hanel_Detail (บันทึกการใช้งานลูกแม่พิมพ์ มี qty_use/qty_balace),
     works_orders (ใบงาน/task)

3. ความสัมพันธ์หลัก (เอกสารไม่ได้ระบุ FK ไว้ เป็นการอนุมานจากชื่อ PK และคอลัมน์ ให้บอกผู้ใช้ว่าเป็นข้อสันนิษฐาน)
   - สินค้า: client + item_no (product_master, product_barcode, product_uom, product_zoning, product_prefer_location)
   - ตำแหน่งจัดเก็บ: warehouse + location (locations -> zone) และ warehouse + zone (zones)
   - สต็อก: pallet_location.pallet = pallet_master.pallet_no; pallet_location.warehouse + location = locations
     ใช้ qty_avail (คงเหลือ), qty_booked (ถูกจอง), qty_rcvd (รับเข้า), qty_pick_pack_booked (จองโดยแพ็ค)
   - ใบรับ: Receipt_master.receipt_no = Receipt_detail.receipt_no (ร่วมกับ client)
   - ใบสั่งซื้อ: purchase_order.order_no = Order_lines.order_no (ร่วมกับ client)
   - ใบเบิก/โอน/ขาย: Transfer_order.order_no = Transfer_Detail.order_no, Sales_orders.order_no = Sales_detail.order_no
   - ใบงาน: works_orders.works_order = stock movement.works_order
   - rec_num เป็นเลขลำดับชุดข้อมูลที่ใช้เชื่อม pallet_location, pallet_history, pallet_qa, stock movement

4. ข้อควรระวัง
   - ชื่อคอลัมน์ในใบเอกสารสลับกันได้: purchase_order ใช้ order_no, Receipt_master ใช้ receipt_no, Sales_detail มีทั้ง org_item และ item_no
   - Data Type บางตัวไม่ตรงกับการใช้งานจริง (เช่น Order_lines.price เป็น nvarchar, pallet_master.last_update เป็น nvarchar)
   - เอกสารต้นฉบับมีคำอธิบายคลาดเคลื่อนบางจุด เช่น client_master กลุ่ม bill_* ที่คำอธิบายเลื่อนไปหนึ่งแถว
     และ locations ที่ phy_height/phy_depth ระบุความยาว/ความสูงสลับกัน
     ถ้าเจอจุดแบบนี้ให้บอกผู้ใช้ว่าอาจเป็นข้อผิดพลาดของเอกสาร และแนะนำให้ตรวจกับโครงสร้างจริงในฐานข้อมูล
   - user_profile มีคอลัมน์ password ห้ามแสดงค่า ห้ามเขียน query ที่ดึงค่านี้ออกมา
   - Interface (inship/outship, *_change_cs, export_num) เกี่ยวกับการส่งข้อมูลระหว่างระบบ อย่าเดาความหมายเกินกว่าที่เอกสารระบุ

5. วิธีตอบ
   - ถามความหมายคอลัมน์: ระบุตาราง ชื่อคอลัมน์ ชนิดข้อมูล และคำอธิบายตามเอกสาร
   - ขอ SQL: เขียน T-SQL ตามชื่อจริงในเอกสาร ใช้ alias, join ด้วย client + key ที่เกี่ยวข้อง และระบุเงื่อนไขกรอง status
     ถ้าไม่ทราบค่า status ที่แท้จริงให้ถามผู้ใช้ ไม่เดาค่า
   - ถามภาพรวม: อธิบายเป็นลำดับ ขาเข้า (PO -> Receipt -> pallet_location) และขาออก (Transfer/Sales -> pick -> movement)
   - ถ้าข้อมูลที่ถามไม่มีในเอกสาร ให้บอกตรงๆ ว่าไม่มีและเสนอทางตรวจสอบ ห้ามแต่งชื่อตารางหรือคอลัมน์ขึ้นเอง