---
name: goodissue-page-ui
description: "โครงสร้างและพฤติกรรมของหน้า Goods Issue (GoodissuePage) ในแอป TCP Handheld (Ionic 3 + Angular 5) 3 แท็บ GI / อ้างอิง JO / รายละเอียด พร้อมกติกาข้อมูลต่อแท็บ สูตรคำนวณน้ำหนักกระดาษ และ service ที่เรียก ใช้เมื่อผู้ใช้แก้ เพิ่ม หรือ debug หน้า goodissue.ts / goodissue.html เช่น เพิ่มช่องกรอก ย้ายฟิลด์ระหว่างแท็บ แก้ปุ่ม แก้ draft/JO/spec กระดาษ หรือสร้างหน้าใหม่ที่มีหน้าตาแบบเดียวกัน Use when editing or extending the Goods Issue page of TCP Handheld."
---

# Goods Issue page (GoodissuePage)

หน้าเบิกสินค้า (GI) ของแอป TCP Handheld: `src/pages/goodissue/goodissue.{ts,html,scss,module.ts}`
`@IonicPage({ name: "GoodsIssuePage", segment: "GoodsIssue" })` สร้างเอกสาร GI อ้างอิง JO (ใบสั่งผลิต) เพิ่มรายการเข้า Draft แล้วสร้าง line

ข้อมูลจริงทุกอย่างมาจาก `Service` (`src/services/service.ts`) แบบ Promise + xml2js ดูรูปแบบใน skill `tcp-wms-handheld`

## โครงหน้า: 3 แท็บด้านล่าง

สลับด้วย `tab` (`'gi' | 'jo' | 'detail'`) ผ่าน `<ion-segment [(ngModel)]="tab" (ionChange)="onTabChange()">` ใน `<ion-footer>`
แท็บ `jo` และ `detail` แสดงแถบสรุป (`.summary`) เลขที่ GI + ลูกค้า ด้านบน

### แท็บ 1: GI (สร้าง/เลือกเอกสาร)
การ์ด 3 ใบ ตามลำดับ
1. ข้อมูลเอกสาร: client (readonly), เลขที่ GI + ปุ่มเลือก GI, วันที่ (เปิด CalendarPage), เลข JO + ปุ่มเลือก JO, บรรทัดสรุป SO | ชื่อลูกค้า JO (`.jo-hint`)
2. ตั้งค่าเอกสาร: branch, book, warehouse, station + zone (คู่, `.pair`), type (NORMAL)
3. ผู้ขอเบิก: employee (select), department (เลือกผ่าน modal) + ชื่อ, customer (เลือกผ่าน modal), remark

ท้ายแท็บ `.action-bar`: ปุ่มล้าง `clearAll()` และปุ่มสร้าง Header `doCreate_Header()`

### แท็บ 2: อ้างอิง JO
1. การ์ด "อ้างอิง JO"
   - เลข JO (readonly) + ปุ่มโหลดใหม่ `doGetJO()`
   - รายการวัตถุดิบของ JO (`data_jo`) มี "เลือกทั้งหมด" + จำนวนแถว (`.chip`)
   - แต่ละแถว (`.jo-item` > `.jo-row`): checkbox, item_no, description, ช่องจำนวน + หน่วย
   - เมื่อติ๊กเลือก ขยายส่วน `.jo-spec` ของแถวนั้น: หมายเหตุ, ขนาดกระดาษ (กว้าง × ยาว, แกรม × จำนวนแผ่น), ช่องจำนวน + ปุ่มคำนวณ `doCalcJO(r)` + ข้อความสูตร
   - ปุ่มใหญ่ "เพิ่มที่เลือก" `doAddJOToDraft()`
2. การ์ด "เพิ่มสินค้าอื่น" (ไม่อยู่ใน JO): บาร์โค้ด (Enter หรือปุ่มเลือกสินค้า) → เมื่อเลือกแล้วจึงแสดง item_no, description (readonly), หน่วย (select), จำนวน, ปุ่ม "เพิ่มเข้าดราฟ" `doAddOtherItem()` (ไม่มี spec กระดาษ)
3. การ์ด "Draft": ปุ่มรีเฟรช `dogetlistdraft()` + รายการ (แก้จำนวน / ลบ ต่อแถว)
4. ท้ายแท็บ (เฉพาะมี `order_no`): ปุ่ม "เพิ่มเข้า GI" `doCreate_Lines()`

### แท็บ 3: รายละเอียด
ปุ่มรีเฟรช `doGetGoodsIssue_Detail()` ถ้าไม่มีข้อมูลแสดง `.empty` จากนั้นการ์ดต่อ 1 line (`.line-card`): badge line_no, item_no, description, status, จำนวน + หน่วย, ปุ่มลบ line

