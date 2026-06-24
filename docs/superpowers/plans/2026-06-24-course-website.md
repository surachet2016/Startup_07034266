# Course Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a public-facing, single-page Vue 3 + Vite website that displays the 8-week startup course content with sticky navigation, week sections, and .pptx download buttons.

**Architecture:** Single SPA with anchor-scroll navigation — no router. All content is static data in `src/data/weeks.js` (ported from `create_slides.py`). Vite serves `.pptx` files from `public/slides/` as static assets. Each week is rendered by one reusable `WeekSection.vue` component via `v-for`.

**Tech Stack:** Vue 3 (Composition API, `<script setup>`), Vite 5, plain CSS with custom properties (no CSS framework)

---

## File Map

| File | Responsibility |
|---|---|
| `package.json` | vue + vite + @vitejs/plugin-vue |
| `vite.config.js` | Vite plugin setup |
| `index.html` | HTML entry, font import |
| `src/main.js` | Mount Vue app |
| `src/style.css` | CSS variables + global reset + responsive |
| `src/App.vue` | Root: Navbar + Hero + WeekSections + Footer |
| `src/data/weeks.js` | All 8 weeks of course content |
| `src/components/NavBar.vue` | Sticky nav with 8 anchor buttons |
| `src/components/HeroSection.vue` | Course banner |
| `src/components/WeekSection.vue` | Week content (used ×8 via v-for) |
| `src/components/FooterBar.vue` | Footer |
| `public/slides/Week0N_Startup.pptx` | Slide files (copied from `slides/`) |

---

## Task 1: Project Scaffold

**Files:**
- Create: `package.json`
- Create: `vite.config.js`
- Create: `index.html`
- Create: `src/main.js`
- Create: `src/style.css`
- Create: `src/App.vue` (placeholder)

- [ ] **Step 1: Create `package.json`**

```json
{
  "name": "startup-course-website",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.4.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.0.0",
    "vite": "^5.0.0"
  }
}
```

- [ ] **Step 2: Install dependencies**

```bash
npm install
```

Expected: `node_modules/` created, no errors.

- [ ] **Step 3: Create `vite.config.js`**

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
})
```

- [ ] **Step 4: Create `index.html`**

```html
<!DOCTYPE html>
<html lang="th">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>07-034-266 การสร้างธุรกิจเริ่มต้นด้วยนวัตกรรมและเทคโนโลยี</title>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@400;600;700&display=swap" rel="stylesheet" />
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

- [ ] **Step 5: Create `src/main.js`**

```js
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'

createApp(App).mount('#app')
```

- [ ] **Step 6: Create `src/style.css`**

```css
:root {
  --bg: #ffffff;
  --accent: #00b4d8;
  --highlight: #ffa700;
  --text: #0d1b2a;
  --card: #f0f8ff;
  --light: #cae9ff;
  --card-border: #d0e8f5;
  --navy-dark: #1a2f45;
}

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: 'Sarabun', sans-serif;
  background: var(--bg);
  color: var(--text);
  line-height: 1.6;
}

a {
  color: inherit;
  text-decoration: none;
}
```

- [ ] **Step 7: Create placeholder `src/App.vue`**

```vue
<template>
  <div>
    <p style="padding: 2rem">Course website — scaffolded</p>
  </div>
</template>
```

- [ ] **Step 8: Run dev server to verify scaffold**

```bash
npm run dev
```

Open `http://localhost:5173`. Expected: page shows "Course website — scaffolded" with no console errors.

- [ ] **Step 9: Commit**

```bash
git init
git add package.json vite.config.js index.html src/
git commit -m "feat: scaffold Vue 3 + Vite project"
```

---

## Task 2: Data Layer

**Files:**
- Create: `src/data/weeks.js`

Port the `weeks` list from `create_slides.py` to JavaScript. The bullet type mapping is:
- Python `"##..."` → `{ type: "h2", text: "..." }`
- Python `"#..."` → `{ type: "h1", text: "..." }`
- Plain text → `{ type: "bullet", text: "..." }`

- [ ] **Step 1: Create `src/data/weeks.js`**

Week 1 shown in full — repeat the same shape for weeks 2–8, reading field values from `create_slides.py`:

