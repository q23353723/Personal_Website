<template>
  <section id="dashboard">
    <div class="wrap">
      <div class="sec-head">
        <div class="sec-tag">04 / LIVE</div>
        <h2>War-room, in miniature<br><small class="zh">// 取樣自戰情室 — 這是一份活的、正在跳動的假資料</small></h2>
      </div>

      <div class="sig">
        <div class="sig-header">
          <span>SITE_07 / FAB_B</span>
          <span>SHIFT — day</span>
          <span>TICK <span style="color:var(--ink)">{{ tick }}</span></span>
          <span class="sig-live"><span class="dot-live" style="margin-right:4px"></span>streaming</span>
        </div>

        <!-- Left: chart + KPIs -->
        <div class="sig-cell">
          <h4>OUTPUT / UPH — units per hour</h4>
          <div class="sig-big">{{ uph }}</div>
          <div class="sig-sub zh">
            過去 24 小時走勢，相較昨日
            <span :style="uphDelta.startsWith('+') ? 'color:var(--accent)' : 'color:var(--danger)'">{{ uphDelta }}</span>
          </div>
          <div class="sig-chart">
            <svg viewBox="0 0 400 100" preserveAspectRatio="none">
              <g class="grid">
                <line x1="0" y1="25" x2="400" y2="25"/>
                <line x1="0" y1="50" x2="400" y2="50"/>
                <line x1="0" y1="75" x2="400" y2="75"/>
              </g>
              <path :d="chartPath" fill="none" stroke="var(--accent)" stroke-width="1.5"/>
              <path :d="chartFill" fill="var(--accent-glow)" stroke="none"/>
            </svg>
          </div>
          <div class="kpi-grid">
            <div class="kpi-k">
              <div class="kpi-lbl">AVAIL</div>
              <div class="kpi-val">{{ avail }}%<span class="delta">+0.4</span></div>
            </div>
            <div class="kpi-k">
              <div class="kpi-lbl">PERF</div>
              <div class="kpi-val">{{ perf }}%<span class="delta">+1.1</span></div>
            </div>
            <div class="kpi-k">
              <div class="kpi-lbl">QUAL</div>
              <div class="kpi-val">{{ qual }}%<span class="delta down">−0.1</span></div>
            </div>
            <div class="kpi-k">
              <div class="kpi-lbl">OEE</div>
              <div class="kpi-val" style="color:var(--accent)">{{ oee }}%<span class="delta">+1.3</span></div>
            </div>
          </div>
        </div>

        <!-- Middle: machine grid + queue -->
        <div class="sig-cell">
          <h4>MACHINES / 40 units · line 07</h4>
          <div class="machines">
            <div
              v-for="(state, i) in machineStates" :key="i"
              class="m" :class="state"
            >{{ String(i + 1).padStart(2, '0') }}</div>
          </div>
          <div style="display:flex;gap:14px;margin-top:10px;font-size:10px;color:var(--ink-mute)">
            <span><span style="color:var(--accent)">■</span> running</span>
            <span><span style="color:var(--danger)">■</span> fault</span>
            <span><span style="color:var(--ink-mute)">■</span> idle</span>
          </div>
          <h4 style="margin-top:22px;">QUEUE / repair tickets</h4>
          <div style="font-family:'Space Grotesk';font-size:40px;color:var(--accent);line-height:1">{{ queueTotal }}</div>
          <div class="sig-sub zh">派工中 {{ queueDispatch }} · 待審 {{ queueWait }}</div>
        </div>

        <!-- Right: event log + CV detection -->
        <div class="sig-cell">
          <h4>EVENT STREAM / tail -f</h4>
          <div class="event-log">
            <div
              v-for="(ev, i) in eventLog" :key="i"
              class="log-line"
            >
              <span class="log-t">{{ ev.ts }}</span>
              <span :class="`log-${ev.cls}`">{{ ev.msg }}</span>
            </div>
          </div>

          <h4 style="margin-top:22px;">CV DETECTION / cam-07</h4>
          <div class="cam-big">
            <span style="position:absolute;bottom:8px;left:10px">frame <span>{{ frame }}</span></span>
            <span style="position:absolute;bottom:8px;right:10px;color:var(--accent)">{{ detectLabel }}</span>
            <svg
              :viewBox="`0 0 200 120`"
              preserveAspectRatio="none"
              style="position:absolute;inset:0;width:100%;height:100%"
              v-html="boxesSvg"
            ></svg>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const tick        = ref('000000')
const uph         = ref('1,284')
const uphDelta    = ref('+3.2%')
const avail       = ref('94.2')
const perf        = ref('91.1')
const qual        = ref('99.2')
const oee         = ref('85.1')
const queueTotal  = ref('14')
const queueDispatch = ref('6')
const queueWait   = ref('8')
const chartPath   = ref('')
const chartFill   = ref('')
const machineStates = ref([])
const eventLog    = ref([])
const frame       = ref(0)
const detectLabel = ref('det: 2')
const boxesSvg    = ref('')

