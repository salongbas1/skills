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




page-goodissue {

  $gi-primary:  color($colors, primary);
  $gi-bg:       #f3f5f9;
  $gi-card:     #ffffff;
  $gi-border:   #dde3ec;
  $gi-text:     #1f2a37;
  $gi-muted:    #7b8798;
  $gi-readonly: #f1f3f7;

  .scroll-content {
    background: $gi-bg;
    padding-bottom: 12px;
  }

  // ---------- แถบสรุปเอกสาร ----------
  .summary {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin: 12px 12px 0;
    padding: 10px 14px;
    background: $gi-primary;
    border-radius: 12px;
    color: #fff;

    .summary-label { font-size: 1.1rem; opacity: .8; }
    .summary-value { font-size: 1.8rem; font-weight: 700; word-break: break-all; }
    .summary-value.small { font-size: 1.4rem; font-weight: 600; }
    .summary-side { text-align: right; margin-left: 12px; }
  }

  // ---------- การ์ด ----------
  .card {
    margin: 12px;
    padding: 14px;
    background: $gi-card;
    border-radius: 14px;
    box-shadow: 0 1px 4px rgba(20, 30, 50, .08);

    .card-title {
      display: flex;
      align-items: center;
      margin-bottom: 12px;
      padding-bottom: 10px;
      border-bottom: 1px solid #edf0f5;
      font-size: 1.6rem;
      font-weight: 700;
      color: $gi-text;

      ion-icon { margin-right: 8px; font-size: 2rem; color: $gi-primary; }
      .push { margin-left: auto; }
    }
  }

  // ---------- ฟอร์ม ----------
  .field { margin-bottom: 12px; flex: 1; min-width: 0; }

  .field-label {
    margin-bottom: 4px;
    font-size: 1.2rem;
    font-weight: 600;
    color: $gi-muted;
  }

  .field-row {
    display: flex;
    align-items: stretch;

    ion-input.box { flex: 1; min-width: 0; }
    .side-btn {
      margin: 0 0 0 8px;
      height: 44px;
      border-radius: 10px;
      font-size: 1.3rem;
      box-shadow: none;
    }
  }

  .pair {
    display: flex;
    align-items: flex-start;

    .field + .field { margin-left: 8px; }
    .times {
      width: 18px;
      margin-top: 30px;
      text-align: center;
      color: $gi-muted;
      font-size: 1.6rem;
    }
    .times + .field { margin-left: 0; }
  }

  ion-input.box,
  ion-item.box {
    min-height: 44px;
    background: #fff;
    border: 1px solid $gi-border;
    border-radius: 10px;
    padding: 0;
  }

  ion-input.box .text-input {
    margin: 0;
    padding: 0 12px;
    height: 42px;
    font-size: 1.5rem;
    color: $gi-text;
  }

  ion-input.box.readonly {
    background: $gi-readonly;
    .text-input { color: #5a6678; }
  }

  ion-input.box.strong {
    border-color: $gi-primary;
    .text-input { font-weight: 700; }
  }

  ion-item.box {
    .item-inner { border-bottom: 0 !important; padding-right: 0; min-height: 42px; }
    ion-select { max-width: 100%; padding: 0 12px; font-size: 1.5rem; }
  }

  // ---------- ปุ่มด้านล่างของแต่ละแท็บ ----------
  .action-bar {
    display: flex;
    margin: 12px;

    &.top { margin-bottom: 0; }

    button { flex: 1; height: 46px; border-radius: 12px; font-size: 1.5rem; margin: 0; }
    button + button { margin-left: 10px; }
    .btn-ghost { flex: 0 0 34%; }
    .btn-main  { flex: 1; font-weight: 700; }
  }

  .btn-main-block {
    height: 46px;
    margin: 12px 0 0;
    border-radius: 12px;
    font-size: 1.5rem;
    font-weight: 700;
  }

  // ---------- chip ----------
  .chip {
    display: inline-block;
    min-width: 22px;
    padding: 1px 8px;
    border-radius: 11px;
    background: #e8eefc;
    color: $gi-primary;
    font-size: 1.2rem;
    font-weight: 700;
    text-align: center;
  }

  // ---------- JO ----------
  .select-all {
    display: flex;
    align-items: center;
    margin: 4px 0 8px;
    font-size: 1.4rem;
    font-weight: 600;
    color: $gi-text;

    ion-checkbox { margin: 0 10px 0 2px; }
    .chip { margin-left: 8px; }
  }

  .jo-row {
    display: flex;
    align-items: center;
    margin-bottom: 8px;
    padding: 8px 10px;
    background: #fafbfd;
    border: 1px solid #edf0f5;
    border-radius: 12px;

    &.checked { background: #eef4ff; border-color: $gi-primary; }

    ion-checkbox { margin: 0 12px 0 2px; }
    .info { flex: 1; min-width: 0; }

    .jo-qty {
      display: flex;
      align-items: center;
      margin-left: 8px;

      ion-input.box { width: 84px; min-height: 40px; }
      ion-input.box .text-input { height: 38px; text-align: right; }
      .unit { margin-left: 6px; font-size: 1.2rem; color: $gi-muted; }
    }
  }

  // ข้อความ 2 บรรทัดที่ใช้ร่วมกัน
  .info {
    .t { font-size: 1.5rem; font-weight: 700; color: $gi-text; word-break: break-all; }
    .s { font-size: 1.2rem; color: $gi-muted; }
  }

  // ---------- ตัวเลือก JO ----------
  .jo-hint {
    margin: -6px 0 12px;
    font-size: 1.2rem;
    color: $gi-muted;
  }

  // ---------- Draft ----------
  .draft-row {
    display: flex;
    align-items: center;
    padding: 8px 0;
    border-bottom: 1px solid #f0f2f6;

    &:last-child { border-bottom: 0; }

    .idx {
      width: 28px;
      height: 28px;
      line-height: 28px;
      margin-right: 10px;
      border-radius: 50%;
      background: #eef1f6;
      color: $gi-muted;
      font-size: 1.2rem;
      font-weight: 700;
      text-align: center;
    }
    .info { flex: 1; min-width: 0; }
    button { margin: 0; }
  }

  // ---------- Detail ----------
  .empty {
    margin: 40px 20px;
    text-align: center;
    color: $gi-muted;
    font-size: 1.4rem;

    ion-icon { display: block; margin-bottom: 8px; font-size: 5rem; opacity: .5; }
  }

  .line-card {
    margin: 12px;
    background: $gi-card;
    border-radius: 14px;
    box-shadow: 0 1px 4px rgba(20, 30, 50, .08);
    overflow: hidden;

    .line-head {
      display: flex;
      align-items: center;
      padding: 12px 14px;
      background: #f7f9fc;
      border-bottom: 1px solid #edf0f5;

      .badge {
        min-width: 30px;
        height: 30px;
        line-height: 30px;
        margin-right: 12px;
        border-radius: 15px;
        background: $gi-primary;
        color: #fff;
        font-size: 1.3rem;
        font-weight: 700;
        text-align: center;
      }
      .info { flex: 1; min-width: 0; }
      .status {
        margin-left: 8px;
        padding: 2px 10px;
        border-radius: 10px;
        background: #e6f6ec;
        color: #1e8a4c;
        font-size: 1.1rem;
        font-weight: 700;
      }
    }

    .line-body { padding: 14px; }

    .group-label {
      margin: 2px 0 6px;
      font-size: 1.3rem;
      font-weight: 700;
      color: $gi-text;
    }

    .result {
      margin-bottom: 12px;
      padding: 10px;
      background: #f4f8ff;
      border: 1px dashed $gi-primary;
      border-radius: 12px;

      .result-top {
        display: flex;
        align-items: flex-end;

        .field { margin-bottom: 0; }
        .calc-btn {
          flex: 0 0 auto;
          margin: 0 0 0 10px;
          height: 44px;
          border-radius: 10px;
          font-size: 1.4rem;
        }
      }
      .hint { margin-top: 6px; font-size: 1.1rem; color: $gi-muted; }
    }

    .line-actions {
      display: flex;

      button { margin: 0; height: 44px; border-radius: 12px; font-size: 1.5rem; }
      .btn-icon { flex: 0 0 56px; }
      .btn-main { flex: 1; margin-left: 10px; font-weight: 700; }
    }
  }

  // ---------- แท็บด้านล่าง ----------
  ion-footer {
    ion-toolbar { padding: 0; min-height: 56px; }
    .toolbar-background { background: #fff; border-top: 1px solid $gi-border; }
  }

  .bottom-tabs {
    width: 100%;
    margin: 0;
    background: transparent;

    ion-segment-button {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 56px;
      min-height: 56px;
      margin: 0;
      border: 0 !important;
      border-bottom: 3px solid transparent !important;
      border-radius: 0 !important;
      background: transparent !important;
      color: $gi-muted;
      font-size: 1.1rem;
      text-transform: none;

      ion-icon { font-size: 2.2rem; line-height: 1; margin-bottom: 2px; }
      span { line-height: 1.2; }

      &.segment-activated {
        color: $gi-primary;
        border-bottom-color: $gi-primary !important;
        font-weight: 700;
      }
    }
  }
}