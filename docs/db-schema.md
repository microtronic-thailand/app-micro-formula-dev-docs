# Database Schema

MicroAccount ใช้โครงสร้างฐานข้อมูลที่มีความยืดหยุ่น รองรับการทำงานทั้งแบบ Cloud (Supabase) และ Local (Postgres)

## ตารางหลักที่สำคัญ

### 1. Profiles (การจัดการผู้ใช้งาน)
เก็บข้อมูลโปรไฟล์ผู้ใช้งานและสิทธิ์การเข้าถึง (RBAC)
- `id`: UUID (Primary Key)
- `email`: อีเมลผู้ใช้งาน
- `role`: สิทธิ์การใช้งาน (`user`, `admin`, `super_admin`)
- `points`: คะแนนสะสม (Gimmick)
- `must_change_password`: บังคับเปลี่ยนรหัสผ่าน

### 2. Customers (ข้อมูลลูกค้า)
- `id`: UUID
- `name`: ชื่อบริษัท/ลูกค้า
- `tax_id`: เลขประจำตัวผู้เสียภาษี
- `branch`: สาขา
- `address`: ที่อยู่
- `phone`: เบอร์โทรศัพท์

### 3. Products (ข้อมูลสินค้าและสต็อก)
- `id`: UUID
- `name`: ชื่อสินค้า
- `sku`: รหัสสินค้า (Unique)
- `price`: ราคาขาย
- `unit`: หน่วยนับ
- `stock_quantity`: จำนวนคงเหลือ

### 4. Quotations (ใบเสนอราคา)
- `number`: เลขที่เอกสาร
- `date`: วันที่ออกเอกสาร
- `status`: สถานะ (`draft`, `sent`, `accepted`, `rejected`, `invoiced`)
- `grand_total`: ยอดรวมสุทธิ

### 5. Invoices (ใบแจ้งหนี้/ใบกำกับภาษี)
- `number`: เลขที่เอกสาร
- `status`: สถานะ (`draft`, `issued`, `paid`, `overdue`, `cancelled`)
- `vat_total`: ยอดภาษีมูลค่าเพิ่ม
- `wht_total`: ยอดหัก ณ ที่จ่าย
- `grand_total`: ยอดรวมสุทธิ

### 6. Expenses (บันทึกรายจ่าย)
- `description`: รายละเอียดรายจ่าย
- `amount`: จำนวนเงิน
- `is_vat`: มี VAT หรือไม่
- `date`: วันที่จ่าย

---

## ความสัมพันธ์ของข้อมูล (ER Diagram Summary)
- **Profiles** สัมพันธ์กับทุกข้อมูลธุรกิจผ่าน `owner_id` เพื่อแยกข้อมูลตามผู้ใช้งาน (Multi-tenancy)
- **Invoices** สามารถเชื่อมโยงมาจาก **Quotations** ได้
- **invoice_items** และ **quotation_items** จะอ้างอิงถึง **Products** เพื่อดึงราคาและรหัสสินค้า