const N = 80
let data = Array.from({ length: N }, (_, i) => 50 + Math.sin(i / 7) * 20 + Math.random() * 10)
let t = 0

function drawChart() {
  const w = 400, h = 100, max = 100, min = 20
  const pts = data.map((v, i) => {
    const x = (i / (N - 1)) * w
    const y = h - ((v - min) / (max - min)) * h
    return [x, Math.max(2, Math.min(h - 2, y))]
  })
  const d = 'M' + pts.map(p => p[0].toFixed(1) + ',' + p[1].toFixed(1)).join(' L')
  chartPath.value = d
  chartFill.value = d + ` L${w},${h} L0,${h} Z`
}

function initMachines() {
  machineStates.value = Array.from({ length: 40 }, () => {
    const r = Math.random()
    return r < 0.85 ? 'ok' : r < 0.92 ? 'idle' : 'err'
  })
}

const EVENTS = [
  ['ok',   'line_07.m14 · cycle complete ·   12.7s'],
  ['ok',   'api.dispatch → tech_B3 accepted ticket #4819'],
  ['dim',  'cv.detect camera=07 defects=2 conf=0.94'],
  ['ok',   'line_07.m22 · cycle complete ·   11.9s'],
  ['warn', 'line_07.m08 · fault ·  vibration threshold exceeded'],
  ['ok',   'kpi.oee recomputed → 85.3%'],
  ['dim',  'scheduler tick — next.sync in 45s'],
  ['ok',   'stream.cam_07 reconnected · latency 88ms'],
  ['ok',   'line_07.m31 · cycle complete ·   10.4s'],
  ['warn', 'queue.backlog > 12 · suggest re-dispatch'],
  ['dim',  'model/yolo_v8.cap inferred @ 62 fps'],
  ['ok',   'ticket #4820 closed · MTTR 9.8m'],
]

function addEvent() {
  const [cls, msg] = EVENTS[Math.floor(Math.random() * EVENTS.length)]
  const ts = new Date().toTimeString().slice(0, 8)
  eventLog.value.unshift({ cls, msg, ts })
  if (eventLog.value.length > 9) eventLog.value.pop()
}

function renderBoxes() {
  frame.value++
  const n = 1 + Math.floor(Math.random() * 3)
  detectLabel.value = 'det: ' + n
  let svg = ''
  for (let i = 0; i < n; i++) {
    const bw = 25 + Math.random() * 35
    const bh = 15 + Math.random() * 25
    const bx = Math.random() * (200 - bw)
    const by = Math.random() * (120 - bh)
    const conf = (0.8 + Math.random() * 0.19).toFixed(2)
    svg += `<rect x="${bx}" y="${by}" width="${bw}" height="${bh}" fill="none" stroke="var(--accent)" stroke-width="0.8"/>
            <text x="${bx + 1}" y="${by - 2}" fill="var(--accent)" font-size="6" font-family="JetBrains Mono">obj ${conf}</text>`
  }
  boxesSvg.value = svg
}

let timers = []

onMounted(() => {
  initMachines()
  drawChart()
  for (let i = 0; i < 6; i++) addEvent()
  renderBoxes()

  timers.push(setInterval(() => {
    t++
    data.shift()
    const last = data[data.length - 1]
    const next = Math.max(30, Math.min(95, last + (Math.random() - 0.5) * 8))
    data.push(next)
    drawChart()
    tick.value = String(t).padStart(6, '0')
    uph.value  = (1200 + Math.round(next * 2)).toLocaleString()
    const sign = next > 60
    uphDelta.value = (sign ? '+' : '') + ((next - 60) / 15).toFixed(1) + '%'
    const a = 92 + Math.random() * 4
    const p = 89 + Math.random() * 4
    const q = 98 + Math.random()
    avail.value = a.toFixed(1)
    perf.value  = p.toFixed(1)
    qual.value  = q.toFixed(1)
    oee.value   = (a * p * q / 10000).toFixed(1)
    const qv = 10 + Math.floor(Math.random() * 10)
    queueTotal.value    = String(qv)
    queueDispatch.value = String(Math.floor(qv * 0.4))
    queueWait.value     = String(qv - Math.floor(qv * 0.4))
  }, 700))

  timers.push(setInterval(() => {
    const idx = Math.floor(Math.random() * 40)
    const r = Math.random()
    machineStates.value[idx] = r < 0.8 ? 'ok' : r < 0.93 ? 'idle' : 'err'
  }, 900))

  timers.push(setInterval(addEvent, 900))
  timers.push(setInterval(renderBoxes, 1300))
})

onUnmounted(() => timers.forEach(clearInterval))
</script>