```js
export const weeks = [
  {
    week: 1,
    title_th: "แนะนำรายวิชาและแนวคิดการเป็นผู้ประกอบการ ธุรกิจเริ่มต้น (Startup)",
    title_en: "Course Introduction & Entrepreneurship / Startup Concepts",
    clos: "CLO1: แสดงพฤติกรรมซื่อสัตย์ มีวินัย ตรงต่อเวลา\nCLO2: อธิบายแนวคิดการเป็นผู้ประกอบการ",
    method: "บรรยาย\nอภิปราย\nกรณีศึกษา",
    media: "สไลด์\nกรณีศึกษา\nVideo Clip",
    slides: [
      {
        title: "ภาพรวมรายวิชา",
        bullets: [
          { type: "h1", text: "รหัส 07-034-266 / 3 หน่วยกิต (2-2-5)" },
          { type: "bullet", text: "วิสาหกิจเริ่มต้น (Startup) คืออะไร" },
          { type: "bullet", text: "ทำไมต้องเรียนรายวิชานี้ในยุคดิจิทัล" },
          { type: "h1", text: "กติกาการเรียนและการประเมิน" },
          { type: "bullet", text: "การเข้าเรียน วินัย ตรงต่อเวลา" },
          { type: "bullet", text: "งานกลุ่ม / แผนธุรกิจ / Pitch Deck" },
          { type: "bullet", text: "เกณฑ์การให้คะแนน (100 คะแนน)" },
        ],
        note: "",
      },
      {
        title: "ผู้ประกอบการคือใคร?",
        bullets: [
          { type: "h1", text: "นิยามผู้ประกอบการ (Entrepreneur)" },
          { type: "bullet", text: "บุคคลที่สร้างคุณค่าใหม่ด้วยการรับความเสี่ยง" },
          { type: "bullet", text: "มองเห็นโอกาสที่คนอื่นมองข้าม" },
          { type: "h1", text: "ความแตกต่าง: ธุรกิจทั่วไป vs Startup" },
          { type: "bullet", text: "Startup: เติบโตเร็ว, ขยายได้ (Scalable)" },
          { type: "bullet", text: "ธุรกิจทั่วไป: เติบโตตามเส้นตรง" },
          { type: "h1", text: "ตัวอย่าง Startup ไทยที่ประสบความสำเร็จ" },
          { type: "bullet", text: "Grab, LINE MAN, Omise, Wongnai" },
        ],
        note: "Startup ไม่ใช่แค่ App มือถือ — มันคือวิธีคิดแบบใหม่",
      },
      {
        title: "Startup Ecosystem ในประเทศไทย",
        bullets: [
          { type: "h1", text: "ระบบนิเวศ Startup ไทย" },
          { type: "bullet", text: "NIA (สำนักงานนวัตกรรมแห่งชาติ)" },
          { type: "bullet", text: "depa (สำนักงานส่งเสริมเศรษฐกิจดิจิทัล)" },
          { type: "bullet", text: "สสว. (สำนักงานส่งเสริมวิสาหกิจขนาดกลางและขนาดย่อม)" },
          { type: "h1", text: "แหล่งทุนและการสนับสนุน" },
          { type: "bullet", text: "Angel Investor, Venture Capital (VC)" },
          { type: "bullet", text: "โครงการ Startup Thailand" },
          { type: "h1", text: "เทรนด์ Startup โลกปี 2025-2026" },
          { type: "bullet", text: "AI-First Startup, Green Tech, HealthTech" },
        ],
        note: "",
      },
    ],
    activities: [
      { title: "บรรยาย", body: "อาจารย์แนะนำรายวิชา\nชี้แจงกติกา เกณฑ์การประเมิน\nและแผนการสอนตลอดภาคเรียน" },
      { title: "อภิปรายกลุ่ม", body: "นักศึกษาแบ่งกลุ่ม\nระดมความคิด:\n'Startup ที่รู้จัก คืออะไร\nทำไมถึงสำเร็จหรือล้มเหลว?'" },
      { title: "กรณีศึกษา", body: "ศึกษา Startup ไทย 1 ราย\nวิเคราะห์จุดเด่น\nและนำเสนอหน้าชั้นเรียน" },
    ],
    takeaways: [
      "เข้าใจความหมายของ Startup และผู้ประกอบการ",
      "รู้จักระบบนิเวศ Startup ในไทยและเทรนด์โลก",
      "เข้าใจกติกาและเกณฑ์การประเมินรายวิชา",
      "เริ่มคิดถึงปัญหาในชีวิตประจำวันที่อยากแก้ไข",
    ],
    next_week: "นวัตกรรมและความคิดสร้างสรรค์ทางธุรกิจ / Design Thinking",
  },
  {
    week: 2,
    title_th: "นวัตกรรมและความคิดสร้างสรรค์ทางธุรกิจ กระบวนการคิดเชิงออกแบบ",
    title_en: "Business Innovation & Creativity / Design Thinking Process",
    clos: "CLO2: อธิบายนวัตกรรมทางธุรกิจ\nCLO3: ค้นหาและพัฒนาไอเดียนวัตกรรม",
    method: "บรรยาย\nเวิร์กชอป\nกิจกรรมกลุ่ม",
    media: "สไลด์\nใบงาน Design Thinking\nVideo",
    slides: [
      {
        title: "นวัตกรรมคืออะไร?",
        bullets: [
          { type: "h1", text: "นิยามนวัตกรรม (Innovation)" },
          { type: "bullet", text: "ไอเดียใหม่ที่สร้างคุณค่าและถูกนำไปใช้จริง" },
          { type: "bullet", text: "ไม่ใช่แค่การประดิษฐ์ — ต้องมี Impact" },
          { type: "h1", text: "ประเภทนวัตกรรม" },
          { type: "bullet", text: "Product Innovation: สินค้า/บริการใหม่" },
          { type: "bullet", text: "Process Innovation: กระบวนการใหม่" },
          { type: "bullet", text: "Business Model Innovation: โมเดลธุรกิจใหม่" },
          { type: "h1", text: "ตัวอย่าง: Netflix เปลี่ยน Business Model" },
          { type: "bullet", text: "จาก DVD rental → Streaming subscription" },
        ],
        note: "นวัตกรรมไม่จำเป็นต้องเป็นเทคโนโลยีเสมอไป",
      },
      {
        title: "Design Thinking 5 ขั้นตอน",
        bullets: [
          { type: "h1", text: "1. Empathize — เข้าใจผู้ใช้" },
          { type: "bullet", text: "สัมภาษณ์, สังเกต, สวมบทบาท (จากมุมมองผู้ใช้)" },
          { type: "h1", text: "2. Define — นิยามปัญหา" },
          { type: "bullet", text: "Point of View (POV) Statement" },
          { type: "h1", text: "3. Ideate — ระดมไอเดีย" },
          { type: "bullet", text: "Brainstorming, SCAMPER, How Might We" },
          { type: "h1", text: "4. Prototype — สร้างต้นแบบ" },
          { type: "bullet", text: "ต้นแบบอย่างรวดเร็ว (Low-Fidelity)" },
          { type: "h1", text: "5. Test — ทดสอบ" },
          { type: "bullet", text: "รับ Feedback จากผู้ใช้จริง → ปรับปรุง" },
        ],
        note: "",
      },
      {
        title: "เครื่องมือ Ideation",
        bullets: [
          { type: "h1", text: "Brainstorming Rules" },
          { type: "bullet", text: "ไม่ตัดสินไอเดียในช่วงระดม" },
          { type: "bullet", text: "ยิ่งมากยิ่งดี (Quantity over Quality)" },
          { type: "h1", text: "SCAMPER Technique" },
          { type: "bullet", text: "S=Substitute, C=Combine, A=Adapt" },
          { type: "bullet", text: "M=Modify, P=Put to other use, E=Eliminate, R=Rearrange" },
          { type: "h1", text: "How Might We (HMW) Questions" },
          { type: "bullet", text: "เปลี่ยนปัญหาเป็นคำถามเปิด" },
          { type: "bullet", text: "เช่น: 'เราจะช่วยให้คนในชนบทเข้าถึงอาหารสดได้อย่างไร?'" },
        ],
        note: "Design Thinking เป็นกระบวนการวนซ้ำ ไม่ใช่เส้นตรง",
      },
    ],
    activities: [
      { title: "บรรยาย", body: "อธิบาย Design Thinking\n5 ขั้นตอนพร้อมตัวอย่าง\nจาก Startup จริง\n(IDEO, Airbnb)" },
      { title: "เวิร์กชอป", body: "ฝึก Empathy Map:\nนักศึกษาเลือก 1 กลุ่มเป้าหมาย\nและวาด Empathy Map\nบน Whiteboard/ใบงาน" },
      { title: "ใบงาน", body: "กลุ่มส่ง POV Statement\n'[ผู้ใช้] ต้องการ [ความต้องการ]\nเพราะ [ข้อมูลเชิงลึก]'\nนำเสนอสั้นๆ 2 นาที" },
    ],
    takeaways: [
      "เข้าใจความหมายและประเภทของนวัตกรรมทางธุรกิจ",
      "รู้จัก Design Thinking 5 ขั้นตอน",
      "ฝึกเขียน Empathy Map และ POV Statement",
      "เริ่มมองปัญหาจากมุมมองของผู้ใช้งาน",
    ],
    next_week: "การค้นหาปัญหา/โอกาสทางธุรกิจ และการพัฒนาไอเดียนวัตกรรม (Ideation)",
  },
  {
    week: 3,
    title_th: "การค้นหาปัญหา/โอกาสทางธุรกิจ และการพัฒนาไอเดียนวัตกรรม",
    title_en: "Problem Discovery & Business Opportunity / Ideation",
    clos: "CLO3: ค้นหาและพัฒนาไอเดียนวัตกรรม\nCLO5: ทำงานร่วมกับผู้อื่นในกลุ่ม",
    method: "ปฏิบัติการกลุ่ม\nระดมสมอง\nนำเสนอ",
    media: "ใบงาน\nเครื่องมือระดมสมอง\nกระดาน Post-it",
    slides: [
      {
        title: "Pain Points และโอกาสทางธุรกิจ",
        bullets: [
          { type: "h1", text: "Pain Points คืออะไร?" },
          { type: "bullet", text: "ปัญหาที่ผู้คนพบในชีวิตประจำวัน" },
          { type: "bullet", text: "ความไม่สะดวก ความเสียเวลา ความสูญเสียเงิน" },
          { type: "h1", text: "3 แหล่งหา Pain Points" },
          { type: "bullet", text: "สังเกตชีวิตประจำวัน (Observation)" },
          { type: "bullet", text: "สัมภาษณ์กลุ่มเป้าหมาย (Interview)" },
          { type: "bullet", text: "ข้อมูล Online Reviews / Social Media" },
          { type: "h1", text: "Pain Point → โอกาส Startup" },
          { type: "bullet", text: "Grab: ปัญหาแท็กซี่ไม่พอ → แอปเรียกรถ" },
          { type: "bullet", text: "Foodpanda: ปัญหาสั่งอาหาร → Delivery App" },
        ],
        note: "ปัญหาที่ดีที่สุดคือปัญหาที่คุณเองก็เจอ",
      },
      {
        title: "Idea Generation Techniques",
        bullets: [
          { type: "h1", text: "Blue Ocean Strategy" },
          { type: "bullet", text: "สร้างตลาดใหม่ที่ไม่มีคู่แข่ง" },
          { type: "bullet", text: "ERRC Framework: Eliminate, Reduce, Raise, Create" },
          { type: "h1", text: "Jobs-to-be-Done Theory" },
          { type: "bullet", text: "ลูกค้าซื้อ 'งาน' ไม่ใช่ 'สินค้า'" },
          { type: "bullet", text: "เช่น: ซื้อสว่านเพื่อได้รู ไม่ใช่ตัวสว่าน" },
          { type: "h1", text: "Trend Analysis" },
          { type: "bullet", text: "Megatrend: AI, Aging Society, Green Economy" },
          { type: "bullet", text: "ใช้ Google Trends, TikTok Trends" },
        ],
        note: "",
      },
      {
        title: "เกณฑ์ประเมินไอเดีย Startup",
        bullets: [
          { type: "h1", text: "Problem-Solution Fit" },
          { type: "bullet", text: "ปัญหาชัดเจนและใหญ่พอ?" },
          { type: "bullet", text: "Solution แก้ปัญหาได้จริง?" },
          { type: "h1", text: "3 มิติประเมินไอเดีย" },
          { type: "bullet", text: "Desirability: คนต้องการไหม?" },
          { type: "bullet", text: "Feasibility: ทำได้จริงไหม?" },
          { type: "bullet", text: "Viability: ทำแล้วมีกำไรไหม?" },
          { type: "h1", text: "เครื่องมือ: Idea Evaluation Matrix" },
          { type: "bullet", text: "ให้คะแนน 1-5 ในแต่ละมิติ" },
          { type: "bullet", text: "เลือกไอเดียที่คะแนนรวมสูงสุด" },
        ],
        note: "ไอเดียที่ดีไม่จำเป็นต้องเป็นไอเดียแรก — ลองหลายๆ ไอเดียก่อน",
      },
    ],
    activities: [
      { title: "Problem Hunting", body: "นักศึกษาออกสำรวจ\nบริเวณมหาวิทยาลัย\nค้นหา 5 Pain Points\nบันทึกลงใบงาน" },
      { title: "Brainstorming", body: "กลุ่มระดมไอเดีย\nด้วย Post-it Note\nไม่ตัดสินไอเดีย\nนาน 15 นาที" },
      { title: "Idea Pitch", body: "แต่ละกลุ่มเลือก\nไอเดียดีที่สุด 1 ข้อ\nนำเสนอ 3 นาที:\nปัญหา / กลุ่มเป้าหมาย / Solution" },
    ],
    takeaways: [
      "สามารถค้นหา Pain Points จากสภาพแวดล้อมรอบตัว",
      "รู้จักเทคนิค Idea Generation (Blue Ocean, JTBD)",
      "ประเมินไอเดียด้วย Desirability-Feasibility-Viability",
      "เริ่มมีไอเดีย Startup เบื้องต้นสำหรับโครงงาน",
    ],
    next_week: "การวิเคราะห์ตลาดและความต้องการของลูกค้า (Customer & Market Validation)",
  },
  {
    week: 4,
    title_th: "การวิเคราะห์ตลาดและความต้องการของลูกค้า",
    title_en: "Customer & Market Validation",
    clos: "CLO3: ค้นหาและพัฒนาไอเดียนวัตกรรม\nCLO5: ทำงานร่วมกับผู้อื่นในกลุ่ม",
    method: "บรรยาย\nปฏิบัติ\nสำรวจตลาด",
    media: "สไลด์\nแบบสำรวจ\nGoogle Forms",
    slides: [
      {
        title: "ทำไมต้องทำ Market Validation?",
        bullets: [
          { type: "h1", text: "ปัญหาของ Startup ส่วนใหญ่" },
          { type: "bullet", text: "42% ของ Startup ล้มเหลวเพราะ 'ไม่มีคนต้องการ'" },
          { type: "bullet", text: "สร้างสิ่งที่ตัวเองอยากสร้าง ≠ สิ่งที่ตลาดต้องการ" },
          { type: "h1", text: "Build-Measure-Learn Loop" },
          { type: "bullet", text: "สร้าง → วัดผล → เรียนรู้ → ปรับปรุง" },
          { type: "bullet", text: "Lean Startup Methodology (Eric Ries)" },
          { type: "h1", text: "Customer Discovery Process" },
          { type: "bullet", text: "กำหนดสมมติฐาน (Assumption)" },
          { type: "bullet", text: "ออกพบลูกค้า (Get out of the building!)" },
        ],
        note: "'ลูกค้าไม่เคยโกหก' — ฟังให้มากกว่าพูด",
      },
      {
        title: "Customer Persona",
        bullets: [
          { type: "h1", text: "Customer Persona คืออะไร?" },
          { type: "bullet", text: "โปรไฟล์ตัวแทนลูกค้าในอุดมคติ" },
          { type: "h1", text: "องค์ประกอบ Persona" },
          { type: "bullet", text: "ชื่อ / อายุ / อาชีพ / รายได้" },
          { type: "bullet", text: "ปัญหา / ความต้องการ / พฤติกรรม" },
          { type: "bullet", text: "ช่องทางที่ใช้ (Social Media, Apps)" },
          { type: "h1", text: "วิธีเก็บข้อมูลสร้าง Persona" },
          { type: "bullet", text: "สัมภาษณ์เชิงลึก 5-10 คน" },
          { type: "bullet", text: "Google Analytics / Social Insight" },
          { type: "bullet", text: "แบบสอบถามออนไลน์" },
        ],
        note: "",
      },
      {
        title: "TAM SAM SOM Framework",
        bullets: [
          { type: "h1", text: "TAM (Total Addressable Market)" },
          { type: "bullet", text: "ขนาดตลาดทั้งหมดในโลก" },
          { type: "bullet", text: "เช่น: ตลาด Food Delivery ไทย = 80,000 ล้านบาท/ปี" },
          { type: "h1", text: "SAM (Serviceable Addressable Market)" },
          { type: "bullet", text: "ส่วนของตลาดที่เราเข้าถึงได้" },
          { type: "bullet", text: "เช่น: ภาคใต้ + กลุ่ม Gen Z = 5,000 ล้านบาท/ปี" },
          { type: "h1", text: "SOM (Serviceable Obtainable Market)" },
          { type: "bullet", text: "ส่วนตลาดที่จะ 'ได้จริง' ใน 1-3 ปีแรก" },
          { type: "bullet", text: "เช่น: 1% ของ SAM = 50 ล้านบาท/ปี" },
        ],
        note: "SOM ต้องสมเหตุสมผล — นักลงทุนดูตรงนี้เป็นพิเศษ",
      },
    ],
    activities: [
      { title: "Mini Survey", body: "กลุ่มออกแบบ\nแบบสำรวจ 5-8 ข้อ\nใน Google Forms\nเกี่ยวกับไอเดียของกลุ่ม" },
      { title: "สัมภาษณ์", body: "สัมภาษณ์นักศึกษา\nและบุคลากร 5 คน\nบันทึกข้อมูลจริง\n(ไม่ใช่การสมมติ)" },
      { title: "วิเคราะห์", body: "สรุปผล Survey\nสร้าง Customer Persona\nและประมาณ TAM/SAM/SOM\nนำเสนอ 5 นาที" },
    ],
    takeaways: [
      "เข้าใจว่า Market Validation สำคัญกว่าการสร้างสินค้าทันที",
      "สร้าง Customer Persona จากข้อมูลจริง",
      "ประมาณขนาดตลาดด้วย TAM/SAM/SOM",
      "ฝึกทักษะการสัมภาษณ์และการฟัง",
    ],
    next_week: "แบบจำลองธุรกิจ Business Model Canvas (BMC)",
  },
  {
    week: 5,
    title_th: "แบบจำลองธุรกิจ (Business Model Canvas)",
    title_en: "Business Model Canvas (BMC)",
    clos: "CLO3: จัดทำและนำเสนอแผนธุรกิจ\nCLO5: ทำงานร่วมกับผู้อื่นในกลุ่ม",
    method: "เวิร์กชอป\nกิจกรรมกลุ่ม",
    media: "BMC Template\nPost-it\nDigital Canvas",
    slides: [
      {
        title: "Business Model Canvas คืออะไร?",
        bullets: [
          { type: "h1", text: "BMC: เครื่องมือออกแบบโมเดลธุรกิจ" },
          { type: "bullet", text: "พัฒนาโดย Alexander Osterwalder" },
          { type: "bullet", text: "1 หน้ากระดาษ แทน Business Plan 30 หน้า" },
          { type: "h1", text: "9 Building Blocks" },
          { type: "bullet", text: "1. Customer Segments (CS)" },
          { type: "bullet", text: "2. Value Propositions (VP)" },
          { type: "bullet", text: "3. Channels (CH)" },
          { type: "bullet", text: "4. Customer Relationships (CR)" },
          { type: "bullet", text: "5. Revenue Streams (RS)" },
          { type: "bullet", text: "6. Key Resources (KR)" },
          { type: "bullet", text: "7. Key Activities (KA)" },
          { type: "bullet", text: "8. Key Partnerships (KP)" },
          { type: "bullet", text: "9. Cost Structure (CS)" },
        ],
        note: "BMC เป็นภาพรวมธุรกิจทั้งหมดในหน้าเดียว",
      },
      {
        title: "4 ด้านหลักของ BMC",
        bullets: [
          { type: "h2", text: "ด้านขวา: Customer Side (รายรับ)" },
          { type: "h1", text: "Customer Segments" },
          { type: "bullet", text: "กลุ่มลูกค้าที่เจาะจง — B2C, B2B, Niche" },
          { type: "h1", text: "Value Propositions" },
          { type: "bullet", text: "คุณค่าที่เราให้กับลูกค้า (ทำไมเลือกเรา?)" },
          { type: "h1", text: "Channels & Customer Relationships" },
          { type: "bullet", text: "ช่องทางถึงลูกค้า / วิธีรักษาความสัมพันธ์" },
          { type: "h2", text: "ด้านซ้าย: Company Side (ต้นทุน)" },
          { type: "h1", text: "Key Resources / Activities / Partnerships" },
          { type: "bullet", text: "ทรัพยากร กิจกรรมหลัก และพันธมิตร" },
          { type: "h1", text: "Cost Structure" },
          { type: "bullet", text: "ต้นทุนคงที่และต้นทุนผันแปร" },
        ],
        note: "",
      },
      {
        title: "Revenue Models ที่พบใน Startup",
        bullets: [
          { type: "h1", text: "Subscription Model" },
          { type: "bullet", text: "Netflix, Spotify — จ่ายรายเดือน" },
          { type: "h1", text: "Freemium Model" },
          { type: "bullet", text: "LINE, Canva — ใช้ฟรี + Premium features" },
          { type: "h1", text: "Marketplace Model" },
          { type: "bullet", text: "Shopee, Grab — ค่าธรรมเนียมต่อธุรกรรม" },
          { type: "h1", text: "SaaS (Software as a Service)" },
          { type: "bullet", text: "B2B software — บริการซอฟต์แวร์ออนไลน์" },
          { type: "h1", text: "Advertising Model" },
          { type: "bullet", text: "Facebook, TikTok — รายได้จากโฆษณา" },
        ],
        note: "เลือก Revenue Model ที่เหมาะกับธุรกิจ อย่าลอกแบบ",
      },
    ],
    activities: [
      { title: "Workshop BMC", body: "แต่ละกลุ่มรับ\nBMC Template\n(กระดาษ A1 หรือ Miro)\nกรอก 9 ช่องให้ครบ" },
      { title: "Gallery Walk", body: "ติด BMC บนผนัง\nกลุ่มอื่น ๆ เดินดู\nและแปะ Post-it\nข้อดี / ข้อสงสัย" },
      { title: "Feedback", body: "แต่ละกลุ่มนำเสนอ BMC\n5 นาที + รับ Feedback\nจากอาจารย์และเพื่อน\nปรับปรุง BMC" },
    ],
    takeaways: [
      "เข้าใจ 9 Building Blocks ของ Business Model Canvas",
      "สามารถออกแบบ BMC สำหรับ Startup ของกลุ่มได้",
      "รู้จัก Revenue Models แบบต่างๆ",
      "ฝึกให้ Feedback และรับ Feedback อย่างสร้างสรรค์",
    ],
    next_week: "คุณค่าที่นำเสนอ (Value Proposition) และ MVP",
  },
  {
    week: 6,
    title_th: "คุณค่าที่นำเสนอ (Value Proposition) และต้นแบบผลิตภัณฑ์ขั้นต่ำ (MVP)",
    title_en: "Value Proposition & Minimum Viable Product (MVP)",
    clos: "CLO3: จัดทำและนำเสนอแผนธุรกิจ\nCLO4: ผลิตสื่อและชิ้นงานโดยใช้เครื่องมือดิจิทัล",
    method: "ปฏิบัติการกลุ่ม\nออกแบบต้นแบบ",
    media: "ใบงาน\nเครื่องมือต้นแบบ (Figma/Canva)",
    slides: [
      {
        title: "Value Proposition Canvas",
        bullets: [
          { type: "h1", text: "Value Proposition Canvas (VPC)" },
          { type: "bullet", text: "เชื่อมโยง Customer Profile ↔ Value Map" },
          { type: "h1", text: "Customer Profile (วงกลมขวา)" },
          { type: "bullet", text: "Jobs: งานที่ลูกค้าต้องทำให้สำเร็จ" },
          { type: "bullet", text: "Pains: ความเจ็บปวด/อุปสรรค" },
          { type: "bullet", text: "Gains: ผลลัพธ์ที่ต้องการ" },
          { type: "h1", text: "Value Map (สี่เหลี่ยมซ้าย)" },
          { type: "bullet", text: "Products & Services: สิ่งที่เรานำเสนอ" },
          { type: "bullet", text: "Pain Relievers: บรรเทาความเจ็บปวด" },
          { type: "bullet", text: "Gain Creators: สร้างผลลัพธ์ที่ต้องการ" },
        ],
        note: "Product-Market Fit เกิดเมื่อ Value Map ตรงกับ Customer Profile",
      },
      {
        title: "Unique Value Proposition (UVP)",
        bullets: [
          { type: "h1", text: "UVP คืออะไร?" },
          { type: "bullet", text: "ประโยคเดียวที่บอกว่าเราช่วยใคร ทำอะไร ต่างจากใคร" },
          { type: "h1", text: "สูตร UVP ง่ายๆ" },
          { type: "bullet", text: "เราช่วย [กลุ่มเป้าหมาย]" },
          { type: "bullet", text: "ให้ [คุณประโยชน์หลัก]" },
          { type: "bullet", text: "ต่างจากคู่แข่งตรงที่ [ความแตกต่าง]" },
          { type: "h1", text: "ตัวอย่าง UVP จาก Startup จริง" },
          { type: "bullet", text: "Slack: 'Be more productive at work with less effort'" },
          { type: "bullet", text: "Airbnb: 'Belong anywhere'" },
          { type: "bullet", text: "Grab: 'Everyday Everything App'" },
        ],
        note: "",
      },
      {
        title: "MVP: Minimum Viable Product",
        bullets: [
          { type: "h1", text: "MVP คืออะไร?" },
          { type: "bullet", text: "ผลิตภัณฑ์ขั้นต่ำที่ทดสอบสมมติฐานหลักได้" },
          { type: "bullet", text: "ใช้ทรัพยากรน้อยที่สุด เรียนรู้ให้เร็วที่สุด" },
          { type: "h1", text: "ประเภท MVP" },
          { type: "bullet", text: "Landing Page MVP (เว็บเพจง่ายๆ)" },
          { type: "bullet", text: "Concierge MVP (ทำด้วยมือก่อน)" },
          { type: "bullet", text: "Wizard of Oz MVP (จำลองระบบ)" },
          { type: "bullet", text: "Paper Prototype (ต้นแบบกระดาษ)" },
          { type: "h1", text: "กรณีศึกษา: Dropbox MVP" },
          { type: "bullet", text: "แค่วิดีโอ 3 นาที → มีคนลงทะเบียน 75,000 คน" },
        ],
        note: "MVP ≠ สินค้าที่ด้อยคุณภาพ — แต่คือการเรียนรู้ที่เร็วที่สุด",
      },
    ],
    activities: [
      { title: "VPC Workshop", body: "กลุ่มกรอก\nValue Proposition Canvas\nสำหรับ Startup ของตน\nบน Miro หรือใบงาน" },
      { title: "UVP Writing", body: "แต่ละกลุ่มเขียน UVP\nในเวลา 10 นาที\nนำเสนอและรับ Feedback\nจากเพื่อนในชั้น" },
      { title: "MVP Design", body: "ออกแบบ MVP:\nเลือกประเภท MVP\nวาด Wireframe/Sketch\nหรือสร้าง Landing Page\nด้วย Canva" },
    ],
    takeaways: [
      "เข้าใจ Value Proposition Canvas และหาจุดตรงกัน",
      "เขียน UVP ที่ชัดเจนสำหรับ Startup ของกลุ่ม",
      "รู้จักประเภท MVP และวิธีเลือกให้เหมาะสม",
      "เริ่มออกแบบ Prototype เบื้องต้นของ Startup",
    ],
    next_week: "ระบบโลจิสติกส์ การขนส่ง และคลังสินค้าสำหรับธุรกิจเริ่มต้น",
  },
  {
    week: 7,
    title_th: "ระบบโลจิสติกส์ การขนส่ง และคลังสินค้าสำหรับธุรกิจเริ่มต้น",
    title_en: "Logistics, Transportation & Warehouse Management for Startups",
    clos: "CLO2: อธิบายพื้นฐานระบบโลจิสติกส์ การขนส่ง และคลังสินค้า",
    method: "บรรยาย\nกรณีศึกษา\nอภิปราย",
    media: "สไลด์\nกรณีศึกษา\nVideo",
    slides: [
      {
        title: "โลจิสติกส์และ Supply Chain",
        bullets: [
          { type: "h1", text: "โลจิสติกส์ (Logistics) คืออะไร?" },
          { type: "bullet", text: "การวางแผนและจัดการการเคลื่อนย้ายสินค้า" },
          { type: "bullet", text: "จากต้นทางไปยังปลายทางอย่างมีประสิทธิภาพ" },
          { type: "h1", text: "Supply Chain Management (SCM)" },
          { type: "bullet", text: "Supplier → ผู้ผลิต → คลังสินค้า → ขนส่ง → ลูกค้า" },
          { type: "h1", text: "ความสำคัญต่อ Startup" },
          { type: "bullet", text: "ลด Cost ได้มากถึง 30-50% จากโลจิสติกส์ที่ดี" },
          { type: "bullet", text: "Customer Experience: ส่งเร็ว ส่งถูก ส่งตรง" },
        ],
        note: "โลจิสติกส์ที่ดีคือ Competitive Advantage ที่มองไม่เห็น",
      },
      {
        title: "การจัดการคลังสินค้า",
        bullets: [
          { type: "h1", text: "Warehouse Management System (WMS)" },
          { type: "bullet", text: "ระบบจัดการสินค้าในคลังสินค้าด้วยซอฟต์แวร์" },
          { type: "h1", text: "หลักการ FIFO / FEFO" },
          { type: "bullet", text: "FIFO: First In, First Out" },
          { type: "bullet", text: "FEFO: First Expired, First Out (อาหาร/ยา)" },
          { type: "h1", text: "Inventory Management" },
          { type: "bullet", text: "Safety Stock: สต๊อกสำรองฉุกเฉิน" },
          { type: "bullet", text: "Reorder Point: จุดที่ต้องสั่งสินค้าใหม่" },
          { type: "h1", text: "Just-In-Time (JIT)" },
          { type: "bullet", text: "สั่งสินค้าเมื่อต้องการ ลดต้นทุนการเก็บสต๊อก" },
        ],
        note: "",
      },
      {
        title: "Last-Mile Delivery และ Startup",
        bullets: [
          { type: "h1", text: "Last-Mile Delivery" },
          { type: "bullet", text: "การส่งสินค้าจากคลังถึงมือลูกค้า" },
          { type: "bullet", text: "ต้นทุนสูงสุด 53% ของค่าขนส่งทั้งหมด" },
          { type: "h1", text: "โมเดลส่งสินค้าสำหรับ Startup ไทย" },
          { type: "bullet", text: "3PL (Third-Party Logistics): Flash, Kerry, J&T" },
          { type: "bullet", text: "Dropshipping: ไม่ต้องมีสต๊อก" },
          { type: "bullet", text: "Fulfillment Center: ฝากเก็บ/แพ็ค/ส่ง" },
          { type: "h1", text: "E-commerce Logistics Trend" },
          { type: "bullet", text: "Same-day delivery, Drone delivery" },
          { type: "bullet", text: "Sustainable Packaging" },
        ],
        note: "Startup ส่วนใหญ่ใช้ 3PL ก่อน — ไม่ต้องลงทุนคลังสินค้าเอง",
      },
    ],
    activities: [
      { title: "บรรยาย", body: "อธิบายระบบโลจิสติกส์\nSupply Chain\nและการจัดการคลังสินค้า\nพร้อม Diagram" },
      { title: "กรณีศึกษา", body: "วิเคราะห์กรณีศึกษา:\n'Startup อาหารสด'\nมีปัญหาโลจิสติกส์อะไร?\nแก้ไขอย่างไร?" },
      { title: "อภิปรายกลุ่ม", body: "แต่ละกลุ่มวาง\nStrategy โลจิสติกส์\nสำหรับ Startup ของตน\n(ใช้ 3PL ตัวไหน? ทำไม?)" },
    ],
    takeaways: [
      "เข้าใจระบบ Supply Chain และโลจิสติกส์พื้นฐาน",
      "รู้จักการจัดการคลังสินค้าและ Inventory",
      "เลือกโมเดลขนส่งที่เหมาะกับ Startup ของตนได้",
      "เข้าใจความสำคัญของ Last-Mile Delivery ต่อ Customer Experience",
    ],
    next_week: "การวางแผนการเงินและการประเมินความเป็นไปได้ของธุรกิจ",
  },
  {
    week: 8,
    title_th: "การวางแผนการเงินและการประเมินความเป็นไปได้ของธุรกิจ",
    title_en: "Financial Planning & Business Feasibility Assessment",
    clos: "CLO3: จัดทำและนำเสนอแผนธุรกิจและกลยุทธ์การจัดการ",
    method: "บรรยาย\nปฏิบัติ\nวิเคราะห์ตัวเลข",
    media: "สไลด์\nตารางการเงิน (Excel/Sheets)",
    slides: [
      {
        title: "Financial Planning สำหรับ Startup",
        bullets: [
          { type: "h1", text: "ทำไม Startup ต้องวางแผนการเงิน?" },
          { type: "bullet", text: "38% ของ Startup ล้มเหลวเพราะเงินหมด (Cash Flow)" },
          { type: "bullet", text: "นักลงทุนดูตัวเลขการเงินก่อนตัดสินใจ" },
          { type: "h1", text: "งบการเงินหลัก 3 รายการ" },
          { type: "bullet", text: "Income Statement: รายได้ - ค่าใช้จ่าย = กำไร" },
          { type: "bullet", text: "Balance Sheet: สินทรัพย์ = หนี้สิน + ทุน" },
          { type: "bullet", text: "Cash Flow Statement: เงินสดเข้า-ออก" },
          { type: "h1", text: "Unit Economics" },
          { type: "bullet", text: "CAC (Customer Acquisition Cost): ต้นทุนหาลูกค้าใหม่ 1 ราย" },
          { type: "bullet", text: "LTV (Lifetime Value): มูลค่าตลอดชีวิตของลูกค้า" },
        ],
        note: "LTV ต้องมากกว่า CAC อย่างน้อย 3 เท่า — กฎทอง Startup",
      },
      {
        title: "การประมาณการรายได้และค่าใช้จ่าย",
        bullets: [
          { type: "h1", text: "Revenue Projection (3 ปี)" },
          { type: "bullet", text: "Year 1: ทดสอบตลาด — ไม่ต้องกำไร" },
          { type: "bullet", text: "Year 2: เติบโต — เริ่มคุ้มทุน" },
          { type: "bullet", text: "Year 3: Scale — กำไรชัดเจน" },
          { type: "h1", text: "ต้นทุนหลักของ Startup" },
          { type: "bullet", text: "Fixed Cost: เงินเดือน ค่าเช่า ค่าซอฟต์แวร์" },
          { type: "bullet", text: "Variable Cost: ต้นทุนสินค้า ค่าขนส่ง" },
          { type: "h1", text: "Break-Even Analysis" },
          { type: "bullet", text: "จุดคุ้มทุน = Fixed Cost ÷ (ราคา - VC ต่อหน่วย)" },
          { type: "bullet", text: "ต้องขายเท่าไหร่จึงจะคุ้มทุน?" },
        ],
        note: "",
      },
      {
        title: "Funding และแหล่งเงินทุน Startup",
        bullets: [
          { type: "h1", text: "Bootstrapping" },
          { type: "bullet", text: "ใช้เงินตัวเอง / เพื่อน / ครอบครัว (FFF)" },
          { type: "bullet", text: "ควบคุมได้ 100% แต่เงินจำกัด" },
          { type: "h1", text: "Grant และทุนภาครัฐ" },
          { type: "bullet", text: "NIA: ทุน Startup ไทย (สูงสุด 1 ล้านบาท)" },
          { type: "bullet", text: "depa: ทุนดิจิทัล SME" },
          { type: "h1", text: "Angel Investor" },
          { type: "bullet", text: "บุคคลที่ลงทุนในระยะแรก" },
          { type: "bullet", text: "ได้เงิน + Mentor + เครือข่าย" },
          { type: "h1", text: "Venture Capital (VC)" },
          { type: "bullet", text: "ระยะ Series A ขึ้นไป — Valuation สูง" },
        ],
        note: "เริ่มด้วย Bootstrapping หรือ Grant — อย่ารีบขายหุ้น",
      },
    ],
    activities: [
      { title: "Workshop Excel", body: "กลุ่มสร้าง\nFinancial Model\nบน Google Sheets:\nรายได้ / ค่าใช้จ่าย / กำไร\n3 ปีข้างหน้า" },
      { title: "Break-Even", body: "คำนวณจุดคุ้มทุน\nของ Startup กลุ่มตนเอง\nต้องขายกี่หน่วย?\nใช้เวลากี่เดือน?" },
      { title: "Pitch Finance", body: "นำเสนอตัวเลขการเงิน\n5 สไลด์:\nRevenue / Cost / Profit\nBreak-Even / Funding Plan" },
    ],
    takeaways: [
      "เข้าใจงบการเงินหลัก 3 รายการสำหรับ Startup",
      "คำนวณ CAC, LTV และ Break-Even Point ได้",
      "วาง Revenue Projection 3 ปีให้สมเหตุสมผล",
      "รู้จักแหล่งทุนและเลือกแหล่งทุนที่เหมาะกับระยะของธุรกิจ",
    ],
    next_week: "กลยุทธ์การตลาดดิจิทัลและการเติบโตของธุรกิจ (Growth)",
  },
]
```

