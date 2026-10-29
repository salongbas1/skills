# RFService (ฝั่ง Server) — สถาปัตยกรรมและข้อควรระวัง

โปรเจกต์ `RF-Service_TCP` = web service ASP.NET (Web Site, .NET Framework 3.5/4.x, C#) ที่แอป TCP Handheld เรียกผ่าน `RFService.asmx`
ตรรกะธุรกิจส่วนใหญ่อยู่ใน **stored procedure ของ SQL Server** (ฐานข้อมูลตระกูล `similanwms_*`) เมธอด C# เป็นแค่ตัวส่งต่อ

## โครงสร้างโฟลเดอร์

```
RFService.asmx            <%@ WebService CodeBehind="~/App_Code/RFService.cs" Class="RFService" %>
App_Code/RFService.cs     ~19.3k บรรทัด, ~500 [WebMethod] ทั้งหมดอยู่ในคลาส RFService ไฟล์เดียว
App_Code/cp_ioc/          SQLServer.cs (RunStoredProcedure, spArgumentsCollection), SQLWrapper.cs, iDatabase.cs, LogFile.cs, xmlProperties.cs
App_Code/cp_qurery/       SqlQuery.cs (selQuery / insQuery สำหรับ inline SQL)
App_Code/cp_encode/       Encode.cs (hashPassword: MD5/MD5I/SHA*), Logger.cs
App_Data/Connection*.xml  ค่าเชื่อมต่อ DB ต่อสภาพแวดล้อม (Connection.xml = ที่ใช้จริง, -kcsDEV, -kcspure) — มีรหัสผ่าน ห้ามคัดลอก/commit
upload.aspx(.cs)          รับรูป/ลายเซ็นของงานส่ง (JSON base64) -> ตาราง delivery_picture
hhupdate/                 ไฟล์อัปเดตแอป (.similan) ของระบบเดิม
Web.config                customErrors=Off, เปิด HttpGet+HttpPost, compilation debug=true, executionTimeout สูง, maxRequestLength ~1GB
Backup/, UpgradeLog.htm, *.rar, *.onetoc2, RegKey.cs   ของตกค้าง ไม่เกี่ยวกับการทำงานปัจจุบัน
```

## การเลือก Database
`RFService()` constructor เรียก `setDBConnection()` อ่าน `~/App_Data/Connection.xml` (Server, DatabaseName, Login, Password, Reports, Files)
แล้วสร้าง `strConnection` — **การสลับ DEV/PROD ทำที่ระดับ virtual directory** (`RF-Service_TCP` กับ `RF-Service_TCP_DEV` เป็นคนละ deployment ที่มี `Connection.xml` ต่างกัน) ไม่ใช่ที่โค้ด สอดคล้องกับฝั่งแอปที่ต่อ `serviceName` ตาม `_type`

## รูปแบบเมธอด 2 แบบ

**A. Stored procedure (≈70%)**

```csharp
[WebMethod]
public DataTable Putaway(string oPallet, string oWarehouse, string oLocation, string oOperation)
{
    try {
        arLstParameter = new ArrayList();
        dsReturn = new DataSet();
        SQLServer.spArgumentsCollection(arLstParameter, "@pallet", oPallet, "NVARCHAR");
        // ... พารามิเตอร์อื่น
        return SQLServer.RunStoredProcedure(strConnection, dsReturn, "rf_putaway_stock_movements_pallet", arLstParameter).Tables["Table"];
    }
    catch (Exception ex) { throw new Exception(ex.Message); }
    finally { arLstParameter = null; dsReturn = null; }
}
```

**B. Inline SQL (≈30%)** — `sqlText = "..." + oParam + "..."` แล้ว `objSqlQuery.setConnection(strConnection); objSqlQuery.selQuery(sqlText).Tables["Table"]`
ส่วนใหญ่เป็น lookup (`Get_*`) และมี ~130 จุดที่ต่อสตริงจากพารามิเตอร์ตรงๆ (ดูสัญลักษณ์ ⚠ ใน `rfservice-endpoints.md`)

- ชนิดคืนส่วนใหญ่ `DataTable` (471 จาก 498); ที่เหลือ `bool`/`string`/`void`/`byte[]` (เช่น `Get_Run_Number`, `ExportPDFDoc`, `DownloadFile`)
- `DataTable` ถูก serialize เป็น XML diffgram -> ตรงกับที่แอป parse (`DataTable → diffgr:diffgram → NewDataSet → Table`)
- ชื่อตารางผลลัพธ์ต้องเป็น `"Table"` (`.Tables["Table"]`) ถ้าเปลี่ยนจะทำให้แอปอ่านไม่เจอ
- error: `catch` แล้ว `throw new Exception(ex.Message)` -> แอปได้ SOAP fault / HTTP 500 ไม่ใช่แถว `sqlstatus`
- สัญญาสถานะมาตรฐานของ SP: คืนแถวที่มีคอลัมน์ `sqlstatus` (`"0"` = สำเร็จ) และ `sqlmessage`
- `CommandTimeout` ของ SP ตั้ง 100000 วินาที (ไม่มี timeout จริง) ระวังการล็อกค้าง

## ชื่อพารามิเตอร์
ส่วนใหญ่ขึ้นต้น `o` (`oClient`, `oUsername`, `oPallet`) แต่มีข้อยกเว้น (`user`, `warehouse`, `client`, `item`, `receipt`) — **ต้องตรงตัวสะกดและตัวพิมพ์กับ signature ใน `RFService.cs`** เวลาเรียกผ่าน GET query string
ASMX ไม่แยกพารามิเตอร์ที่หายไป (ได้ค่า null) ให้ตรวจชื่อก่อนเสมอ

## Login / สิทธิ์
- `Check_Login(oUsername, oPassword)`: hash รหัสผ่านด้วย `Encode.hashPassword("MD5I", ...)` (**MD5 ไม่ใส่ salt**) แล้วเรียก SP `check_login`
- `permission_using(oUUID, oLicenseKey)` -> SP `rf_permission_using` ตรวจอุปกรณ์/license
- `CheckVersionHH(oVersion)` -> SP `check_version_HH`; `Get_Version` คืนค่าคงที่ที่ hard-code ในโค้ด (`1.2.2`) ไม่ใช่เวอร์ชันแอป
- `Get_Menu_HH(oUsername)` -> SP `get_menu_hh` (ผลคือ `access_level`, `raw_name` ที่แอปใช้สร้างเมนู DC)
- ไม่มี token/session ฝั่ง server: ทุก endpoint เปิดให้เรียกได้โดยไม่ยืนยันตัวตน (เปิด HttpGet/HttpPost) ความปลอดภัยพึ่งเครือข่ายภายในเท่านั้น

## upload.aspx (รูป + ลายเซ็นงานส่ง)
- รับพารามิเตอร์ใน query string: `oClient, oDelivery, oPackNo, oPicture_no, oRemark` และ body เป็น JSON array ของ object ที่มี `pic1` (base64) — ตัวที่ 0 = รูป, ตัวที่ 1 = ลายเซ็น
- ใช้ SqlCommand แบบ parameterized: ถ้ามีแถวอยู่แล้ว (client+delivery+picture_no) จะ UPDATE ไม่เช่นนั้น INSERT ลง `delivery_picture`
- **เขียน log ทุกค่ารวมทั้งข้อมูล base64 เต็มๆ ลงไฟล์ `C:\LogFile\ErrorLog.txt`** (ไฟล์โตเร็ว ควรปิดหรือจำกัด)

## ไฟล์/รายงาน
- Crystal Reports (`CrystalDecisions.*` 10.5) ใช้ใน `ExportPDFDoc` / เมธอด print; path มาจาก `<Reports>` / `<Files>` ใน `Connection.xml` (ไดรฟ์ D: ของเซิร์ฟเวอร์เดิม)
- โฟลเดอร์ `PDF/` ว่างในรีโป — ไฟล์ที่สร้างจะลงที่ `<Files>` บนเซิร์ฟเวอร์จริง

## สูตรงานฝั่ง server

**เพิ่ม endpoint ใหม่**
1. `grep -n "public .* <Name>(" App_Code/RFService.cs` ให้แน่ใจว่ายังไม่มีชื่อซ้ำ (ASMX ไม่รองรับ overload ที่ชื่อเดียวกันโดยไม่ตั้ง `MessageName`)
2. เพิ่มเมธอดตามแบบ A (SP) ท้ายหมวดที่ใกล้เคียง ใช้ `.Tables["Table"]`; ทำ SP ใหม่/แก้ใน SQL Server แยกต่างหาก (ไม่อยู่ในรีโปนี้)
3. ถ้าต้อง inline SQL ให้ใช้ `SqlParameter` แทนการต่อสตริง
4. แจ้งทีมแอป: ชื่อ endpoint, พารามิเตอร์ (`o*`), คอลัมน์ที่คืน (ชื่อ + ชนิด + เป็น array ใน xml2js) แล้วเพิ่ม method ใน `service.ts`
5. deploy ลง virtual directory DEV ก่อน (`RF-Service_TCP_DEV`) แล้วค่อย PROD

**Debug "แอปได้ [] / ไม่มีข้อมูล"**: เรียก `http://{host}/RF-Service_TCP/RFService.asmx/<Name>?...` ในเบราว์เซอร์ ดู XML ดิบ; ถ้าได้ HTTP 500 ให้ดู `C:\LogFile\*.txt` และ SP ใน SQL Server (`customErrors=Off` จึงเห็น stack trace ในหน้าเว็บ)

## ความเสี่ยงที่พบฝั่ง server (แจ้งผู้ใช้เมื่อเกี่ยวข้อง ไม่แก้เองโดยไม่ได้ขอ)
1. **SQL injection**: inline SQL ต่อสตริงจากพารามิเตอร์ ~130 endpoint (เช่น `Get_Warehouse`, `Get_ClientByUser`, `Get_ZoneByUser`) ควรเปลี่ยนเป็น parameterized/SP
2. **ไม่มีการยืนยันตัวตนต่อ request** และเปิด HttpGet จึงเรียกแบบ URL ตรงๆ ได้ทุก endpoint รวมถึงเมธอดเขียนข้อมูล (`Confirm_*`, `Update_*`, `Delete_*`, `Reverse_*`)
3. **ข้อมูลรับรอง DB**: `App_Data/Connection*.xml` เก็บรหัสผ่านบัญชีผู้ดูแลฐานข้อมูล (`sa`) เป็นข้อความธรรมดา และมี IP/พอร์ตของฐานข้อมูลจริง ควรหมุนรหัสผ่าน, ใช้บัญชีสิทธิ์จำกัด, เก็บนอกรีโป (หรือเข้ารหัส section) และอย่า commit ไฟล์เหล่านี้ (skill นี้ไม่คัดลอกค่าเหล่านั้น)
4. **รหัสผ่านผู้ใช้ hash ด้วย MD5 ไม่มี salt** และถูกส่งเป็น query string ผ่าน HTTP
5. `customErrors=Off`, `compilation debug=true`, `maxRequestLength` ~1GB, `executionTimeout` สูงมาก — เหมาะกับ dev เท่านั้น
6. `catch (Exception) { }` เงียบใน `ApplyLogonDatabase` และ `throw new Exception(ex.Message)` ทิ้ง stack ต้นทาง
7. ไฟล์ตกค้างที่ไม่ควรอยู่ในซอร์ส: `*.rar`, `*.onetoc2`, `UpgradeLog.htm`, `Backup/`, `hhupdate/*.similan` (~2.9 MB), `App_Code/1.csv` (ข้อมูลสต็อก ~86 KB, encoding TIS-620)
8. `RFService.cs` ไฟล์เดียว ~19k บรรทัด: แก้เฉพาะจุด หลีกเลี่ยง refactor ใหญ่ที่ไม่ได้ขอ
