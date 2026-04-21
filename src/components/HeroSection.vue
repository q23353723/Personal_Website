<template>
  <header class="hero">
    <!-- LEFT: terminal -->
    <div class="term">
      <div class="term-chrome">
        <div class="lights"><i></i><i></i><i></i></div>
        <span>— zsh — 120×40 — identity.sh</span>
        <span class="path">~/engineer/identity</span>
      </div>

      <div class="boot" v-html="bootHtml"></div>
      <pre class="ascii-art">{{ asciiVisible }}</pre>
      <div v-if="showPrompt">
        <div class="prompt">
          <span class="prompt-user">engineer</span
          ><span class="prompt-host">@auo</span>:<span class="prompt-path">~</span>$ cat ./hello.md
        </div>
        <div style="color:var(--ink); margin:6px 0 10px; white-space:pre-wrap">
# hello,
  <span class="zh" style="color:var(--ink-dim)">你好，我是一位位在台灣台中的</span>
  <span class="zh" style="color:var(--ink)"><b>全端 × 深度學習影像</b> 工程師</span>。
  <span class="zh" style="color:var(--ink-mute)">下方 ↓ 是我的履歷與作品。</span>
        </div>
        <div class="prompt">
          <span class="prompt-user">engineer</span
          ><span class="prompt-host">@auo</span>:<span class="prompt-path">~</span>$
          <span>{{ currentCmd }}</span><span class="caret-block"></span>
        </div>
      </div>
    </div>

    <!-- RIGHT: profile -->
    <div class="hero-right">
      <div>
        <div style="color:var(--ink-mute);font-size:11px;letter-spacing:.1em;margin-bottom:14px;">
          <span style="color:var(--accent)">&gt;</span> PROFILE / v4.2 — engineer bio
        </div>
        <h1 class="big-title">
          Full-Stack<br>
          × <em>DL Vision</em><br>
          <span class="zh-sub zh">高級工程師 — 將數據轉為決策</span>
        </h1>
        <p class="hero-sub zh">
          專注於 <b style="color:var(--accent)">全端開發</b> 與
          <b style="color:var(--accent)">深度學習影像應用</b>。
          結合技術精準度與使用者體驗，致力於將複雜的數據轉化為直觀的數位決策工具。
        </p>
        <div style="display:flex;flex-wrap:wrap;gap:6px;margin-top:16px;">
          <span class="lbl"><b>FastAPI</b></span>
          <span class="lbl"><b>Vue.js</b></span>
          <span class="lbl"><b>Python</b></span>
          <span class="lbl"><b>Deep Learning</b></span>
          <span class="lbl"><b>Docker</b></span>
          <span class="lbl"><b>Linux</b></span>
        </div>
      </div>

      <div>
        <div class="meta-grid">
          <div><div class="meta-k">ROLE</div><div class="meta-v">Senior Engineer</div></div>
          <div><div class="meta-k">COMPANY</div><div class="meta-v accent">AUO / 友達光電</div></div>
          <div><div class="meta-k">LOCATION</div><div class="meta-v">Taichung, TW</div></div>
          <div>
            <div class="meta-k">STATUS</div>
            <div class="meta-v"><span class="dot-live" style="width:7px;height:7px;margin-right:6px;vertical-align:-1px"></span>open_to_chat</div>
          </div>
        </div>

        <div class="readout">
          <div class="readout-row" v-for="r in readout" :key="r.k">
            <span class="rk">{{ r.k }}</span>
            <span class="bar"><i :style="`transform:scaleX(${r.v / 100})`"></i></span>
            <span class="rv">{{ r.v }}%</span>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const ASCII = String.raw`
 ██████╗ ███╗   ██╗    ██████╗ ██╗   ██╗████████╗██╗   ██╗
██╔═══██╗████╗  ██║    ██╔══██╗██║   ██║╚══██╔══╝╚██╗ ██╔╝
██║   ██║██╔██╗ ██║    ██║  ██║██║   ██║   ██║    ╚████╔╝
██║   ██║██║╚██╗██║    ██║  ██║██║   ██║   ██║     ╚██╔╝
╚██████╔╝██║ ╚████║    ██████╔╝╚██████╔╝   ██║      ██║
 ╚═════╝ ╚═╝  ╚═══╝    ╚═════╝  ╚═════╝    ╚═╝      ╚═╝   . eng`

const BOOT_LINES = [
  ['boot', 'init identity.sh @ node-07'],
  ['ok',   'mounting /home/engineer ........................... [ OK ]'],
  ['ok',   'loading credentials ............................... [ OK ]'],
  ['ok',   'fetching recent commits ........................... [ OK ]'],
  ['dim',  'scanning skills ··· python · vue · fastapi · cv · docker'],
  ['ok',   'handshake with AUO / line-07 ...................... [ OK ]'],
  ['warn', 'coffee buffer low ................................. [ 14% ]'],
  ['ok',   'warming up .......................................  ready'],
  ['dim',  '── authenticated as <span style="color:var(--accent)">engineer</span> ─────────────────────────────'],
]

const bootHtml     = ref('')
const asciiVisible = ref('')
const showPrompt   = ref(false)
const currentCmd   = ref('')

const readout = [
  { k: 'focus',    v: 92 },
  { k: 'backend',  v: 88 },
  { k: 'frontend', v: 82 },
  { k: 'cv / dl',  v: 78 },
  { k: 'caffeine', v: 96 },
]

let timers = []
function later(fn, ms) { const t = setTimeout(fn, ms); timers.push(t); return t }

function pad(s, n) { s = String(s); while (s.length < n) s = '0' + s; return s }

function startBoot() {
  let i = 0
  let delay = 300
  const step = () => {
    if (i >= BOOT_LINES.length) { later(showAscii, 200); return }
    const [cls, txt] = BOOT_LINES[i++]
    const ts = pad((performance.now() / 1000).toFixed(3), 7)
    bootHtml.value += `<div><span style="color:var(--ink-mute)">[ ${ts} ]</span> <span class="${cls}">${txt}</span></div>`
    later(step, 120 + Math.random() * 180)
  }
  later(step, delay)
}

function showAscii() {
  const lines = ASCII.split('\n')
  let k = 0
  const step = () => {
    if (k > lines.length) { later(revealPrompt, 200); return }
    asciiVisible.value = lines.slice(0, k).join('\n')
    k++
    later(step, 45)
  }
  step()
}

function revealPrompt() {
  showPrompt.value = true
  typeCmd()
}

function typeCmd() {
  const cmds = ['./scroll_down.sh', 'open portfolio/', 'npm run hire']
  let ci = 0
  function loop() {
    const c = cmds[ci % cmds.length]
    let k = 0
    currentCmd.value = ''
    const typing = () => {
      if (k > c.length) {
        later(() => erase(), 1600)
        return
      }
      currentCmd.value = c.slice(0, k++)
      later(typing, 55)
    }
    const erase = () => {
      if (currentCmd.value.length === 0) { ci++; later(loop, 350); return }
      currentCmd.value = currentCmd.value.slice(0, -1)
      later(erase, 28)
    }
    typing()
  }
  loop()
}

onMounted(startBoot)
onUnmounted(() => timers.forEach(clearTimeout))
</script>