- [ ] **Step 2: Verify data integrity**

```bash
node --input-type=module << 'EOF'
import { weeks } from './src/data/weeks.js'
console.log('Total weeks:', weeks.length)
weeks.forEach(w => {
  if (!w.title_th || !w.slides?.length || !w.activities?.length || !w.takeaways?.length)
    throw new Error('Week ' + w.week + ' missing required field')
})
console.log('All weeks valid ✓')
EOF
```

Expected:
```
Total weeks: 8
All weeks valid ✓
```

- [ ] **Step 3: Commit**

```bash
git add src/data/weeks.js
git commit -m "feat: add course data for all 8 weeks"
```

---

## Task 3: NavBar Component

**Files:**
- Create: `src/components/NavBar.vue`
- Modify: `src/App.vue`

- [ ] **Step 1: Create `src/components/NavBar.vue`**

```vue
<script setup>
const weekNums = Array.from({ length: 8 }, (_, i) => i + 1)

function scrollTo(id) {
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
}
</script>

<template>
  <nav class="navbar">
    <div class="navbar-brand">
      <span class="navbar-code">07-034-266</span>
      <span class="navbar-name">การสร้างธุรกิจเริ่มต้นฯ</span>
    </div>
    <div class="navbar-links">
      <button
        v-for="w in weekNums"
        :key="w"
        class="week-link"
        @click="scrollTo(`week-${w}`)"
      >
        W{{ w }}
      </button>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.75rem 2rem;
  background: var(--text);
  color: white;
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
  gap: 1rem;
  flex-wrap: wrap;
}

.navbar-brand {
  display: flex;
  flex-direction: column;
  line-height: 1.2;
}

.navbar-code {
  font-size: 0.75rem;
  color: var(--accent);
  font-weight: 600;
}

.navbar-name {
  font-size: 0.9rem;
  font-weight: 700;
}

.navbar-links {
  display: flex;
  gap: 0.4rem;
  flex-wrap: wrap;
}

.week-link {
  background: transparent;
  border: 1px solid var(--accent);
  color: var(--accent);
  padding: 0.25rem 0.6rem;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.85rem;
  font-family: 'Sarabun', sans-serif;
  transition: background 0.2s, color 0.2s;
}

.week-link:hover {
  background: var(--accent);
  color: var(--text);
}
</style>
```