## รูปแบบ UI (คลาสที่ใช้ซ้ำ ไม่ต้องคิดใหม่)

| ส่วน | คลาส / องค์ประกอบ |
|---|---|
| การ์ด + หัวการ์ด | `.card` > `.card-title` (`<ion-icon>` + `<span>`) |
| ฟิลด์ | `.field` > `.field-label` + `<ion-input class="box">` (ฟิลด์อ่านอย่างเดียวเพิ่ม `readonly` + `.readonly`; ช่องผลลัพธ์ใช้ `.strong`) |
| ฟิลด์ + ปุ่มข้าง | `.field-row` ใส่ input + `<button ion-button class="side-btn">` |
| select | `<ion-item no-lines class="box">` > `<ion-select interface="popover">` |
| สองช่องคู่กัน | `.pair`; คั่นด้วย `<div class="times">×</div>` สำหรับสูตรคูณ |
| หัวกลุ่มย่อย | `.group-label` |
| ผลลัพธ์/คำนวณ | `.result` > `.result-top` (ช่อง + `.calc-btn`) + `.hint` |
| แถวรายการ | JO: `.jo-item` / `.jo-row` (+ `.checked`) / `.jo-qty` / `.jo-spec`; Draft: `.draft-row` (`.idx`, `.info` > `.t` ชื่อ / `.s` รอง); Detail: `.line-card` (`.line-head`, `.line-body`) |
| แถบปุ่ม | `.action-bar` (ปุ่มหลัก `.btn-main`, ปุ่มรอง `.btn-ghost`); ปุ่มเต็มความกว้าง `.btn-main-block`; ปุ่มไอคอน `.btn-icon` |
| ตัวนับ | `.chip` (`.push` ดันไปขวา) |

ข้อความทั้งหมดใช้ `| translate` (คีย์ใน `src/assets/i18n/{th,en,la}.json`) คีย์ของหน้านี้: `goods_issue`, `gi_no`, `gi_section_doc`, `gi_section_setup`, `gi_section_jo`, `gi_section_draft`, `gi_jo_no`, `gi_jo_load`, `gi_jo_hint`, `gi_jo_reload`, `gi_select_all`, `gi_jo_add_selected`, `gi_select_gi`, `gi_employee`, `gi_select_employee`, `gi_department`, `gi_department_name`, `gi_select_department`, `gi_select_customer`, `gi_paper_size`, `gi_paper_width`, `gi_paper_length`, `gi_gsm`, `gi_sheet_qty`, `gi_formula_hint`, `gi_view_draft`, `gi_view_detail`, `gi_empty_detail`, `gi_add_to_gi`, `gi_tab_gi/jo/detail`, `btn_calc`, `btn_clear`, `btn_newsheet` (ข้อความ "เพิ่มสินค้าอื่น" และ "เพิ่มเข้าดราฟ" ยัง hard-code เป็นไทย) ถ้าเพิ่มข้อความใหม่ ให้เพิ่มคีย์ในทั้ง 3 ไฟล์

## กติกาข้อมูล (ต้องรักษาไว้)

1. **ค่าจาก service เป็น array** (xml2js `explicitArray`) อ่านด้วย `getFirst(v)` เสมอ ห้ามใช้ `v` ตรงๆ
2. **ล้างข้อมูลข้ามแท็บ**: เมื่อเปลี่ยน GI เป็นใบอื่น (เทียบ `order_no` ใหม่กับเดิม) หรือกด `clearAll()` ต้องเรียก `resetOtherTabs()` ซึ่งล้าง `data_jo`, `joAll`, `oAdd`/บาร์โค้ด/หน่วย (`resetAddItem()`), `draftSpecs`, `data_draft`, `data_detail`, `data_detail_view` ล้างเฉพาะฝั่งแอป ไม่ลบ `GoodsIssue_Draft` ฝั่งเซิร์ฟเวอร์ และถ้าเลือกใบเดิมซ้ำห้ามล้าง `draftSpecs`
3. **Spec กระดาษ/หมายเหตุอยู่ต่อบรรทัด JO** (`r.remark`, `r.width`, `r.length`, `r.gsm`, `r.sheets`) แถวที่เพิ่มเข้า Draft สำเร็จจะล้างค่าเหล่านี้ทันที เพื่อกันเพิ่มซ้ำ
4. **`draftSpecs`**: Draft ไม่เก็บขนาดกระดาษ จึงจำ spec ฝั่งแอป แล้วหลัง `doCreate_Lines()` สำเร็จ `applyDraftSpecs()` จะดึง line ของ GI มาจับคู่ตาม `item_no` (ตามลำดับ) แล้วบันทึกด้วย `update_GoodsIssue_Detail_Spec`; `doDelete_detail_daft` ต้องตัด spec ของ item นั้นออกด้วย
5. **`remark`** ส่งไปกับ `insert_TempGoodsIssue` (อาร์กิวเมนต์ที่ 6) แล้วเก็บในคอลัมน์ `GoodsIssue_Draft.remark` (ในกรณีที่ item เดิมอยู่ในดราฟแล้ว จะบวก qty และเขียนทับ remark เมื่อส่งค่าใหม่มาเท่านั้น)
6. **ต้องมี `order_no` ก่อนเพิ่มรายการ** (สร้าง Header ที่แท็บ GI ก่อน) ตรวจทั้งใน `doAddJOToDraft` และ `doAddOtherItem`
7. **Loader**: ใช้ `presentLoading()` / `finishLoding()` เท่านั้น (นับซ้อนได้ ปิดเมื่อครบ) ห้ามสร้าง `loadingCtrl` เอง
8. **`doGetJO()` มี guard `joLoading`** กันเรียกซ้ำ; `onTabChange()` ดึงรายการ JO อัตโนมัติเมื่อเข้าแท็บ `jo` โดยมีเลข JO แต่ `data_jo` ว่าง
9. หลังสร้าง Header สำเร็จ ย้ายไปแท็บ `jo` เอง; หลังสร้าง line สำเร็จ ล้าง Draft แล้วย้ายไปแท็บ `detail`

