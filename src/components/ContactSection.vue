<template>
  <section id="contact">
    <div class="wrap">
      <div class="contact-wrap">
        <div class="contact-left">
          <h2>Let's <em>build</em><br>something.</h2>
          <p class="zh" style="margin-top:16px;color:var(--ink-dim);font-size:14px;max-width:36ch">
            對合作、技術討論、有趣的副專案都保持開放。若你的產線需要一雙看得懂程式與現場的眼睛 — 歡迎寄信。
          </p>
          <pre style="color:var(--accent-dim);font-size:11px;margin-top:26px;line-height:1.3">
  ┌──────────────────────────┐
  │  available for            │
  │   · freelance / contract  │
  │   · full-time (TW remote) │
  │   · technical consulting  │
  └──────────────────────────┘</pre>
        </div>
        <div class="contact-right">
          <ul>
            <li>
              <span class="ck">EMAIL</span>
              <a class="cv" href="mailto:you@example.com">a23353723@gmail.com</a>
              <span class="car">→</span>
            </li>
            <li>
              <span class="ck">LINKEDIN</span>
              <a class="cv" href="www.linkedin.com/in/有達-賴-02243b282">賴有達</a>
              <span class="car">→</span>
            </li>
            <li>
              <span class="ck">GITHUB</span>
              <a class="cv" href="https://github.com/q23353723">@q23353723</a>
              <span class="car">→</span>
            </li>
            <li>
              <span class="ck">LOCATION</span>
              <span class="cv zh">台灣，台中 · UTC+8</span>
              <span class="car">◉</span>
            </li>
            <li>
              <span class="ck">RESPONSE</span>
              <span class="cv">&lt; 24h · weekdays</span>
              <span class="car">◉</span>
            </li>
          </ul>
        </div>
      </div>

      <footer>
        <div>© 2026 · engineered in Taichung · hand-typed, not vibe-coded.</div>
        <div class="fr">
          <span>v4.2.1</span>
          <span>last_commit: 2026-04-21</span>
          <span>uptime: {{ uptime }}</span>
        </div>
      </footer>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const uptime = ref('00:00:00')
const start  = Date.now()
let timer

function two(n) { return n < 10 ? '0' + n : String(n) }
function tick() {
  const s = Math.floor((Date.now() - start) / 1000)
  uptime.value = `${two(Math.floor(s / 3600))}:${two(Math.floor(s / 60) % 60)}:${two(s % 60)}`
}

onMounted(() => { tick(); timer = setInterval(tick, 1000) })
onUnmounted(() => clearInterval(timer))
</script>