- [ ] **Step 2: Update `src/App.vue` to include NavBar**

```vue
<script setup>
import NavBar from './components/NavBar.vue'
</script>

<template>
  <NavBar />
  <main>
    <p style="padding: 2rem">Content coming soon…</p>
  </main>
</template>
```

- [ ] **Step 3: Verify in browser**

Open `http://localhost:5173`. Expected: dark sticky navbar with cyan "07-034-266" and 8 W1–W8 buttons that turn cyan-filled on hover.

- [ ] **Step 4: Commit**

```bash
git add src/components/NavBar.vue src/App.vue
git commit -m "feat: add sticky NavBar with week anchor buttons"
```

---

## Task 4: HeroSection Component

**Files:**
- Create: `src/components/HeroSection.vue`
- Modify: `src/App.vue`

- [ ] **Step 1: Create `src/components/HeroSection.vue`**

```vue
<template>
  <section class="hero">
    <div class="hero-inner">
      <div class="hero-badge">07-034-266</div>
      <h1 class="hero-title-th">การสร้างธุรกิจเริ่มต้นด้วยนวัตกรรมและเทคโนโลยี</h1>
      <h2 class="hero-title-en">Starting a Business with Innovation and Technology</h2>
      <div class="hero-divider" />
      <div class="hero-meta">
        <div class="meta-item">
          <span class="meta-label">ผู้สอน</span>
          <span class="meta-value">อาจารย์ ดร.สุรเชษฐ์ สังขพันธ์</span>
        </div>
        <div class="meta-item">
          <span class="meta-label">สังกัด</span>
          <span class="meta-value">คณะวิทยาการจัดการ มหาวิทยาลัยนราธิวาสราชนครินทร์</span>
        </div>
        <div class="meta-item">
          <span class="meta-label">หน่วยกิต</span>
          <span class="meta-value">3 หน่วยกิต (2-2-5) · 8 สัปดาห์</span>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  background: linear-gradient(135deg, var(--text) 0%, #1a3a5c 100%);
  color: white;
  padding: 4rem 2rem;
  text-align: center;
}

.hero-inner {
  max-width: 800px;
  margin: 0 auto;
}

.hero-badge {
  display: inline-block;
  background: var(--accent);
  color: var(--text);
  font-weight: 700;
  font-size: 0.9rem;
  padding: 0.3rem 1rem;
  border-radius: 20px;
  margin-bottom: 1.5rem;
}

.hero-title-th {
  font-size: clamp(1.4rem, 3vw, 2.2rem);
  font-weight: 700;
  line-height: 1.4;
  margin-bottom: 0.75rem;
}

.hero-title-en {
  font-size: clamp(0.9rem, 2vw, 1.2rem);
  font-weight: 400;
  color: var(--accent);
  margin-bottom: 2rem;
}

.hero-divider {
  width: 60px;
  height: 3px;
  background: var(--highlight);
  margin: 0 auto 2rem;
  border-radius: 2px;
}

.hero-meta {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2rem;
}

.meta-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
}

.meta-label {
  font-size: 0.75rem;
  color: var(--light);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.meta-value {
  font-size: 0.95rem;
  font-weight: 600;
}
</style>
```

