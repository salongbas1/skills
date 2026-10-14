---
name: wms-swg-database-schema
description: "ความรู้โครงสร้างฐานข้อมูล SQL Server similanwls_SWG ของระบบ Similan WLS/WMS (383 ตาราง ไม่มี foreign key) ใช้ช่วยเขียน ตรวจ และอธิบาย SQL/T-SQL เรื่อง PO, Receipt/Check-in, Sales Order, Pick/Pack/Delivery, Invoice, สต็อกพาเลท (pallet_location, stock_movements), Stock Count, Transfer/Adjust, BOM/Kitting, Master Data, POS, TMS และ interface log ใช้ทุกครั้งที่ผู้ใช้ถามว่าข้อมูลอยู่ตารางไหน join อย่างไร ขอ query หรือรายงานจากฐานข้อมูล WMS นี้ ถามความหมายคอลัมน์ หรือพูดถึงชื่อตารางเช่น sales_orders, receipt_master, product_master แม้ไม่ได้พูดคำว่า schema"
---

ฐานข้อมูล similanwls_SWG (SQL Server, schema dbo อย่างเดียว) มี 383 ตาราง ไม่มี foreign key และมี PRIMARY KEY จริงแค่ 17 ตาราง ไม่มีข้อมูลจริงให้ดู จึงไม่ทราบค่าที่เก็บอยู่ในคอลัมน์ ความรู้ในนี้มาจากสคริปต์ schema และเอกสาร Data Dictionary ภาษาไทย (ครอบคลุม 31 ตาราง)

## ข้อจำกัดที่ต้องบอกผู้ใช้เมื่อเกี่ยวข้อง
- ความสัมพันธ์ระหว่างตารางอนุมานจากชื่อคอลัมน์และคำอธิบายใน dictionary ไม่ได้มาจาก constraint ให้บอกระดับความมั่นใจเมื่อ join ข้ามตารางที่ไม่ชัด
- ค่าของ status, order_type, grade, flag ต่างๆ ไม่ทราบ (dictionary ก็ไม่ระบุ) อย่าเดาค่าใน WHERE ให้เสนอ SELECT DISTINCT status, COUNT(*) ... GROUP BY status ให้ผู้ใช้รันก่อน ชื่อสถานะในคู่มือ Similan (เช่น DATA ENTRY, WAITING) อาจเก็บต่างจากที่เห็นในคู่มือ
- ไม่ทราบว่ามี index อะไรนอกจาก PRIMARY KEY ตารางใหญ่ (stock_movements, movements_history, pallet_history, ตาราง *_log) ให้กรอง client และช่วงวันที่เสมอ และแนะนำให้ตรวจ sys.indexes ก่อนรันบน production
- Data dictionary ไม่ตรงกับฐานข้อมูล SWG 100% บางคอลัมน์มีเฉพาะใน dictionary (เช่น product_master.description_eng, stock_movements.engine) และบางคอลัมน์มีเฉพาะใน SWG ให้ใช้ dictionary อ่านความหมายเท่านั้น และให้ผู้ใช้ยืนยันชื่อคอลัมน์ด้วย SELECT COLUMN_NAME, DATA_TYPE FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = '<ตาราง>' เมื่อไม่แน่ใจ อย่าเดาชื่อคอลัมน์
- คำอธิบายใน dictionary มีที่คลาดเคลื่อนอยู่บ้าง (เช่น client_master.bill_* เลื่อนผิดช่อง bill_postcode ถูกอธิบายเป็นเบอร์โทร, Transfer_Detail.order_no ถูกอธิบายเป็น "เล่มเอกสาร") ให้ใช้เป็นแนวทาง ไม่ใช่ข้อเท็จจริงสุดท้าย
- ตาราง hanel_master และ hanel_detail อยู่ใน dictionary แต่ไม่พบใน schema SWG ห้ามเขียน query กับสองตารางนี้โดยไม่ยืนยันกับผู้ใช้ก่อน
- ความหมายของกลุ่ม OI_*, POOI_*, pd_*, SO_*, Salesorder_*, PurchaseOrder_* อนุมานจากชื่อ ให้บอกว่าเป็นการอนุมาน

## ข้อตกลงของฐานข้อมูลนี้
- Dialect คือ T-SQL: ใช้ TOP, [ชื่อ], ISNULL, GETDATE(), OFFSET/FETCH ไม่ใช้ LIMIT
- row_id (มี 284 ตาราง) เป็น IDENTITY surrogate key ของแต่ละตาราง ห้ามใช้ join ข้ามตาราง ให้ใช้ business key
- คอลัมน์ client อยู่ใน 223 ตาราง (multi-tenant) ต้อง join และกรองด้วย client เสมอ
- Audit: maker + last_update (ผู้แก้/เวลาแก้ล่าสุด) ส่วนวันที่สร้างชื่อไม่คงที่ (create_date, created_date, created_by, create_by) ต้องตรวจทุกครั้ง
- เอกสารมี book (เล่มเอกสาร) และ branch เลขเอกสารรันจากตาราง last_number_counter (seq_group, doc_type, order_type, prefix, last_number)
- ชนิดข้อมูลส่วนใหญ่ nvarchar(50) จำนวนเป็น numeric(15,3) บางตาราง (books_master, OI_*) ชื่อคอลัมน์ผสมตัวพิมพ์ใหญ่
- คอลัมน์ที่ซ้ำในตารางเอกสารและสต็อก: remark01..10, item_size, item_color, item_class, lot_no, batch_no, grade, expiry_date, prod_date, ppq_packing, ppq_qty

