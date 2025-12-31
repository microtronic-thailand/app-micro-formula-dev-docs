# Security & Audit Architecture

ระบบ MicroFormula ออกแบบมาให้รองรับมาตรฐานความปลอดภัยและการตรวจสอบ (Compliance) โดยมีหัวใจหลักคือระบบ License และ Audit Trail

---

## 🔑 License Security System
ระบบป้องกันการใช้งานโดยไม่ได้รับอนุญาตผ่านการใช้ Digital Signature หลักการทำงานคือ:
1. **Machine Key**: รหัสประจำเครื่อง (HWID) ที่ใช้ระบุตัวตนระบบ
2. **Signature Verification**: ทุก License Key จะถูกเข้ารหัสแบบ Base64 และมีการตรวจสอบลายเซ็น (SALT) ใน `lib/license-helper.ts`
3. **Lock Mechanism**: หาก License หมดอายุหรือลายเซ็นไม่ถูกต้อง `LicenseGuard.tsx` จะล็อกหน้าจอเข้าถึงข้อมูลทันที

## 📝 Audit Trail System
การบันทึกประวัติการเปลี่ยนแปลงข้อมูล (Immutable Logs) เพื่อการตรวจสอบย้อนหลัง:
- **Table**: `audit_logs`
- **Function**: `logAudit` ใน `lib/data-service.ts`
- **Fields**:
    - `action`: ประเภทการกระทำ (CREATE, UPDATE, DELETE, LOGIN)
    - `entity_type`: ชื่อโมดูลที่เกี่ยวข้อง (INVOICE, EXPENSE, etc.)
    - `entity_id`: ID ของข้อมูลที่อ้างถึง
    - `old_data` & `new_data`: บันทึกข้อมูลก่อนและหลังเปลี่ยน (JSONB) ในกรณีที่ต้องการตรวจสอบรายละเอียดเชิงลึก (Implemented via Data Service wrappers)

## 🗄️ Database Strategy (Dual Engine)
ระบบรองรับการทำงานทั้งแบบ Cloud และ Local 100%:
- **Cloud Mode**: เชื่อมกับ Supabase ผ่าน Client SDK
- **Local Mode**: ใช้ Local DB ผ่าน `lib/postgres.ts` (Bypass Supabase)
- **Engine Selection**: ควบคุมผ่าน Environment Variables ใน `.env.local`

---

## 🚀 การจัดการ Database
หากมีการแก้ไข Schema:
1. แก้ไขไฟล์ `supabase/schema.sql` (Master Schema)
2. ทำการรัน SQL บนเครื่อง Local หรือ Supabase Dashboard
3. Update `types/index.ts` ให้ตรงกับ Schema ใหม่
