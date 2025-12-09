# 💻 Frontend Developer: Planning & Project Initialization

เอกสารนี้รวบรวมขั้นตอนการเตรียมโปรเจกต์ (Scaffolding) การกำหนดมาตรฐาน Code และการวางโครงสร้างโฟลเดอร์สำหรับ Frontend Web Application ระดับ Production

## Phase 1: Requirement & Technical Analysis (วิเคราะห์งาน)
*อย่าเพิ่งเขียน Code จนกว่าจะเข้าใจภาพรวม*

- [ ] **Design Review (Figma/Adobe XD):**
    - [ ] ตรวจสอบ Grid System และ Breakpoints
    - [ ] เช็ค Design Tokens (Colors, Typography, Spacing) ว่าครบถ้วนไหม
    - [ ] วิเคราะห์ความยากของ UI (Complex Interactions/Animations)
- [ ] **API Analysis (Swagger/Postman):**
    - [ ] ตรวจสอบว่า Backend มี API Document หรือยัง?
    - [ ] ตกลงเรื่อง Data Structure และ Error Handling Format
    - [ ] ตกลงเรื่อง Authentication Flow (Cookie vs LocalStorage, JWT Expiry)
- [ ] **Browser Support:** กำหนด Browser เป้าหมาย (ต้องรองรับ IE ไหม? หรือ Modern Browsers เท่านั้น?)

## Phase 2: Repository & Environment Setup (ตั้งค่าสภาพแวดล้อม)
*สร้างบ้านให้เป็นระเบียบ*

- [ ] **Tech Stack Initialization:**
    - [ ] Framework: (e.g., Next.js, Vite + React, Vue, Nuxt)
    - [ ] Language: **TypeScript** (Mandatory for professional work)
    - [ ] Package Manager: (pnpm, yarn, หรือ npm)
- [ ] **Environment Variables (`.env`):**
    - [ ] สร้างไฟล์ `.env.example` (ห้ามใส่ Secret Key จริง)
    - [ ] กำหนดตัวแปรพื้นฐาน: `API_BASE_URL`, `NEXT_PUBLIC_...`
- [ ] **Git Setup:**
    - [ ] สร้าง `.gitignore` ที่ถูกต้อง
    - [ ] กำหนด Branching Strategy (Gitflow หรือ Trunk Based)

## Phase 3: Code Quality & Standards (มาตรฐานโค้ด) 🛡️
*กันไว้ดีกว่าแก้: บังคับใช้มาตรฐานก่อนเริ่มเขียนบรรทัดแรก*

- [ ] **Linter & Formatter:**
    - [ ] **ESLint:** ตั้งค่ากฎเพื่อจับ Bug และ Best Practices
    - [ ] **Prettier:** ตั้งค่าการจัดรูปแบบ Code (Space, Semicolon, Quotes) ให้เหมือนกันทั้งทีม
    - [ ] **Sort Imports:** ติดตั้ง Plugin จัดเรียงลำดับการ import อัตโนมัติ
- [ ] **Husky & Lint-staged:**
    - [ ] ตั้งค่า **Pre-commit hook**: ห้าม Commit ถ้า Code ยังไม่ผ่าน Lint หรือ Test
- [ ] **VS Code Settings:** สร้างโฟลเดอร์ `.vscode/settings.json` เพื่อบังคับ Config ของ Editor ให้ตรงกันทุกคนในทีม

## Phase 4: Project Architecture & Folder Structure (โครงสร้างโปรเจกต์)
*ออกแบบโฟลเดอร์ให้รองรับการขยายตัว (Feature-based Architecture)*

- [ ] **Base Directory Structure:**
```text
src/
├── app/ (หรือ pages/ สำหรับ Router)
├── components/          # Shared UI Components (Button, Input)
│   ├── ui/              # Atom components (ใช้ซ้ำได้ทั่วโปรเจกต์)
│   └── layout/          # Navbar, Footer, Sidebar
├── features/            # แบ่งตามฟีเจอร์ธุรกิจ (สำคัญมาก!)
│   ├── auth/            # (Login, Register, Forgot Password)
│   │   ├── components/
│   │   ├── hooks/
│   │   └── services/    # API calls เฉพาะ feature นี้
│   └── dashboard/
├── hooks/               # Global Hooks (useWindowSize, useTheme)
├── lib/                 # Third-party lib setup (axios instance, utils)
├── stores/              # Global State Management (Zustand, Redux)
├── styles/              # Global CSS, Tailwind Config
├── types/               # Global TypeScript Interfaces
└── utils/               # Pure functions (formatDate, currency)
```
- [ ] **Module Aliases:** ตั้งค่า `tsconfig.json` ให้ใช้ `@/components/...` แทน `../../components/...`

## Phase 5: Core Libraries Integration (เชื่อมต่อระบบหลัก)
*วางระบบท่อประปาและไฟฟ้า*

- [ ] **Styling System:**
    - [ ] Setup **Tailwind CSS** (หรือ CSS-in-JS library)
    - [ ] นำ **Design Tokens** (สี, ฟอนต์) จากข้อ 1 มาใส่ใน `tailwind.config.js`
- [ ] **Networking / API Client:**
    - [ ] Setup **Axios Instance** (หรือ Fetch wrapper)
    - [ ] **Interceptors:** ดักจับ Request (ใส่ Token) และ Response (จัดการ 401 Unauthorized/Logout อัตโนมัติ)
- [ ] **State Management Strategy:**
    - [ ] **Server State:** ใช้ React Query (TanStack Query) หรือ SWR (เลิกใช้ `useEffect` ดึงข้อมูลตรงๆ)
    - [ ] **Client State:** ใช้ Zustand, Context API, หรือ Redux Toolkit สำหรับ UI State
- [ ] **Routing & Guards:**
    - [ ] สร้าง PrivateRoute / ProtectedRoute Component
    - [ ] จัดการ Redirect กรณีไม่มีสิทธิ์เข้าถึง

## Phase 6: Development Experience (DX)
*ทำให้ชีวิตคนเขียน Code ง่ายขึ้น*

- [ ] **Create Common Components:** สร้างปุ่ม, Input, Modal, Card เตรียมไว้ก่อนเริ่มทำหน้าจริง
- [ ] **Mocking API:** เตรียม MSW (Mock Service Worker) หรือ Mock JSON สำหรับกรณี Backend ยังไม่เสร็จ
- [ ] **README.md:** เขียนวิธีการ Run, Build, และโครงสร้างโปรเจกต์ให้เพื่อนร่วมทีมอ่าน

---

> **💡 Pro Tip:** หัวใจสำคัญของ Frontend ยุคใหม่คือ **"Separation of Concerns"** พยายามแยก Logic (Hooks/Services) ออกจาก UI (JSX/TSX) เสมอ จะทำให้ Code อ่านง่ายและ Test ง่ายครับ