## แผนที่เอกสาร Header ↔ Detail
เพิ่ม h.client = d.client ทุกครั้ง

| กระบวนการ | Header | Detail | คีย์เชื่อม | ความมั่นใจ |
|---|---|---|---|---|
| ใบขอซื้อ PR | pr_master | pr_details | order_no | สูง |
| ใบสั่งซื้อ PO | purchase_orders | order_lines | order_no | สูง (dictionary: ใบรอรับสินค้าจากการสั่งซื้อที่ Interface จาก Oracle; order_lines มี qty_po, qty_received, qty_putaway) |
| รับสินค้า | receipt_master | receipt_detail | receipt_no | สูง |
| Incoming/ASN | incoming_master | incoming_detail | incoming_no | สูง (receipt_master/receipt_detail ก็มี incoming_no) |
| ใบสั่งขาย SO | sales_orders | sales_details | order_no | สูง |
| จองสต็อก | reserve_orders | reserve_details | order_no | สูง |
| Packing | packing_order | packing_order_detail | packing_no (มี sales_order ทั้งคู่) | สูง |
| ส่งสินค้า DO | delivery_master | delivery_detail | delivery | สูง (detail มี sales_order, so_line_no, load_summary, rec_num, pallet) |
| Load summary | loadsummary_master | loadsummary_detail | load_summary | สูง (master ไม่มี client) |
| Invoice | invoice_master | invoice_detail | invoice_no | สูง |
| ขอเบิก/โอน | transfer_orders | transfer_details | order_no | สูง (dictionary เรียก "ข้อมูลการขอเบิกใช้สินค้า"; ตารางมี zone_dest, location_dest) |
| ปรับยอด | adjusts_master | adjusts_detail | adjust_no | สูง |
| Stocktake | stocktake_master | stockcount_history | stocktake_ref | กลาง |
| Cycle count | cycle_count_master | cycle_count_detail | order_no | กลาง |
| BOM/Kitting | bom_master | bom_detail | setcode + version | สูง |
| PD | pd_master | pd_details | order_no | สูง |
| OI | OI_master | OI_detail | OI_no | กลาง |

ความหมายทางธุรกิจตาม dictionary: sales_orders/sales_details ถูกอธิบายว่า "การเบิกเพื่อส่งเคลม" ซึ่งอาจเป็นของ deployment หรือเวอร์ชันที่ต่างจาก SWG ให้ถามผู้ใช้ก่อนเรียกเอกสารว่า "เคลม" หรือ "ขาย" และ order_lines.qty_received / qty_putaway ถูกอธิบายว่า "จำนวนที่ต้องการรับ / ต้องการจัดเก็บ" ไม่ใช่ยอดที่รับแล้วแน่นอน ให้ตรวจก่อนนำไปรวมยอด

เชื่อมเอกสารข้ามขั้น: delivery_detail.sales_order → sales_orders.order_no, invoice_master.sales_order / delivery, sales_details.invoice_no, sales_details.oi_no, receipt_detail.client_po_no → PO (ตรวจรูปแบบค่าก่อน)

## Master data lookup
| ต้องการ | ตาราง | คีย์ |
|---|---|---|
| สินค้า | product_master (261 คอลัมน์) | client + item_no (item_code, alternate_code เป็นรหัสอื่น) |
| หน่วยนับ/แพ็ก | product_uom | client + item_no + item_uom |
| บาร์โค้ด | product_barcode | client + barcode_no + item_no |
| ลูกค้า | customer_master | client + customer (มี cust_id ด้วย ตรวจว่าเอกสารเก็บตัวไหน) |
| ซัพพลายเออร์ | suppliers | client + sup_id (PK ตาม dictionary) |
| client | client_master | client |
| คลัง/โซน | warehouses, zones | warehouse / warehouse + zone |
| ตำแหน่งรายตัว | locations | warehouse + location (location_setup เก็บช่วง aisle/bay/level/slot สำหรับสร้างตำแหน่ง ไม่ใช่รายตัว) |
| ผู้ใช้/สิทธิ์ | user_profile, user_group, group_access, menu_options | user_id, user_group |

