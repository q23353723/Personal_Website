<template>
  <section id="skills">
    <div class="wrap">
      <div class="sec-head">
        <div class="sec-tag">02 / STACK</div>
        <h2>Technical surface<br><small class="zh">// 技術棧 — 從資料到像素，從後端到現場</small></h2>
      </div>

      <div class="skill-grid">
        <div class="skill-cell" v-for="(cat, idx) in skills" :key="idx">
          <span class="sc-num">.0{{ idx + 1 }}</span>
          <div class="sc-cat">{{ cat.name }}</div>
          <div class="sc-items">
            <div class="sc-item" v-for="item in cat.items" :key="item">{{ item }}</div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const skills = [
  { name: 'language',  items: ['Python', 'JavaScript ES6+', 'SQL', 'Bash'] },
  { name: 'frontend',  items: ['Vue.js 3', 'Tailwind CSS', 'ECharts', 'WebSocket UI'] },
  { name: 'backend',   items: ['FastAPI', 'RESTful API', 'Async / Queue', 'PostgreSQL'] },
  { name: 'ai · vision', items: ['Deep Learning', 'Image Processing', 'OpenCV', 'Model Pipeline'] },
  { name: 'devops',    items: ['Docker', 'Git / CI', 'Linux', 'Nginx'] },
]
</script>
