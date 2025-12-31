# 🐳 คู่มือการติดตั้งและรันระบบ (Installation Guide)

MicroAccount รองรับการติดตั้งทั้งแบบ "ทีเดียวจบ" (Unified Docker) สำหรับการใช้งานทั่วไป และแบบแยกส่วนสำหรับการพัฒนาต่อยอด

---

## ⚡ 1. วิธีติดตั้งแบบด่วน (Unified Docker - แนะนำ)
วิธีนี้จะติดตั้งทั้ง App, ฐานข้อมูล และเครื่องมือจัดการ หลังบ้านให้ในคำสั่งเดี่ยว เหมาะสำหรับผู้ใช้ทั่วไปหรือการทดสอบระบบ

1. **เตรียมความพร้อม**: ติดตั้ง [Docker Desktop](https://www.docker.com/products/docker-desktop/)
2. **รันคำสั่ง**: เข้าไปที่โฟลเดอร์รวบยอด (`app-micro-account`) แล้วรัน:
   ```bash
   docker-compose up -d
   ```
3. **เริ่มใช้งาน**:
   - **แอปพลิเคชัน**: [http://localhost:3000](http://localhost:3000)
   - **ฐานข้อมูล (pgAdmin)**: [http://localhost:5050](http://localhost:5050) (User: `admin@microtronic.biz`, Pass: `admin`)

---

## 🛠 2. สำหรับนักพัฒนา (Manual Setup for Dev)
หากต้องการแก้ไขโค้ดและเห็นผลทันที (Hot Reload) ให้ใช้วิธีรันแยกส่วนดังนี้:

### ขั้นตอนที่ 1: รันฐานข้อมูล (Engines)
```bash
cd app-micro-formula-engins
docker-compose up -d
```

### ขั้นตอนที่ 2: รันโปรแกรม (Frontend)
```bash
cd app-micro-formula
npm install
npm run dev
```

---

## 🧪 3. การทดสอบการเชื่อมต่อ
หากต้องการทดสอบว่า Frontend คุยกับ Docker DB ได้หรือไม่:
```bash
cd app-micro-formula
npx tsx lib/test-db.ts
```

## 🔄 4. การจัดการฐานข้อมูล
หากมีการเปลี่ยนแปลง SQL ใน Migration ให้รันคำสั่งนี้เพื่อ Sync เข้า Docker:
```bash
cat supabase/migrations/*.sql | docker exec -i micro-account-db psql -U admin -d micro_account
```

> **หมายเหตุสำคัญ**: การติดตั้งแบบ Unified Docker ในขั้นตอนที่ 1 จะใช้ค่า Config เริ่มต้นที่พร้อมใช้งานทันที ไม่ต้องตั้งค่า `.env` เพิ่มเติมเอง