## สูตรคำนวณจำนวน (น้ำหนัก)

`qty = กว้าง × ยาว × แกรม × จำนวนแผ่น ÷ 1000` ปัดขึ้นทศนิยม 3 ตำแหน่ง ใช้ `calcWeight(width, length, gsm, sheets)`
(ปัด `raw*1000` ด้วย `toFixed(6)` ก่อน `Math.ceil` เพื่อกันเลขลอยตัว) ต้องกรอกทั้ง 4 ค่าและมากกว่า 0 ไม่ครบให้แจ้งเตือน ผลลัพธ์ใส่ใน `r.qty` ของแถวนั้น (JO) แก้เองต่อได้

## Service และ Modal ที่หน้านี้พึ่งพา

- Service: `get_branchs`, `get_BookAJ`, `get_WarehouseByUser`, `get_Station`, `get_Zone`, `get_Employee`, `get_JO_Detail(oClient, oDocNo)`, `insert_TempGoodsIssue(item_no, qty, uom, order_no, maker, remark)`, `get_TempGoodsIssue`, `update_TempGoodsIssue`, `delete_TempGoodsIssue`, `clear_TempGoodsIssue`, `create_GoodsIssue_Header`, `create_GoodsIssue_Lines`, `get_GoodsIssue_Detail`, `update_GoodsIssue_Detail_Spec`, `delete_GoodsIssue_Detail`, `get_Item_Barcode`, `get_UOM`, `get_Barcode_Detail`
- Modal: `ModalgoodissuelistPage` (เลือก GI), `ModaljolistPage` (เลือก JO ได้ `doc_no`, `sales_order`, `customer`, `customer_name`), `ModaldepartmentPage`, `CustomermodalPage`, `ItemNomodalPage` (`oType: "OTHER"`), `CalendarPage`
- ฟิลด์ที่ `get_JO_Detail` ต้องคืน: `line_no, item_no, description, qty, uom` (มาจากตาราง `production_order_material`)

## เช็กลิสต์ก่อนส่งงานเมื่อแก้หน้านี้

- [ ] ทุกเมธอดที่ template เรียก (`doCalcJO`, `doAddJOToDraft` ฯลฯ) มีอยู่จริงใน `.ts` (error `... is not a function` มักมาจากข้อนี้)
- [ ] ถ้าเพิ่มฟิลด์ใหม่ในแถว JO ต้องเพิ่มใน `doGetJO()` (ค่าเริ่มต้น), ใน `doAddJOToDraft()` (การส่ง/เก็บ spec) และล้างค่าหลังเพิ่ม
- [ ] ถ้าเพิ่มพารามิเตอร์ให้ `insert_TempGoodsIssue` ต้องแก้ครบ 4 ชั้น: ตาราง `GoodsIssue_Draft` → SP `sp_inserttempgoodsissue` → `RFService.cs` → `service.ts` → จุดเรียกในหน้า (มี 2 จุด: JO และสินค้าอื่น)
- [ ] ถ้าเพิ่ม state ใหม่ที่ผูกกับเอกสาร ให้ล้างใน `resetOtherTabs()` หรือ `clearAll()`
- [ ] div ใน template ปิดครบ และใช้คลาสจากตารางด้านบนก่อนสร้างคลาสใหม่
- [ ] ข้อความใหม่ใส่คีย์แปลครบ th / en / la