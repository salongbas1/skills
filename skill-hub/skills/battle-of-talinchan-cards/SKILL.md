---
name: battle-of-talinchan-cards
description: "ฐานข้อมูลการ์ดเกม Battle of Talinchan ครบทุกเซ็ต 1,347 ใบ (BT01-BT11, SD, CC, ODY1, KD, PRMO ฯลฯ) พร้อมชื่อ ประเภท สี Symbol Cost/Gem/Power เรเรตี้ และข้อความเอฟเฟกต์ ใช้เมื่อผู้ใช้ถามถึงการ์ดใบใดใบหนึ่ง ค้นหา/กรองการ์ด เทียบการ์ด แนะนำหรือช่วยสร้างเด็ค ถามเอฟเฟกต์ คีย์เวิร์ด (จุติ คำสั่งเสีย สอดแนม ธรณีสูบ ฯลฯ) หรือรหัสการ์ดเช่น BT01-001 Use when the user asks about Battle of Talinchan / Talinchan TCG cards, deck building, card lookups or effect text."
---

# Battle of Talinchan Cards

ข้อมูลการ์ดเกม **Battle of Talinchan** แปลงมาจาก repo `kerlos/battle-of-talinchan-cards` (fan-made, MIT สำหรับซอร์สโค้ด; รูปและข้อความบนการ์ดเป็นลิขสิทธิ์ของ บริษัท พสุโลล จำกัด / บริษัท ไลอ้อน เฮ้าส์ จำกัด) ข้อมูลทั้งหมดอยู่ในโฟลเดอร์ `references/` แยกไฟล์ตามเซ็ต

## Instructions

### วิธีค้นหาการ์ด (สำคัญ: อย่าโหลดทุกไฟล์พร้อมกัน)
1. **ทราบรหัสการ์ด** (เช่น `BT05-023`) → ดูไฟล์เซ็ตตามคำนำหน้า (`references/bt05.md`) ใช้ Grep หา `^BT05-023`
2. **ทราบชื่อการ์ด (บางส่วน)** → Grep ชื่อข้ามทุกไฟล์ใน `references/` (เช่น `grep -rn "นนทก" references/`)
3. **ค้นตามเงื่อนไข** (สี, Symbol, Cost, คีย์เวิร์ด, ประเภท) → Grep ข้อความในคอลัมน์ที่ต้องการข้ามทุกไฟล์ แล้วสรุปผล ถ้าผลเยอะให้จำกัดด้วย `head_limit` และบอกผู้ใช้ว่ามีทั้งหมดกี่ใบ
4. **ถามเอฟเฟกต์/คีย์เวิร์ด** → อ่านข้อความเอฟเฟกต์จากบรรทัดการ์ดตามจริง อย่าเดาหรือแต่งกติกาเพิ่ม
5. **ช่วยสร้าง/ตรวจเด็ค** → ค้นการ์ดที่เข้าเงื่อนไขก่อน แล้วเสนอเป็นรายการ ระบุรหัสการ์ดทุกใบ และเช็กข้อจำกัด `limit=` / `ex=` ของการ์ดแต่ละใบ

### รูปแบบบรรทัดการ์ด
```
PRINT | NAME | TYPE[:subtype] | COLOR | SYMBOL | STATS | RARITIES | SOI | EFFECT [tags]
```
- **PRINT** รหัสการ์ด `<เซ็ต>-<เลข>` (หนึ่งรหัสต่อหนึ่งบรรทัด; เรเรตี้ที่ต่างกันของใบเดียวกันรวมในช่อง RARITIES เช่น `UR/SCR/CBR/PR`)
- **TYPE** `Avatar` | `Magic` (subtype: `Normal`, `React`, `Modification`, `Land`) | `Life` | `Construct`
- **COLOR** แดง / ฟ้า / เขียว / ม่วง (`-` = ไม่มีสี/ไม่ระบุ เช่น Life)
- **SYMBOL** เผ่า/หมวดของการ์ด เช่น เทพ ยักษ์ จอมเวทย์ คน แมลง สัตว์ รัททาทุย นรก ผี หุ่นยนต์ ปลา สิ่งก่อสร้าง ต่างชาติ ต้นไม้ เปรต เอเลี่ยน ฤษี กะปอม สัตว์มหัศจรรย์ ทหาร มังกร ไซเบอร์
- **STATS** Avatar/Construct = `cost/gem/power`; Magic = `cost`; Life = `-`
- **RARITIES** SR, UR, PR, CBR, C, SCR, R, USEC
- **SOI (ซอย)** หมายเลขซอย 0-3 ตามข้อมูลต้นทาง
- **EFFECT** ข้อความเอฟเฟกต์หลัก; `¶` = ขึ้นบรรทัดใหม่ในข้อความ; `{symbol X}`, `{magic}`, `{mod}`, `[link ชื่อการ์ด]` คือมาร์กอัปอ้างอิงที่มาจากต้นทาง
- **`‖ alt:`** ข้อความเอฟเฟกต์ของอีกพิมพ์ที่ถ้อยคำต่างกัน (54 ใบ มักเป็นการแก้ถ้อยคำ/errata) — ถ้าผู้ใช้ถามเรื่องเอฟเฟกต์ ให้แจ้งว่ามีหลายถ้อยคำเมื่อพบ
- **tags** `ex=` (เช่น `Only #1`), `limit=` (จำนวนที่ใส่เด็คได้ต่างจากปกติ), `gemColor=`, `by=` (ผู้ออกแบบ), `flavor=` (ข้อความประกอบของ Life)