- [ ] **Step 2: Update `src/App.vue`**

```vue
<script setup>
import NavBar from './components/NavBar.vue'
import HeroSection from './components/HeroSection.vue'
</script>

<template>
  <NavBar />
  <HeroSection />
  <main>
    <p style="padding: 2rem">Week sections coming soon…</p>
  </main>
</template>
```

- [ ] **Step 3: Verify in browser**

Open `http://localhost:5173`. Expected: dark gradient hero banner with course title, English subtitle in cyan, amber divider line, and 3 metadata items (instructor / faculty / credits).

- [ ] **Step 4: Commit**

```bash
git add src/components/HeroSection.vue src/App.vue
git commit -m "feat: add HeroSection with course and instructor info"
```

---

## Task 5: WeekSection Component

**Files:**
- Create: `src/components/WeekSection.vue`
- Modify: `src/App.vue`

- [ ] **Step 1: Create `src/components/WeekSection.vue`**

```vue
<script setup>
defineProps({
  week: {
    type: Object,
    required: true,
  },
})

function slideFile(weekNum) {
  return `/slides/Week${String(weekNum).padStart(2, '0')}_Startup.pptx`
}
</script>

<template>
  <section :id="`week-${week.week}`" class="week-section">

    <!-- Header -->
    <div class="week-header">
      <div class="week-badge">สัปดาห์ที่ {{ week.week }}</div>
      <div class="week-titles">
        <h2 class="week-title-th">{{ week.title_th }}</h2>
        <p class="week-title-en">{{ week.title_en }}</p>
      </div>
    </div>

    <!-- CLO / Method / Media chips -->
    <div class="week-chips">
      <div class="chip">
        <span class="chip-label">CLOs</span>
        <span class="chip-value">{{ week.clos }}</span>
      </div>
      <div class="chip">
        <span class="chip-label">วิธีการสอน</span>
        <span class="chip-value">{{ week.method }}</span>
      </div>
      <div class="chip">
        <span class="chip-label">สื่อ</span>
        <span class="chip-value">{{ week.media }}</span>
      </div>
    </div>

    <!-- Slide cards -->
    <div class="slides-grid">
      <div v-for="(slide, i) in week.slides" :key="i" class="slide-card">
        <div class="slide-card-header">{{ slide.title }}</div>
        <ul class="slide-bullets">
          <li
            v-for="(b, j) in slide.bullets"
            :key="j"
            :class="`bullet-${b.type}`"
          >{{ b.text }}</li>
        </ul>
        <p v-if="slide.note" class="slide-note">💡 {{ slide.note }}</p>
      </div>
    </div>

    <!-- Activities -->
    <h3 class="section-sub-title">กิจกรรม</h3>
    <div class="activities-grid">
      <div v-for="(act, i) in week.activities" :key="i" class="activity-card">
        <div class="activity-title">{{ act.title }}</div>
        <p class="activity-body">{{ act.body }}</p>
      </div>
    </div>

    <!-- Takeaways -->
    <h3 class="section-sub-title">Key Takeaways</h3>
    <ul class="takeaway-list">
      <li v-for="(t, i) in week.takeaways" :key="i" class="takeaway-item">
        <span class="checkmark">✓</span> {{ t }}
      </li>
    </ul>

    <!-- Next week preview -->
    <p class="next-week">สัปดาห์หน้า: <strong>{{ week.next_week }}</strong></p>

    <!-- Download button -->
    <a
      :href="slideFile(week.week)"
      :download="`Week${String(week.week).padStart(2,'0')}_Startup.pptx`"
      class="download-btn"
    >
      ⬇ ดาวน์โหลดสไลด์ Week {{ week.week }} (.pptx)
    </a>

  </section>
</template>

<style scoped>
.week-section {
  max-width: 1100px;
  margin: 3rem auto;
  padding: 0 1.5rem 3rem;
  border-bottom: 1px solid var(--card-border);
}

.week-header {
  display: flex;
  align-items: flex-start;
  gap: 1.5rem;
  margin-bottom: 1.5rem;
}

.week-badge {
  flex-shrink: 0;
  background: var(--accent);
  color: white;
  font-weight: 700;
  font-size: 0.85rem;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  text-align: center;
  min-width: 110px;
}

.week-title-th {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--text);
  line-height: 1.4;
}

.week-title-en {
  font-size: 1rem;
  color: var(--accent);
  margin-top: 0.25rem;
}

.week-chips {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  margin-bottom: 2rem;
}

.chip {
  background: var(--card);
  border: 1px solid var(--card-border);
  border-radius: 8px;
  padding: 0.75rem 1rem;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  flex: 1;
  min-width: 160px;
  white-space: pre-line;
}

.chip-label {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--highlight);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.chip-value {
  font-size: 0.85rem;
  color: var(--text);
}

.slides-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.slide-card {
  background: var(--card);
  border: 1px solid var(--card-border);
  border-top: 3px solid var(--accent);
  border-radius: 8px;
  padding: 1rem 1.25rem;
}

.slide-card-header {
  font-weight: 700;
  font-size: 1rem;
  color: var(--text);
  margin-bottom: 0.75rem;
}

.slide-bullets {
  list-style: none;
  font-size: 0.875rem;
  line-height: 1.7;
}

.bullet-h1 {
  font-weight: 700;
  color: var(--accent);
  margin-top: 0.5rem;
}

.bullet-h2 {
  font-weight: 700;
  color: var(--highlight);
  margin-top: 0.75rem;
  font-size: 0.95rem;
}

.bullet-bullet::before {
  content: '• ';
  color: var(--accent);
}

.slide-note {
  margin-top: 0.75rem;
  font-size: 0.8rem;
  color: var(--highlight);
  font-style: italic;
  border-top: 1px solid var(--card-border);
  padding-top: 0.5rem;
}

.section-sub-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 1rem;
  padding-left: 0.75rem;
  border-left: 4px solid var(--accent);
}

.activities-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.activity-card {
  background: var(--card);
  border: 1px solid var(--card-border);
  border-top: 3px solid var(--highlight);
  border-radius: 8px;
  padding: 1rem;
}

.activity-title {
  font-weight: 700;
  font-size: 0.9rem;
  color: var(--highlight);
  margin-bottom: 0.5rem;
}

.activity-body {
  font-size: 0.85rem;
  color: var(--text);
  white-space: pre-line;
  line-height: 1.6;
}

.takeaway-list {
  list-style: none;
  margin-bottom: 1rem;
}

.takeaway-item {
  display: flex;
  align-items: baseline;
  gap: 0.5rem;
  font-size: 0.95rem;
  padding: 0.4rem 0;
  border-bottom: 1px dashed var(--card-border);
}

.checkmark {
  color: var(--accent);
  font-weight: 700;
  flex-shrink: 0;
}

.next-week {
  font-size: 0.9rem;
  color: #666;
  margin: 1rem 0 1.5rem;
}

.download-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: var(--accent);
  color: white;
  font-weight: 700;
  font-size: 0.95rem;
  padding: 0.75rem 1.5rem;
  border-radius: 8px;
  text-decoration: none;
  transition: background 0.2s, transform 0.1s;
}

.download-btn:hover {
  background: #009bb8;
  transform: translateY(-1px);
}
</style>
```

