<template>
  <section id="work">
    <div class="wrap">
      <div class="sec-head">
        <div class="sec-tag">03 / WORK</div>
        <h2>Selected projects<br><small class="zh">// 精選作品集 — 兩個從頭打造的系統</small></h2>
      </div>

      <!-- Project A -->
      <div class="proj">
        <div>
          <div class="proj-n">A.001</div>
          <div style="color:var(--ink-mute);font-size:11px;margin-top:6px;">since 2023<br>side-project</div>
        </div>
        <div class="proj-body">
          <div class="proj-card" @mousemove="trackMouse($event, 'pt')" ref="ptCard">
            <div class="proj-sub">E-COMMERCE · PRICE TRACKING PLATFORM</div>
            <h3>電商商品<br>價格追蹤平台</h3>
            <p class="zh">為解決消費者難以即時掌握電商波段折扣的問題，從零開發的一套全自動價格追蹤系統。從爬蟲、排程、通知到使用者訂閱介面，皆由一人完成。</p>
            <ul class="feat">
              <li><span class="feat-l">SUBSCRIBE</span><span class="zh">使用者可自由新增電商商品連結，設定目標購買金額</span></li>
              <li><span class="feat-l">NOTIFY</span><span class="zh">整合 Line Notify 與 E-mail API，價格達標即刻推播</span></li>
              <li><span class="feat-l">MONITOR</span><span class="zh">持續爬取並分析價格走勢，不漏掉任何優惠</span></li>
            </ul>
            <div class="tags">
              <span class="chip">FastAPI</span>
              <span class="chip">Python Scraper</span>
              <span class="chip">Scheduler</span>
              <span class="chip">Line API</span>
              <span class="chip">SMTP</span>
            </div>
          </div>

          <!-- Price Tracker preview -->
          <div class="preview">
            <div class="prev-head">
              <span>WATCHLIST</span>
              <span class="prev-live">LIVE</span>
            </div>
            <div>
              <div
                v-for="(item, i) in ptItems" :key="i"
                class="pt-item"
                :class="{ hit: item.hit }"
                :ref="el => ptRows[i] = el"
              >
                <div class="pt-name">{{ item.name }}<small>{{ item.shop }}</small></div>
                <div class="pt-price">
                  <del>NT${{ item.was.toLocaleString() }}</del>NT${{ item.cur.toLocaleString() }}
                </div>
              </div>
            </div>
            <div class="pt-notif">
              <div class="pt-t">▲ NOTIFY SENT</div>
              <div style="margin-top:4px;">
                Shokz OpenRun Pro <code>@ NT$3,690</code> — 達標 📣 line_notify →
                <b style="color:var(--accent)">OK</b>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Project B -->
      <div class="proj">
        <div>
          <div class="proj-n">B.002</div>
          <div style="color:var(--ink-mute);font-size:11px;margin-top:6px;">AUO · 2024—now<br>production</div>
        </div>
        <div class="proj-body">
          <div class="proj-card" @mousemove="trackMouse($event, 'rd')" ref="rdCard">
            <div class="proj-sub">FACTORY · REPAIR DASHBOARD</div>
            <h3>工廠維修<br>數位戰情室</h3>
            <p class="zh">針對大規模生產線開發的整合式管理平台，將硬體操作與數據監控全面數位化。管理員無需親臨現場，即可視察、派工、模擬產能。</p>
            <ul class="feat">
              <li><span class="feat-l">MONITOR</span><span class="zh">視覺化呈現工廠內各機台的即時運作狀態</span></li>
              <li><span class="feat-l">KPI</span><span class="zh">自動計算維修關鍵指標，以動態圖表分析產能趨勢</span></li>
              <li><span class="feat-l">STREAM</span><span class="zh">遠端串接機台實際影像，遠程視察維修點</span></li>
              <li><span class="feat-l">CONTROL</span><span class="zh">直接透過網頁介面操作廠內派貨系統，軟硬體整合</span></li>
              <li><span class="feat-l">SIMULATE</span><span class="zh">編輯維修履歷與機台配置，即時預估並優化產能</span></li>
            </ul>
            <div class="tags">
              <span class="chip">Vue3</span>
              <span class="chip">ECharts</span>
              <span class="chip">Video Stream</span>
              <span class="chip">FastAPI</span>
              <span class="chip">Hardware Integration</span>
              <span class="chip">DL Vision</span>
            </div>
          </div>

          <!-- Repair Dashboard mini-preview -->
          <div class="preview">
            <div class="prev-head">
              <span>REPAIR.DASH</span>
              <span>LINE_07</span>
              <span class="prev-live">LIVE</span>
            </div>
            <div class="db">
              <div class="db-cell">
                <div class="db-t">OEE</div>
                <div class="db-big">{{ oee }}</div>
              </div>
              <div class="db-cell">
                <div class="db-t">MTTR · min</div>
                <div class="db-big">{{ mttr }}</div>
              </div>
              <div class="db-cell span2">
                <div class="db-t">THROUGHPUT · last 24h</div>
                <div class="spark">
                  <b v-for="(h, i) in sparkBars" :key="i" :style="`height:${h}%`"></b>
                </div>
              </div>
              <div class="db-cell cam-box">
                <span style="position:absolute;bottom:8px;left:10px;">CAM-07</span>
                <span style="position:absolute;bottom:8px;right:10px;color:var(--accent)">{{ camDetect }}</span>
              </div>
              <div class="db-cell">
                <div class="db-t">QUEUE</div>
                <div class="db-big" style="font-size:28px;">{{ queue }}</div>
                <div class="db-t" style="margin-top:4px;">pending tickets</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