### คีย์เวิร์ดที่พบในข้อความการ์ด
จุติ, คำสั่งเสีย, เซ่นไหว้, สอดแนม, ธรณีสูบ, เลือกปฏิบัติ, สามัคคี, โล่มนุษย์, เตะไข่, เทิร์นละครั้ง, ต่อเนื่อง, สั่งใช้, อัตโนมัติ, พอดี, ลูกฮึด, แทงหลัง
(repo ต้นทางไม่มีคำอธิบายกติกาของแต่ละคีย์เวิร์ด ให้อธิบายจากข้อความการ์ดที่ปรากฏเท่านั้น และบอกผู้ใช้หากต้องอาศัยกติกาเกมที่ไม่มีในข้อมูลนี้)

### เซ็ตที่มี (จำนวนการ์ดไม่ซ้ำรหัส)
| เซ็ต | จำนวน | ซอย | ไฟล์ |
|---|---|---|---|
| BT01 | 55 | soi 1 | references/bt01.md |
| BT02 | 60 | soi 1 | references/bt02.md |
| BT03 | 60 | soi 1 | references/bt03.md |
| BT04 | 60 | soi 1 | references/bt04.md |
| BT05 | 75 | soi 2,1 | references/bt05.md |
| BT06 | 70 | soi 2,1 | references/bt06.md |
| BT07 | 75 | soi 2 | references/bt07.md |
| BT08 | 70 | soi 2 | references/bt08.md |
| BT09 | 75 | soi 3 | references/bt09.md |
| BT10 | 75 | soi 3 | references/bt10.md |
| BT11 | 18 | soi 3 | references/bt11.md |
| CC01 | 60 | soi 1 | references/cc01.md |
| CC02 | 61 | soi 2,1 | references/cc02.md |
| FPRO | 7 | soi 2,3,1 | references/fpro.md |
| KD00 | 4 | soi 0 | references/kd00.md |
| KD01 | 25 | soi 3 | references/kd01.md |
| KD02 | 25 | soi 3 | references/kd02.md |
| KD03 | 25 | soi 3 | references/kd03.md |
| KD04 | 25 | soi 3 | references/kd04.md |
| ODY1 | 90 | soi 1 | references/ody1.md |
| PRE0 | 7 | soi 1,2 | references/pre0.md |
| PRMO | 107 | soi 1,2,3 | references/prmo.md |
| SD01 | 25 | soi 1 | references/sd01.md |
| SD02 | 25 | soi 1 | references/sd02.md |
| SD03 | 25 | soi 1 | references/sd03.md |
| SD04 | 25 | soi 1 | references/sd04.md |
| SD05 | 25 | soi 1 | references/sd05.md |
| SD06 | 25 | soi 2,1 | references/sd06.md |
| SD07 | 25 | soi 2,1 | references/sd07.md |
| SD08 | 25 | soi 3,2 | references/sd08.md |
| SL01 | 8 | soi 1 | references/sl01.md |
| SL02 | 10 | soi 2 | references/sl02.md |

### แนวปฏิบัติ
- ตอบเป็นภาษาเดียวกับที่ผู้ใช้ถาม และคงชื่อการ์ด/คีย์เวิร์ดเป็นภาษาไทยตามต้นฉบับ
- อ้างรหัสการ์ดทุกครั้งที่พูดถึงการ์ด เพื่อให้ตรวจสอบได้
- ไม่พบการ์ด → บอกตรง ๆ แล้วลองค้นชื่อใกล้เคียง (ข้อมูลอาจไม่ครบหรือล้าสมัยตาม commit ต้นทาง) อย่าแต่งการ์ดขึ้นเอง
- ข้อมูลนี้ไม่มี drop rate และไม่มีราคา; ถ้าผู้ใช้ต้องการรูปการ์ด ให้ชี้ไปที่โฟลเดอร์ `images/` ใน repo ต้นทาง (ชื่อไฟล์รูปแบบ `BT01-001.png`, `CC02-025-SCR.png`)
- ห้ามระบุว่าเป็นข้อมูลทางการ — เป็นข้อมูลที่แฟนคลับรวบรวม
