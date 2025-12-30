# 🛠 คู่มือนักพัฒนา (Developer Installation Guide)

ยินดีต้อนรับสู่โปรเจกต์ MicroAccount เวอร์ชันใช้งานภายใน (Offline First) นี่คือขั้นตอนการติดตั้งและรันระบบด้วย Docker แบบ Step-by-Step

---

## 🚀 1. การเตรียมสภาพแวดล้อม (Prerequisites)
- **Docker Desktop**: ติดตั้งให้เรียบร้อยและรันอยู่
- **Node.js 18+**: สำหรับรัน Frontend
- **npm / yarn**: สำหรับจัดการ Packages

---

## 🐳 2. การติดตั้งฐานข้อมูล (Engines)
ฐานข้อมูลจะรันผ่าน Docker เพื่อความสะดวกในการจัดการ Schema

1. เข้าไปที่โฟลเดอร์เครื่องยนต์:
   ```bash
   cd app-micro-formula-engins
   ```
2. รัน Docker Compose:
   ```bash
   docker-compose up -d
   ```
3. ตรวจสอบสถานะการรัน:
   - **PostgreSQL**: `localhost:5432`
   - **pgAdmin**: `http://localhost:5050` (User: admin@microtronic.biz / Pass: admin)

---

## 📂 3. การรันตัวโปรแกรม (Frontend)
1. เข้าไปที่โฟลเดอร์โปรแกรม:
   ```bash
   cd app-micro-formula
   ```
2. ติดตั้ง dependencies:
   ```bash
   npm install
   ```
3. ตรวจสอบไฟล์ `.env.local`:
   - มั่นใจว่า `USE_LOCAL_DB=true`
   - `DATABASE_URL=postgres://admin:password123@localhost:5432/micro_account`
4. รันโปรแกรม:
   ```bash
   npm run dev
   ```

---

## 🧪 4. การทดสอบการเชื่อมต่อ (DB Connectivity)
หากต้องการทดสอบว่า Frontend คุยกับ Docker DB ได้หรือไม่ ให้รัน:
```bash
npx tsx lib/test-db.ts
```
หากสำเร็จจะแสดงรายชื่อ Tables ทั้งหมดในฐานข้อมูลครับ

---

## 🔄 5. Schema Migration สำหรับ Dev
หากมีการเปลี่ยนแปลง SQL ใน Supabase Migrations ให้รันคำสั่งนี้เพื่อ Sync เข้า Docker:
```bash
cat supabase/migrations/*.sql | docker exec -i micro-account-db psql -U admin -d micro_account
```
*(หมายเหตุ: ระบบจะข้ามค่า `auth.uid()` ของ Supabase เพื่อให้รันใน Local ได้)*
