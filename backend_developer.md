# ⚙️ Node.js Backend: Planning & Project Initialization

เอกสารนี้รวบรวมมาตรฐานการขึ้นโปรเจกต์ Node.js (Express/NestJS/Fastify) ระดับ Production-Grade ที่เน้นความปลอดภัยและโครงสร้างที่ยั่งยืน

## Phase 1: Technical Requirement & Data Design (วางแผนเทคนิคและข้อมูล)
*Backend ที่ดี เริ่มต้นที่ Database Design ไม่ใช่ Code*

- [ ] **Tech Stack Decision:**
    - [ ] **Framework:** Express (Minimal), NestJS (Enterprise/Structured), หรือ Fastify (Performance)
    - [ ] **Language:** **TypeScript** (ภาคบังคับสำหรับ Backend ยุคปัจจุบัน เพื่อ Type Safety)
    - [ ] **Database:** SQL (PostgreSQL/MySQL) หรือ NoSQL (MongoDB)
- [ ] **Database Schema Design:**
    - [ ] เขียน ER Diagram (Entity-Relationship)
    - [ ] ตรวจสอบ Data Normalization
    - [ ] กำหนด Relationships (1-1, 1-N, N-N) และ Foreign Keys
- [ ] **API Specification (Contract First):**
    - [ ] ร่างรายการ Endpoints (Method, Path, Request Body, Response)
    - [ ] หรือเขียน **OpenAPI (Swagger) Spec** ล่วงหน้า เพื่อตกลงกับ Frontend

## Phase 2: Environment & Repository Setup (เตรียมเครื่องมือ)
*รากฐานที่แข็งแรง*

- [ ] **Project Initialization:**
    - [ ] `npm init` หรือ `pnpm init`
    - [ ] Setup **TypeScript Config (`tsconfig.json`)**: ตั้งค่า `strict: true`, `target`, `moduleResolution`
- [ ] **Code Quality Tools:**
    - [ ] **ESLint:** Config กฎมาตรฐาน (เช่น Airbnb หรือ Google Standard)
    - [ ] **Prettier:** จัด Format อัตโนมัติ
    - [ ] **Husky & Lint-staged:** ห้าม Commit ถ้า Code เน่า
- [ ] **Environment Configuration:**
    - [ ] ติดตั้ง `dotenv` หรือ library จัดการ config
    - [ ] สร้าง `.env.example`
    - [ ] ใช้ Library ตรวจสอบ Type ของ Env (เช่น **Envalid** หรือ **Zod**) เพื่อป้องกันแอปพังถ้าลืมใส่ Key

## Phase 3: Project Architecture (โครงสร้างสถาปัตยกรรม) 🏗️
*เลือกใช้ Layered Architecture / Clean Architecture แทนการกองทุกอย่างใน controller*

- [ ] **Directory Structure Design:**
```text
src/
├── config/              # Environment variables & configuration
├── controllers/         # รับ Request / ส่ง Response (ห้ามมี Business Logic)
├── services/            # Business Logic อยู่ที่นี่ (คำนวณ, เงื่อนไขต่างๆ)
├── repositories/        # (Optional) ติดต่อ Database โดยตรง
├── models/              # Database Schema / DTOs
├── middlewares/         # Auth check, Error handler, Logging
├── routes/              # API Route definitions
├── utils/               # Helper functions (Date formatter, Hash)
├── types/               # TypeScript interfaces
├── app.ts               # Entry point
└── server.ts            # Entry point run server
```
- [ ] **Global Error Handling:**
    - [ ] สร้าง Custom Error Class (AppError) ที่ระบุ StatusCode ได้
    - [ ] สร้าง Middleware ดักจับ Error ตัวสุดท้าย เพื่อส่ง Response Format ที่เป็นมาตรฐาน JSON เสมอ (ห้ามส่ง Stack Trace ให้ User เห็น)
- [ ] **Logging System:**
    - [ ] เลิกใช้ `console.log`
    - [ ] ติดตั้ง **Winston** หรือ **Pino** (รองรับ Log levels: Info, Warn, Error และการหมุนไฟล์ Log)

## Phase 4: Database Layer Integration (เชื่อมต่อฐานข้อมูล)
*จัดการข้อมูลอย่างเป็นระบบ*

- [ ] **ORM / Query Builder Setup:**
    - [ ] เลือกเครื่องมือ: **Prisma** (Modern/Type-safe), **TypeORM**, หรือ **Sequelize**
    - [ ] Setup Connection Pooling
- [ ] **Migrations Strategy:**
    - [ ] ตั้งค่าระบบ Migration (ห้ามแก้ DB Structure ด้วยมือใน Production เด็ดขาด)
- [ ] **Seeding:** เตรียม Script สำหรับสร้างข้อมูลจำลอง หรือข้อมูลตั้งต้น (Master Data)

## Phase 5: Security & Validation (ความปลอดภัย) 🛡️
*หัวใจสำคัญของ Backend*

- [ ] **Input Validation:**
    - [ ] ติดตั้ง **Zod** หรือ **Joi** เพื่อตรวจสอบ Request Body/Query/Params ก่อนเข้า Controller
    - [ ] "Never Trust Client"
- [ ] **Security Headers:**
    - [ ] ติดตั้ง **Helmet** (HTTP Headers security)
    - [ ] Config **CORS** (อนุญาตเฉพาะ Domain ที่กำหนด)
- [ ] **Rate Limiting:** ป้องกันการยิงถล่ม API
- [ ] **Authentication & Authorization:**
    - [ ] วางแผนระบบ Login (JWT vs Session)
    - [ ] เตรียม Middleware สำหรับเช็ค Role (Admin/User)

## Phase 6: Documentation & Testing (เอกสารและการทดสอบ)
*เพื่อให้ทีมทำงานต่อได้*

- [ ] **API Documentation:**
    - [ ] Setup **Swagger UI** (swagger-jsdoc) ให้ Generate Doc จาก Code หรือ Comment
- [ ] **Testing Setup:**
    - [ ] ติดตั้ง **Jest** หรือ **Vitest**
    - [ ] เตรียม Environment สำหรับ Test Database (Docker Container)
- [ ] **Dockerization:**
    - [ ] เขียน `Dockerfile` (Multi-stage build เพื่อลดขนาด Image)
    - [ ] เขียน `docker-compose.yml` สำหรับรัน DB และ Redis ในเครื่อง Dev

---

> **💡 Pro Tip:** จงแยก **"App"** ออกจาก **"Server"** ในไฟล์ Entry point
>
> - `app.ts`: Setup express, middleware, routes (ใช้สำหรับการเขียน Test)
> - `server.ts`: Import app มาแล้วสั่ง `app.listen()` (ใช้สำหรับรันจริง)
>
> วิธีนี้จะทำให้การเขียน Unit/Integration Test ง่ายขึ้นมากครับ