function trackMouse(e, card) {
  const el = card === 'pt' ? ptCard.value : rdCard.value
  if (!el) return
  const r = el.getBoundingClientRect()
  el.style.setProperty('--mx', ((e.clientX - r.left) / r.width * 100) + '%')
  el.style.setProperty('--my', ((e.clientY - r.top) / r.height * 100) + '%')
}

const ptCard = ref(null)
const rdCard = ref(null)
const ptRows = ref([])

const ptItems = ref([
  { name: 'Sony WH-1000XM5',     was: 11900, target: 9800, cur: 10490, shop: 'momo',   hit: false },
  { name: 'Logitech MX Master 3S',was: 3790,  target: 3200, cur: 3290,  shop: 'pchome', hit: false },
  { name: 'Kindle Paperwhite',   was: 4480,  target: 3900, cur: 4190,  shop: 'shopee', hit: false },
  { name: 'Shokz OpenRun Pro',   was: 4490,  target: 3800, cur: 3690,  shop: 'momo',   hit: true  },
  { name: 'Anker Nano Charger',  was: 990,   target: 700,  cur: 790,   shop: 'pchome', hit: false },
  { name: 'Uniqlo Airism Polo',  was: 790,   target: 590,  cur: 690,   shop: 'uniqlo', hit: false },
])

const oee       = ref('87.4%')
const mttr      = ref('12.8')
const queue     = ref('14')
const camDetect = ref('detect: 3')
const sparkBars = ref(Array.from({ length: 28 }, () => 30 + Math.random() * 70))

let timers = []

onMounted(() => {
  timers.push(setInterval(() => {
    const idx = Math.floor(Math.random() * ptItems.value.length)
    const item = ptItems.value[idx]
    if (item.hit) return
    const drift = Math.round((Math.random() - 0.5) * 40)
    item.cur = Math.max(item.target - 50, item.cur + drift)
    const row = ptRows.value[idx]
    if (row) row.animate([{ background: 'var(--accent-glow)' }, { background: 'transparent' }], { duration: 600 })
  }, 1400))

  timers.push(setInterval(() => {
    sparkBars.value.shift()
    sparkBars.value.push(30 + Math.random() * 70)
    oee.value      = (85 + Math.random() * 4).toFixed(1) + '%'
    mttr.value     = (10 + Math.random() * 5).toFixed(1)
    queue.value    = String(10 + Math.floor(Math.random() * 8))
    camDetect.value = 'detect: ' + Math.floor(Math.random() * 5)
  }, 1200))
})

onUnmounted(() => timers.forEach(clearInterval))
</script>
