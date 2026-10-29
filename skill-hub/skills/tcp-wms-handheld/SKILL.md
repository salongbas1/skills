---
name: tcp-wms-handheld
description: ความรู้เกี่ยวกับโปรเจกต์ "TCP Handheld" (TCP_WMS) แอปมือถือ/แฮนด์เฮลด์สำหรับคลังสินค้า (WMS) เขียนด้วย Ionic 3 + Angular 5 + Cordova (Android) พร้อมโค้ด backend ASP.NET ASMX ฝั่ง server (RF-Service_TCP: RFService.asmx / RFService.cs, stored procedure SQL Server, upload.aspx). Use this skill whenever the user works on, debugs, extends, builds or documents this app — e.g. adding a page/menu, calling/adding/debugging an RFService endpoint (client or server C# side), stored procedure mapping, upload.aspx, login/license/server-URL settings, receiving (Check-in), Putaway, Pick by Order/Task, Stock Count, Relocation, Replenishment, Load to Truck, Goods Issue, Branch operations, i18n (th/en/la), or building the APK. Trigger on keywords TCP, TCP_WMS, TCP Handheld, RFService, RF-Service_TCP, RFService.cs, Get_Menu_HH, rf_ stored procedure, Ionic WMS handheld, io.ionic.anj.TCP.
---

# TCP Handheld (TCP_WMS)

แอปแฮนด์เฮลด์สำหรับงานคลังสินค้า (DC) และงานสาขา (Branch) ใช้สแกนบาร์โค้ดเพื่อรับสินค้า เก็บเข้าที่ หยิบ นับสต็อก ย้ายตำแหน่ง เติมสินค้า โหลดขึ้นรถ ฯลฯ
ตัวแอปเป็น client บางๆ ที่เรียก web service เป็นหลัก ตรรกะธุรกิจส่วนใหญ่อยู่ฝั่ง server (stored procedure ผ่าน `RFService.asmx`)

## Stack และข้อมูลโปรเจกต์

| หัวข้อ | ค่า |
|---|---|
| Package / Widget id | `TCP_WMS` / `io.ionic.anj.TCP` ชื่อแอป "TCP Handheld" เวอร์ชัน 0.0.1 |
| Framework | Ionic 3.9.5 (`ionic-angular`), Angular 5.2.11, TypeScript ~2.6, RxJS 5.5 |
| Native | Cordova (cordova-android ^8.1.0), minSdk 19, ล็อกจอแนวตั้งที่หน้า Login |
| Plugin หลัก | barcodescanner, camera, network-information, app-version, device, datepicker, browsertab, screen-orientation, ionic-keyboard, ionic-webview |
| Lib อื่น | `@ionic/storage` (key-value), `@ngx-translate` (i18n), `xml2js` (parse response), `ionic-select-searchable` |
| Platform ใน config | `android`, `browser` |

คำสั่งใน `package.json`:

```bash
npm run dev     # ionic s  (ทดสอบบน browser)
npm run build   # ionic-app-scripts build
npm run apk     # ionic cordova build android
```

ข้อควรระวังการ build: Ionic 3 + Angular 5 เก่ามาก มักต้องใช้ Node เวอร์ชันเก่า และไม่ควรอัปเกรด dependency เดี่ยวๆ
`package.json` มี `@ionic/angular ^5` ปนอยู่กับ `ionic-angular 3.9.5` เป็นของตกค้าง ไม่ได้ใช้จริงในโค้ด

## โครงสร้างโค้ด (`src/`)

```
src/app/            app.component.ts (root = LoginPage, side menu), app.module.ts, main.ts
src/pages/<name>/   หน้าแต่ละฟีเจอร์: <name>.ts / .html / .scss / .module.ts (lazy-loaded @IonicPage)
src/pages/modal/    ~54 modal (เลือก location, item, customer, driver, equipment, pallet, summary ฯลฯ)
src/services/       service.ts (~11.8k บรรทัด, ทุก API call), jobsservice, listjobservice,
                    confdelijobdropservice, uploadpictureservice, global.ts
src/helper/         utility.ts (Alert, Toast, Loading), native.ts
src/assets/i18n/    th.json (ค่าเริ่มต้น), en.json, la.json
src/assets/imgs/menu/  ไอคอนเมนู (ชื่อไฟล์ต้องตรงกับ `img` ใน menu_list)
```

ทุกหน้าใช้รูปแบบเดียวกัน:

```ts
@IonicPage({ name: "PutawayPage", segment: "Putaway" })
@Component({ selector: "page-putaway", templateUrl: "putaway.html" })
export class PutawayPage { constructor(..., private service: Service, private storage: Storage) {} }
```

เปิดหน้าด้วย `this.navCtrl.push("PutawayPage")` (เป็นสตริง ไม่ import class เพราะ lazy-load)
helper ที่แทบทุกหน้ามี: `Alert(title, subTitle)`, `presentToast(msg, showClose, position)`, `presentLoading()/finishLoding()` (สะกดตามโค้ดจริง: `finishLoding`)

## การเชื่อมต่อ Backend

### Base URL
ตั้งค่าที่หน้า **Setting** (เปิดจากหน้า Login) เก็บใน `@ionic/storage`:

- `_url` = host[:port] ของ server
- `_type` = `PROD` หรือ `DEV` (ค่าอื่น/ว่างใช้เหมือน PROD)
- `_license` = license key ของอุปกรณ์

`Service` ประกอบ URL ดังนี้:

```
http://{_url}/{serviceName}/RFService.asmx        # API
http://{_url}/{serviceName}/upload.aspx?          # อัปโหลดรูป
serviceName = PROD -> "RF-Service_TCP",  DEV -> "RF-Service_TCP_DEV"
```

`Service.setUrl({url,type})` ใช้เปลี่ยนค่า runtime หลังบันทึก Setting
นอกจากนี้มี `hostWebService1` (REST `/api/po`, ใช้กับ `doLoginPost/doLoginGet`) ฝัง IP วง LAN ไว้ในโค้ด และ `services/global.ts` ก็ฝัง host เก่าไว้ ทั้งสองส่วนแทบไม่ได้ใช้ ห้ามเอาไปใช้เป็นแบบอย่างโดยไม่ตรวจก่อน
ทราฟฟิกเป็น HTTP แบบ cleartext (ไม่มี TLS)

### รูปแบบ response (สำคัญ)
endpoint เป็น ASMX คืน XML ของ .NET `DataTable` (diffgram) แอป parse ด้วย `xml2js` แล้วดึง `Table`:

```ts
xml2js.parseString(response.text(), { explicitArray: true }, (err, result) => { a = result; });
return a.DataTable["diffgr:diffgram"]["0"].NewDataSet["0"].Table;   // array ของ row
// บางฟังก์ชันคืน .Table["0"] (row เดียว); ถ้า parse พัง catch แล้วคืน []
```

- ทุก field ใน row เป็น **array** ต้องอ่านด้วย `row.field[0]` (เช่น `res["0"].Warehouse[0]`)
- row สถานะมาตรฐาน: `sqlstatus` (`"0"` = สำเร็จ ค่าอื่น = ผิดพลาด) และ `sqlmessage` (ข้อความที่แสดงให้ผู้ใช้)
- ส่วนใหญ่ GET + query string (`oUsername=...&oPassword=...` ชื่อพารามิเตอร์ขึ้นต้น `o`) ส่วน `permission_using` และบางตัวใช้ POST แบบ `application/x-www-form-urlencoded`

### แม่แบบการเพิ่ม endpoint ใหม่ใน `service.ts`

```ts
get_xxx(oClient, oParam) {
  const parameters = "oClient=" + oClient + "&oParam=" + oParam;
  return this.http
    .get(this.hostWebService + "/Get_Xxx?" + parameters)
    .toPromise()
    .then((response) => {
      let a;
      xml2js.parseString(response.text(), { explicitArray: true }, (err, result) => { a = result; });
      try { return a.DataTable["diffgr:diffgram"]["0"].NewDataSet["0"].Table; }
      catch (e) { return []; }
    });
}
```

ชื่อ endpoint ฝั่ง server ต้องมีอยู่ใน `RFService.asmx` จริง (แอปเรียกใช้ ~310 ตัว ฝั่ง server มี ~500 `[WebMethod]`) โค้ดฝั่ง server (`RF-Service_TCP`) สรุปไว้ใน `references/rfservice-server.md` และดัชนี endpoint ใน `references/rfservice-endpoints.md`

## Flow การเริ่มต้นและ Login

1. `app.component` ตั้ง `rootPage = LoginPage`, ลบ `_user` ใน storage ทุกครั้งที่เปิดแอป (ต้อง login ใหม่ทุกครั้ง), ภาษาเริ่มต้น `th` (เก็บใน `_language`)
2. หน้า Login อ่าน `_type`, หา `device.uuid` (บน browser ใช้ค่า fallback `developer3`) แล้วเรียก `permission_using(uuid, licenseKey)` เพื่อตรวจสิทธิ์อุปกรณ์/license
3. ตรวจเวอร์ชันด้วย `CheckVersionHH` (เทียบกับ `AppVersion`) ถ้าเก่าจะเปิด `http://{ip}/web_apk` ด้วย BrowserTab ให้ดาวน์โหลด APK ใหม่
4. `Check_Login(oUsername, oPassword)` สำเร็จ -> เก็บ `_warehouse` (จาก `get_WarehouseByUser`), `_client` (จาก `get_client_by_user`), `_user` -> `setRoot("HomePage")`
5. `HomePage` เรียก `Get_User` แล้วเก็บ `_user_Group`, `_user_Cus_name`, `_user_Cus_code`, `_branch_code`
6. เมนูข้างมี Home / DC Operation / Branch Operation / Logout (Logout ล้าง key ใน storage)

storage key ที่พบบ่อย: `_user`, `_url`, `_type`, `_license`, `_warehouse`, `_client`, `_user_Group`, `_user_Cus_code`, `_user_Cus_name`, `_branch_code`, `_language`, และ key ชั่วคราวของงาน (`_oItem`, `_oReceipt`, `_oLine`, `_Wh`, `_RecNum`, `_PalleNO`, `_Qty`, `_oReversePallet`, `_saleorder`)

## เมนู DC Operation (`OperationPage`)

เมนูโหลดจาก server: `Get_Menu_HH` คืนรายการ `{access_level, raw_name}` แล้วแอปกรอง `access_level` 100 และ 101 ออก
`menu_list` ใน `operation.ts` แมป `access_level` -> ไอคอน + ฟังก์ชันเปิดหน้า (ตรวจ `Network.type !== "none"` ก่อน push)

| access_level | หน้า (`navCtrl.push`) | ไอคอน |
|---|---|---|
| 1 | AddDimentionPage | Stock-count |
| 2 | CheckinPage (รับสินค้าตาม PO/ASN) | checkbox-pen-outline |
| 3 | ReceiptOtherPage | checkbox-pen-outline |
| 4 | IteminvquePage (สอบถามสต็อกตามสินค้า) | Item-Inventory-query |
| 5 | ItemrelocationPage | Item-Relocation |
| 6 | LocationinvquePage (สอบถามสต็อกตามตำแหน่ง) | Location-inventory-query |
| 7 | LocationrelocationPage | Location-relocation |
| 8 | PalletrelocationPage | Pallet-Relocation |
| 9 | PickbyorderPage | icon-pick-by-order |
| 10 | PutawayPage | icon-forklift |
| 11 | StockadjustPage | Adjust |
| 12 | StockcountPage | Stock-count |
| 13 | PickbyTransferRetailPage | Adjust |
| 14 | GoodsIssuePage | Goods Issue |

หน้าอื่นที่มีในโปรเจกต์แต่ถูกเรียกจากหน้าอื่น/ฟังก์ชันอื่นใน `operation.ts` (ไม่อยู่ใน `menu_list` ปัจจุบัน):
PutawayByPalletPage, PutawayByItemPage, PickbytaskPage, PickbytasknewPage, PickOrderSummaryPage, PickbytaskTransferPage, ListJobsPage, ReplenishmentHeaderPage, LoadtotruckPage, TemplatestockcountPage, ReceiptReturnPage (`oBook` "01"/"02"), CheckpalletPage, SaleReturnHeaderPage, ReceiptCheckinBranchPage, CheckinOrderPage, ReservationOrderPage, CheckProductPage, DirecttostorePage, StockcountbranchPage, StockcountboxdetailPage, PreferLocationPage, OrderTransferPage, DeliveryConfirmPage

**Branch Operation** (`OperationBranchPage`): SaleReturnHeader, CheckinOrder, ReservationOrder, CheckProduct, Stockcount, Replenishment

## ฟีเจอร์หลักและ endpoint ที่เกี่ยวข้อง

(สรุปจาก `this.service.*` ในแต่ละหน้า ใช้เป็นจุดเริ่มค้นโค้ด ไม่ใช่สเปกครบถ้วน)

- **Check-in / รับสินค้า** (`checkin`, ~2.8k บรรทัด): ค้น PO (`get_PO`, `get_po_has_asn`), ตรวจบาร์โค้ด (`Get_Check_Barcode_From_Po`/`_From_ASN`), UOM (`get_UOM`), วันหมดอายุ (`Get_ExpiryDateByItem`), ตรวจ/อนุมัติ shelf-life (`checkShelflife`, `checkApprShelflife`, `checkApproverShelflife`, `approveShelflife`), สร้างพาเลทใหม่ (`get_new_pallet`), พิมพ์ฉลาก/เอกสาร (`ExportPDFDoc`), ยืนยันรับ (`receipt_checkin`, `receipt_checkin_by_pallet`), ยกเลิก (`Undo_Receipt`, `Undo_Receipt_by_line`, `Reverse_Receipt_Pallet`)
- **รับคืน / รับอื่นๆ / Branch**: `receiptreturn`, `receiptother`, `receiptcheckinbranch`, `checkinorder`, `directtostore`
- **Putaway** (`putaway`, `putawaybypallet`, `putawaybyitem`): สแกนพาเลท `get_pallet_for_putaway` -> สแกน/ยืนยัน location -> `putaway`; ตรวจพาเลทใน location `check_pallet_in_location`
- **Pick**: `pickbyorder` (`assign_Manual_Pick_by_Order_new`, `get_WO_Pick_by_Order_Detail`, `Confirm_Works_Order_by_Pallet`, `Pick_Sales_Detail`), `pickbytask`/`pickbytasknew` (`Get_WO_Pick_by_Task_New`, `Confirm_Works_Order_by_Task_New`), `pickordersummary`, `pickbytasktransfer`, `pickbytransferretailtask`; ยกเลิกการหยิบ `Reverse_Trx_*`
- **Relocation**: `itemrelocation` (`Item_Relocation`), `locationrelocation` (`ConfirmLocationRelocation`), `palletrelocation` (`ConfirmPalletRelocation`)
- **Inventory query**: `iteminvque`, `locationinvque`, `stocklist` (`Show_StockList_Item/_Location`)
- **Stock count / adjust**: `stockcount` (`StockCountListLocation`, `Update_Stock_Count*`, `SummaryStockCount`), `templatestockcount` (`Save_Template_Stock_Count_Header`, `get_template_stock_count_detail`), `stockcountbranch`, `stockcountbox*`, `stockadjust` (ตรวจสิทธิ์ `check_adjust_permission`, `Get_Stock_For_Stockadjus`)
- **Replenishment**: `replenishmentheader`, `replenishment`, `replenishment-by-pickorder` (`Get_Pallet_For_Replenishment`, `Pick_Replenishment`, `Closed_Replenishment(_New)`)
- **Load to Truck / Packing / Delivery**: `loadtotruck` (`Save_Loading_Summary_Header/Detail`, `Confirm_Loading_Summary_Detail`, lookup รถ/คนขับ), `checkpallet` (`Packing_Detail_*`), `sumpallet`, `deliveryconfirm`, `list-job`/`jobs*` (งานส่ง + ถ่ายรูป ผ่าน `upload.aspx`)
- **Goods Issue**: `goodissue` (`insert_TempGoodsIssue`, `create_GoodsIssue_Header/Lines`, `get_OrderList_GoodsIssue`)
- **Sale return / Reservation (Branch)**: `salereturnheader`, `salereturn`, `reservationorder`
- **อื่นๆ**: `adddimention` (บันทึกบาร์โค้ด/มิติสินค้า), `preferlocation` (ตำแหน่งที่ควรเก็บ), `checkproduct` (เช็คราคา/ป้ายสินค้า)

การสแกน: แอปออกแบบให้ใช้สแกนเนอร์ฮาร์ดแวร์ของเครื่องแฮนด์เฮลด์แบบ "พิมพ์เข้าช่อง" ช่องกรอกผูก `onKeyup` (จับ Enter) และ `barcodeOnClick()` ทำหน้าที่ซ่อนคีย์บอร์ดแล้ว `setFocus()` ไปที่ช่องสแกน (ไม่ได้เปิดกล้อง) ส่วน `phonegap-plugin-barcodescanner` (กล้อง) ติดตั้งไว้ในโปรเจกต์ ตรวจแต่ละหน้าก่อนสมมติว่าใช้กล้อง

## i18n

- ใช้ `@ngx-translate` โหลด `./assets/i18n/{th,en,la}.json` ภาษาเริ่มต้น `th` เปลี่ยนด้วย `changeLanguage()` (เก็บ `_language`)
- ชื่อเมนูมาจาก `menu.raw_name` และ `menu.access_level` ที่ server ส่งมา แล้วแปลด้วย `| translate` จึงต้องมี key ตรงกันใน json ทุกภาษา
- ธรรมเนียม key: `dc_*` (ชื่อเมนู DC), `btn_*` (ปุ่ม), key ทั่วไปเป็น snake_case
- ข้อความ Alert/Toast หลายจุดเป็นภาษาไทย hard-code ในโค้ด (ไม่ผ่าน i18n)

## สูตรงานที่ทำบ่อย

**เพิ่มหน้า/เมนูใหม่**
1. สร้าง `src/pages/<name>/` (`.ts`, `.html`, `.scss`, `.module.ts`) ใช้ `@IonicPage({ name: "<Name>Page", segment: "<Name>" })` และ `IonicPageModule.forChild(...)` ตามหน้าที่มีอยู่ (คัดลอกจากหน้าที่ใกล้เคียง เช่น `putaway`)
2. เพิ่ม method ใน `service.ts` ตามแม่แบบด้านบน (และต้องมี endpoint ฝั่ง server)
3. ถ้าเป็นเมนู DC: เพิ่มรายการใน `menu_list` ของ `operation.ts` (key = `access_level` ใหม่, `img`, `fn`) + เมธอด `doXxx()` ที่เช็คเครือข่ายก่อน push + ไฟล์ไอคอนใน `assets/imgs/menu/` + เพิ่มแถวเมนูใน `Get_Menu_HH` ฝั่ง server และ key แปลใน `th/en/la.json`
4. ถ้าเป็นเมนู Branch: เพิ่มใน `operationBranch`

**แก้ปัญหา "ไม่มีข้อมูล/ค่าว่าง"**: เปิด response ดิบของ endpoint ตรวจโครงสร้าง `DataTable -> diffgr:diffgram -> NewDataSet -> Table` แล้วดูว่าฟังก์ชันคืน `.Table` หรือ `.Table["0"]` และ page อ่านเป็น `res[0].field[0]` ตรงกันหรือไม่ (catch จะกลืน error แล้วคืน `[]` เงียบๆ)

**ทดสอบบน browser**: `npm run dev` ได้ แต่ปลั๊กอิน native (สแกนเนอร์, device uuid, network) ไม่ทำงาน device uuid จะ fallback เป็น `developer3` ตรวจ CORS ของ server ด้วย

## ข้อสังเกต / ความเสี่ยงที่พบในโค้ดปัจจุบัน

1. **ความปลอดภัย**: ไฟล์ `readme.txt` ในโปรเจกต์มีข้อมูลเข้าถึงระยะไกล (AnyDesk ID + รหัสผ่าน) เขียนเป็นข้อความธรรมดา ควรลบออกจากรีโป/ประวัติ git และเปลี่ยนรหัสผ่านนั้นทันที (skill นี้ตั้งใจไม่คัดลอกค่าเหล่านั้น)
2. **เมนูไม่ผูกกับผู้ใช้ที่ login**: `OperationPage` ตั้ง `oUsername` ค่าคงที่แล้วส่งไป `Get_Menu_HH` (โค้ดอ่านจาก `_user` ถูกคอมเมนต์ไว้) ทำให้ทุกคนเห็นเมนูชุดเดียวกัน ถ้าต้องการสิทธิ์รายผู้ใช้ให้เปิดโค้ดที่คอมเมนต์ไว้และทดสอบกับ server
3. **รหัสผ่านส่งทาง query string ผ่าน HTTP** (`Check_Login?oUsername=..&oPassword=..`) ควรย้ายเป็น POST + HTTPS ในอนาคต
4. `service.ts` ยาวเกือบ 12k บรรทัดและซ้ำรูปแบบเดิมเยอะ เวลาแก้ให้ค้นชื่อ endpoint ก่อน (`grep -n '"/Get_Xxx'`) และแก้เฉพาะจุด หลีกเลี่ยงการ refactor ใหญ่ที่ไม่ได้ขอ
5. โฟลเดอร์ `platforms/` และ `.sourcemaps/` เป็นผล build (มี APK ~12 MB) ไม่ควร commit/อัปโหลดเป็นส่วนของซอร์ส
6. มีไฟล์ `stockcount.rar` / `stockcount2.rar` ค้างอยู่ใน `src/pages/` ควรย้ายออก

## แนวทางตอบเมื่อใช้ skill นี้

- ก่อนแก้โค้ด ให้ตรวจว่าเป็นฝั่ง app (`src/` Ionic) หรือฝั่ง server (`RF-Service_TCP`, C#/ASMX) ถ้าซอร์สของฝั่งนั้นไม่ได้อยู่ในงาน ให้สรุปสัญญา endpoint (ชื่อ, พารามิเตอร์, คอลัมน์ที่คืน) ให้ทีมที่ดูแลฝั่งนั้น
- เมื่อทำงานกับ endpoint: เปิด `references/rfservice-endpoints.md` หาชื่อ/พารามิเตอร์/SP ก่อน แล้วอ่านเมธอดจริงใน `RFService.cs`; เรื่องสถาปัตยกรรม, การเพิ่ม endpoint ฝั่ง server และความเสี่ยง ดู `references/rfservice-server.md`
- รักษารูปแบบเดิม (Promise + xml2js, `explicitArray`, `sqlstatus`) และใช้ helper `Alert/presentToast/presentLoading` ที่มีอยู่
- ห้ามใส่ข้อมูลลับ (รหัสผ่าน, license key, IP/พอร์ตของ DB หรือเซิร์ฟเวอร์จริง, ค่าใน `App_Data/Connection*.xml`) ลงในโค้ดตัวอย่างหรือเอกสารใหม่

## เอกสารอ้างอิงฝั่ง server (โหลดเมื่อต้องใช้)

- `references/rfservice-server.md` — โครงสร้างโปรเจกต์ RF-Service_TCP, รูปแบบเมธอด (SP vs inline SQL), Login/สิทธิ์, upload.aspx, สูตรเพิ่ม/ดีบัก endpoint, ความเสี่ยงที่พบ
- `references/rfservice-endpoints.md` — ดัชนี ~500 endpoint แยกตามหมวด (ชื่อ, พารามิเตอร์, stored procedure/ตารางที่ใช้, ⚠ = เสี่ยง SQL injection)
