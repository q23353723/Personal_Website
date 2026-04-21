# 有達的個人網頁 — Full-Stack × DL Vision

> Terminal/Monospace 終端機美學個人網頁，以 Vue 3 + Tailwind CSS 建置，部署於 GitHub Pages。

**Live：** https://q23353723.github.io/Personal_Website/

---

## 技術棧

| 層級 | 技術 |
|------|------|
| 框架 | Vue 3 (Composition API + `<script setup>`) |
| 樣式 | Tailwind CSS v4 (透過 `@tailwindcss/vite` 整合) |
| 建置工具 | Vite 8 |
| 部署 | GitHub Pages + GitHub Actions CI/CD |

---

## 專案結構

```
Personal_Website/
├── .github/
│   └── workflows/
│       └── deploy.yml        # CI/CD：推送 main 自動部署
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── TopBar.vue        # 頂部導覽列（時鐘、uptime、主題連結）
│   │   ├── HeroSection.vue   # 終端機 boot 序列 + ASCII art + 打字動畫
│   │   ├── AboutSection.vue  # 關於我
│   │   ├── SkillsSection.vue # 技術棧 5 欄格線
│   │   ├── WorkSection.vue   # 作品集（含即時跳動 preview）
│   │   ├── DashboardSection.vue # 戰情室 live dashboard
│   │   ├── ContactSection.vue   # 聯絡資訊 + footer
│   │   └── TweaksPanel.vue   # 右下角主題色切換面板
│   ├── App.vue               # 根元件，組合所有 section
│   ├── main.js               # Vue app 入口
│   └── style.css             # 全域 CSS 設計系統（CSS 變數 + Tailwind）
├── index.html
├── vite.config.js
└── package.json
```

---

## 設計特色

- **終端機美學** — 深石墨底色、掃描線疊加、雜訊紋理，完整 terminal 氛圍
- **Hero boot sequence** — 逐行打字的 boot log → ASCII name art 逐行顯示 → 循環打字指令
- **主題色切換** — Amber / Phosphor Green / Cyan 三色，右下角 Tweaks 面板即時套用
- **Live War-room Dashboard** — 每 700ms 更新的 UPH 折線圖、40 台機台狀態格、tail -f 事件日誌、CV bounding box 動畫
- **作品集 preview** — 電商追蹤器即時閃爍價格；維修戰情室 mini-preview 含 OEE / MTTR / Spark
- **Hover 互動** — proj-card 滑鼠跟隨 radial glow 效果
- **中英混排** — Space Grotesk 英文大標 + Noto Sans TC 中文內文 + JetBrains Mono 等寬基調

---

## 本地開發

**Prerequisites：** Node.js 24+、npm 11+

```bash
# 安裝依賴
npm install

# 啟動開發伺服器（預設 http://localhost:5173）
npm run dev

# 建置 production（輸出至 dist/）
npm run build

# 本地預覽 production build
npm run preview
```

---

## CI/CD 流程

```
git push origin main
        │
        ▼
GitHub Actions (.github/workflows/deploy.yml)
        │
        ├── Checkout → Setup Node 24 → npm install → npm run build
        │
        └── Upload dist/ → actions/deploy-pages
                │
                ▼
        https://q23353723.github.io/Personal_Website/
```

- 觸發條件：push 到 `main` 分支，或手動觸發（`workflow_dispatch`）
- 並發控制：同時只跑一個部署，新推送會取消舊的進行中部署
- 部署時間：約 30 秒

---

## 更新個人資訊

所有 placeholder 集中在以下位置：

| 欄位 | 檔案 | 位置 |
|------|------|------|
| Email | `src/components/ContactSection.vue` | `href="mailto:..."` |
| LinkedIn | `src/components/ContactSection.vue` | `/in/your-handle` |
| GitHub | `src/components/ContactSection.vue` | `@your-handle` |
| ASCII 名稱 | `src/components/HeroSection.vue` | `const ASCII = ...` |
| 技能條百分比 | `src/components/HeroSection.vue` | `const readout = [...]` |

---

## License

MIT
