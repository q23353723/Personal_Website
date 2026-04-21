<template>
  <div class="topbar">
    <div>
      <span class="dot-live"></span>SESSION
      <span style="color:var(--ink)"> eng.portfolio</span>
      · BUILD 2026.04 · UPTIME
      <span style="color:var(--ink)">{{ uptime }}</span>
    </div>
    <nav class="nav">
      <a href="#about">about</a>
      <a href="#skills">stack</a>
      <a href="#work">work</a>
      <a href="#dashboard">live</a>
      <a href="#contact">contact</a>
    </nav>
    <div class="clock">{{ clock }}</div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const clock  = ref('—')
const uptime = ref('00:00:00')
const start  = Date.now()
let timer

function two(n) { return n < 10 ? '0' + n : String(n) }
function tick() {
  const d = new Date()
  clock.value = d.toISOString().replace('T', ' ').slice(0, 19) + 'Z'
  const s = Math.floor((Date.now() - start) / 1000)
  uptime.value = `${two(Math.floor(s / 3600))}:${two(Math.floor(s / 60) % 60)}:${two(s % 60)}`
}

onMounted(() => { tick(); timer = setInterval(tick, 1000) })
onUnmounted(() => clearInterval(timer))
</script>