- [ ] **Step 2: Wire up all components in `src/App.vue`**

```vue
<script setup>
import NavBar from './components/NavBar.vue'
import HeroSection from './components/HeroSection.vue'
import WeekSection from './components/WeekSection.vue'
import { weeks } from './data/weeks.js'
</script>

<template>
  <NavBar />
  <HeroSection />
  <main>
    <WeekSection v-for="w in weeks" :key="w.week" :week="w" />
  </main>
</template>
```

- [ ] **Step 3: Verify in browser**

Open `http://localhost:5173`. Check:
- All 8 week sections render with title, CLO/method/media chips, slide cards, activity cards, takeaway list
- Clicking W1–W8 in navbar scrolls to the correct section
- Download button is visible for each week (clicking shows 404 until Task 7 copies the files)

- [ ] **Step 4: Commit**

```bash
git add src/components/WeekSection.vue src/App.vue
git commit -m "feat: add WeekSection with slides, activities, takeaways, and download button"
```

---

## Task 6: FooterBar Component

**Files:**
- Create: `src/components/FooterBar.vue`
- Modify: `src/App.vue`

- [ ] **Step 1: Create `src/components/FooterBar.vue`**

```vue
<template>
  <footer class="footer">
    <div class="footer-inner">
      <p class="footer-course">07-034-266 การสร้างธุรกิจเริ่มต้นด้วยนวัตกรรมและเทคโนโลยี</p>
      <p class="footer-instructor">อาจารย์ ดร.สุรเชษฐ์ สังขพันธ์</p>
      <p class="footer-faculty">คณะวิทยาการจัดการ · มหาวิทยาลัยนราธิวาสราชนครินทร์</p>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  background: var(--text);
  color: var(--light);
  text-align: center;
  padding: 2rem 1.5rem;
  margin-top: 2rem;
  font-size: 0.9rem;
  line-height: 1.8;
}

.footer-course {
  font-weight: 700;
  color: var(--accent);
  margin-bottom: 0.25rem;
}

.footer-instructor {
  color: white;
}

.footer-faculty {
  color: var(--light);
  font-size: 0.8rem;
}
</style>
```