สำคัญ: suppliers.supplier คือ "คำนำหน้าผู้จัดจำหน่าย" ไม่ใช่รหัส ส่วนคอลัมน์ supplier ในตารางเอกสาร (receipt_master, purchase_orders, order_lines, pallet_history ฯลฯ) คือ "รหัสผู้ขาย" จึงน่าจะ join doc.supplier = suppliers.sup_id (เป็นการอนุมานจาก dictionary ให้ผู้ใช้ตรวจกับข้อมูลจริงก่อน)

คีย์เชิงออกแบบอื่นตาม dictionary (ไม่ได้แปลว่ามี PRIMARY KEY constraint จริง): pallet_master (pallet_no), product_zoning (client + item_no + zone + search_seq), product_prefer_location (client + item_no + warehouse), last_number_counter (seq_group + doc_type + order_type + prefix)

## สต็อกและการเคลื่อนไหว
- สต็อกปัจจุบัน: pallet_location (client, item_no, pallet, warehouse, location, uom, qty_rcvd, qty_avail, qty_booked, qty_pick_pack_booked, status) ใช้ rec_num เป็นตัวระบุเรคคอร์ดสต็อก
- ความหมายจาก dictionary: qty_rcvd = จำนวนที่รับ, qty_avail = คงเหลือ, qty_booked = ถูกจอง, qty_pick_pack_booked = ถูกจองโดยการแพ็ค, rec_num = "เลขที่ลำดับชุดของข้อมูล", pallet_flag = flag การใช้งานเลขที่พาเลท
- งาน/การเคลื่อนไหว: stock_movements (งานที่ยังเปิด) และ movements_history (ประวัติ) มี rec_num, movement_type, pallet_from/to, location_from/to, qty, qty_moved, works_order, sales_order, order_no; task_no = ลำดับของใบงาน, activity = ลำดับของการทำงาน, priority = ลำดับความสำคัญ (เป็นตัวเลข ไม่ใช่ข้อความ)
- rec_num ปรากฏใน 33 ตารางสต็อก/นับ/โอน (pallet_history, delivery_detail, adjusts_detail, stockcount_history ฯลฯ)
- ยอดคงเหลือต่อสินค้า: SUM(qty_avail) จาก pallet_location กรอง client และ item_no (ตรวจก่อนว่าสถานะ/ตำแหน่ง hold ควรรวมหรือไม่)

## ตารางที่ไม่ควรใช้เป็นแหล่งข้อมูลหลัก
ชื่อขึ้นต้น temp_ / Temp_, ลงท้าย _temp, _bk, _debug, _archive, _log_sj และตารางเฉพาะกิจ (a, a_location, Sheet1$, GG, DEBUG, _fix_scan, product_master1, product_uom_008_04_8121) ถ้าผู้ใช้ไม่ได้ระบุ ให้ใช้ตารางจริง ไม่ใช่สำเนา

## ความปลอดภัย
- ห้าม SELECT คอลัมน์ password (user_profile.password, web_user.password) และห้ามเสนอ query ที่ export รหัสผ่าน ถ้าต้องดูผู้ใช้ให้เลือกเฉพาะ user_id, username, user_group, operational_status
- ค่าเริ่มต้นเป็นอ่านอย่างเดียว ถ้าผู้ใช้ขอ UPDATE/DELETE/INSERT ให้: (1) เขียน SELECT ที่ใช้ WHERE เดียวกันเพื่อ preview (2) ครอบด้วย BEGIN TRAN ... ROLLBACK สำหรับทดสอบ (3) เตือนว่าการแก้ตรงที่ฐานข้อมูลข้าม logic ของแอป อาจทำให้สถานะเอกสาร pallet_location, stock_movements และ history ไม่สอดคล้องกัน คู่มือ Similan ระบุให้แก้เอกสารย้อนหลังผ่านเมนู Miscellaneous Maintenance Tool โดย Administrator
- ข้อมูลส่วนบุคคล (ที่อยู่ โทร อีเมลลูกค้า/พนักงาน) ดึงเฉพาะเมื่อผู้ใช้ต้องการจริง

## วิธีตอบ
1. ระบุโดเมนของคำถาม (inbound / outbound / stock / master ฯลฯ)
2. เลือกตารางจากแผนที่ด้านบน ถ้าไม่แน่ใจชื่อคอลัมน์ให้ขอผลจาก INFORMATION_SCHEMA.COLUMNS
3. เขียน T-SQL ที่ join ด้วย client + business key ใช้ alias เลือกเฉพาะคอลัมน์ที่ต้องใช้ (ไม่ใช้ SELECT * กับตารางกว้าง)
4. ตอบตามลำดับ: โค้ด SQL → อธิบายสั้นๆ ว่า join อย่างไร → สมมติฐานและสิ่งที่ต้องตรวจ (ค่า status, ความมั่นใจของ join) และบอกว่ายังไม่ได้ทดสอบกับข้อมูลจริง
5. ถ้าคอลัมน์ที่ผู้ใช้พูดถึงไม่ปรากฏในตาราง ให้ถามหรือเสนอคอลัมน์ใกล้เคียง ไม่สร้างชื่อคอลัมน์ขึ้นเอง