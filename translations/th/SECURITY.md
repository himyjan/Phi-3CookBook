<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "57f14126c1c6add76b3aef3844dfe4e3",
  "translation_date": "2025-05-09T04:18:53+00:00",
  "source_file": "SECURITY.md",
  "language_code": "th"
}
-->
## ความปลอดภัย

Microsoft ให้ความสำคัญกับความปลอดภัยของผลิตภัณฑ์ซอฟต์แวร์และบริการของเราอย่างจริงจัง ซึ่งรวมถึงที่เก็บซอร์สโค้ดทั้งหมดที่จัดการผ่านองค์กร GitHub ของเรา เช่น [Microsoft](https://github.com/Microsoft), [Azure](https://github.com/Azure), [DotNet](https://github.com/dotnet), [AspNet](https://github.com/aspnet) และ [Xamarin](https://github.com/xamarin)

หากคุณเชื่อว่าคุณพบช่องโหว่ด้านความปลอดภัยในที่เก็บข้อมูลที่เป็นของ Microsoft ที่ตรงตาม [คำจำกัดความของช่องโหว่ด้านความปลอดภัยของ Microsoft](https://aka.ms/security.md/definition) กรุณารายงานให้เราทราบตามที่อธิบายไว้ด้านล่าง

## การรายงานปัญหาด้านความปลอดภัย

**กรุณาอย่ารายงานช่องโหว่ด้านความปลอดภัยผ่านปัญหาสาธารณะใน GitHub**

แทนที่จะเป็นเช่นนั้น กรุณารายงานไปยัง Microsoft Security Response Center (MSRC) ที่ [https://msrc.microsoft.com/create-report](https://aka.ms/security.md/msrc/create-report)

หากคุณต้องการส่งโดยไม่ต้องเข้าสู่ระบบ ส่งอีเมลไปที่ [secure@microsoft.com](mailto:secure@microsoft.com) หากเป็นไปได้ กรุณาเข้ารหัสข้อความของคุณด้วยกุญแจ PGP ของเรา; กรุณาดาวน์โหลดได้จาก [Microsoft Security Response Center PGP Key page](https://aka.ms/security.md/msrc/pgp)

คุณควรได้รับการตอบกลับภายใน 24 ชั่วโมง หากด้วยเหตุผลใดก็ตามที่คุณไม่ได้รับ กรุณาติดตามผลผ่านอีเมลเพื่อให้แน่ใจว่าเราได้รับข้อความต้นฉบับของคุณ ข้อมูลเพิ่มเติมสามารถดูได้ที่ [microsoft.com/msrc](https://www.microsoft.com/msrc)

กรุณาระบุข้อมูลที่ร้องขอด้านล่างนี้ (เท่าที่คุณสามารถให้ได้) เพื่อช่วยให้เราเข้าใจลักษณะและขอบเขตของปัญหาที่อาจเกิดขึ้นได้ดีขึ้น:

  * ประเภทของปัญหา (เช่น buffer overflow, SQL injection, cross-site scripting เป็นต้น)
  * เส้นทางเต็มของไฟล์ซอร์สที่เกี่ยวข้องกับการแสดงออกของปัญหา
  * ตำแหน่งของซอร์สโค้ดที่ได้รับผลกระทบ (tag/branch/commit หรือ URL โดยตรง)
  * การตั้งค่าพิเศษใดๆ ที่จำเป็นต้องใช้ในการทำซ้ำปัญหา
  * คำแนะนำทีละขั้นตอนในการทำซ้ำปัญหา
  * โค้ด proof-of-concept หรือ exploit (ถ้าเป็นไปได้)
  * ผลกระทบของปัญหา รวมถึงวิธีที่ผู้โจมตีอาจใช้ประโยชน์จากปัญหา

ข้อมูลนี้จะช่วยให้เราจัดการรายงานของคุณได้รวดเร็วขึ้น

หากคุณรายงานเพื่อรับรางวัลบั๊กบาวน์ตี้ รายงานที่สมบูรณ์ยิ่งขึ้นจะช่วยเพิ่มโอกาสได้รับรางวัลสูงขึ้น กรุณาเยี่ยมชมหน้า [Microsoft Bug Bounty Program](https://aka.ms/security.md/msrc/bounty) สำหรับรายละเอียดเพิ่มเติมเกี่ยวกับโปรแกรมที่กำลังดำเนินอยู่ของเรา

## ภาษาที่ต้องการ

เราต้องการให้การสื่อสารทั้งหมดเป็นภาษาอังกฤษ

## นโยบาย

Microsoft ปฏิบัติตามหลักการของ [Coordinated Vulnerability Disclosure](https://aka.ms/security.md/cvd)

**ข้อจำกัดความรับผิดชอบ**:  
เอกสารนี้ได้รับการแปลโดยใช้บริการแปลภาษา AI [Co-op Translator](https://github.com/Azure/co-op-translator) แม้ว่าเราจะพยายามให้มีความถูกต้อง แต่โปรดทราบว่าการแปลโดยอัตโนมัติอาจมีข้อผิดพลาดหรือความไม่ถูกต้อง เอกสารต้นฉบับในภาษาต้นทางถือเป็นแหล่งข้อมูลที่เชื่อถือได้ สำหรับข้อมูลที่สำคัญ ขอแนะนำให้ใช้บริการแปลโดยมนุษย์ผู้เชี่ยวชาญ เราไม่รับผิดชอบต่อความเข้าใจผิดหรือการตีความที่ผิดพลาดที่เกิดจากการใช้การแปลนี้