- [ ] **Step 2: Add FooterBar to `src/App.vue`**

```vue
<script setup>
import NavBar from './components/NavBar.vue'
import HeroSection from './components/HeroSection.vue'
import WeekSection from './components/WeekSection.vue'
import FooterBar from './components/FooterBar.vue'
import { weeks } from './data/weeks.js'
</script>

<template>
  <NavBar />
  <HeroSection />
  <main>
    <WeekSection v-for="w in weeks" :key="w.week" :week="w" />
  </main>
  <FooterBar />
</template>
```

- [ ] **Step 3: Verify in browser**

Scroll to bottom of page. Expected: dark footer with cyan course name, white instructor name, and light-blue faculty text.

- [ ] **Step 4: Commit**

```bash
git add src/components/FooterBar.vue src/App.vue
git commit -m "feat: add FooterBar with course and instructor details"
```

---

## Task 7: Slide Assets + Download Links

**Files:**
- Copy: `slides/Week*.pptx` → `public/slides/`

- [ ] **Step 1: Copy slide files to `public/slides/`**

```bash
mkdir -p public/slides
cp slides/Week01_Startup.pptx public/slides/
cp slides/Week02_Startup.pptx public/slides/
cp slides/Week03_Startup.pptx public/slides/
cp slides/Week04_Startup.pptx public/slides/
cp slides/Week05_Startup.pptx public/slides/
cp slides/Week06_Startup.pptx public/slides/
cp slides/Week07_Startup.pptx public/slides/
cp slides/Week08_Startup.pptx public/slides/
```

