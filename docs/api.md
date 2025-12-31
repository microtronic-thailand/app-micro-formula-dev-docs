# API Reference (Data Services)

ระบบใช้ `data-service.ts` เป็นเลเยอร์กลางในการจัดการข้อมูลทั้งหมด โดยรองรับทั้งการเรียกผ่าน API จริง และการสลับไปใช้ **Mock Data** ในโหมด Demo

## สถาปัตยกรรม
ฟังก์ชันใน Data Service จะถูกเรียกใช้งานจากฝั่ง Client (React Components/Hooks) โดยมีการตรวจสอบสถานะ `isDemoMode()` ก่อนดำเนินการเสมอ

### 1. การจัดการโปรไฟล์ (Profiles)
- `getProfile(id)`: ดึงข้อมูลโปรไฟล์ผู้ใช้งาน
- `getAllProfiles()`: ดึงข้อมูลโปรไฟล์ทั้งหมด (เฉพาะ Admin)
- `updateUserRole(userId, role)`: เปลี่ยนสิทธิ์การเข้าถึง

### 2. การจัดการลูกค้า (Customers)
- `getCustomers()`: ดึงรายชื่อลูกค้าทั้งหมด
- `createCustomer(data)`: เพิ่มข้อมูลลูกค้าใหม่
- `updateCustomer(id, data)`: แก้ไขข้อมูลลูกค้า

### 3. การจัดการสินค้าและสต็อก (Products)
- `getProducts()`: ดึงรายชื่อสินค้าและจำนวนสต็อก
- `updateStock(productId, delta)`: ปรับปรุงจำนวนสต็อกแบบ Atomic (ผ่าน RPC)

### 4. การจัดการเอกสาร (Documents)
- `getQuotations()` / `getInvoices()`: ดึงรายการเอกสาร
- `createQuotation(data)` / `createInvoice(data)`: สร้างเอกสารใหม่พร้อมรายการคำนวณภาษี

### 5. ระบบรายงานและภาษี (Tax & Reports)
- `getTaxReport(month, year)`: คำนวณสรุปยอดซื้อ-ขาย และภาษีมูลค่าเพิ่มสำหรับยื่น ภ.พ.30

---

## รูปแบบการเรียกใช้งาน (Example)

```typescript
import { getInvoices } from '@/lib/data-service';

async function fetchData() {
    try {
        const invoices = await getInvoices();
        console.log(invoices);
    } catch (error) {
        console.error("Failed to load invoices");
    }
}
```

## Mock Data Implementation
ในไฟล์ `mock-data.ts` จะมีข้อมูลจำลองที่ถูกส่งกลับแทนการเรียก Database ในกรณีที่ `isDemoMode()` เป็น `true` เพื่อความรวดเร็วในการทดสอบระบบและลดภาระ Server