- [ ] **Step 2: Verify files are present**

```bash
ls public/slides/
```

Expected:
```
Week01_Startup.pptx  Week05_Startup.pptx
Week02_Startup.pptx  Week06_Startup.pptx
Week03_Startup.pptx  Week07_Startup.pptx
Week04_Startup.pptx  Week08_Startup.pptx
```

- [ ] **Step 3: Test download in browser**

Open `http://localhost:5173`. Click "⬇ ดาวน์โหลดสไลด์ Week 1 (.pptx)".  
Expected: `Week01_Startup.pptx` downloads with no 404 error.  
Repeat for Week 4 and Week 8.

- [ ] **Step 4: Commit**

```bash
git add public/slides/
git commit -m "feat: add pptx slide files as public static assets"
```

---

## Task 8: Responsive Layout & Production Build

**Files:**
- Modify: `src/style.css`

- [ ] **Step 1: Append responsive styles to end of `src/style.css`**

```css
@media (max-width: 640px) {
  .week-header {
    flex-direction: column;
    gap: 0.75rem;
  }

  .week-badge {
    min-width: unset;
    align-self: flex-start;
  }

  .slides-grid,
  .activities-grid {
    grid-template-columns: 1fr;
  }
}
```

- [ ] **Step 2: Verify responsive layout**

In browser devtools, set viewport to 375px wide. Expected: no horizontal scroll, all cards stack vertically, navbar wraps cleanly.

- [ ] **Step 3: Run production build**

```bash
npm run build
```

Expected: `dist/` folder created, zero build errors, no warnings about missing imports.

- [ ] **Step 4: Preview production build**

```bash
npm run preview
```

Open `http://localhost:4173`. Verify: all 8 weeks render, nav anchor links work, download buttons function.

- [ ] **Step 5: Commit**

```bash
git add src/style.css
git commit -m "feat: add responsive breakpoints; verify production build"
```
