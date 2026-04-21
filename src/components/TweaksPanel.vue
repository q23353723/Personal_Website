<template>
  <div class="tweaks-panel">
    <div class="tweaks-title">
      <span><span class="tw-dot"></span>TWEAKS / theme</span>
      <span>◉</span>
    </div>
    <div class="tweaks-row">
      <button
        v-for="t in themes" :key="t.key"
        class="tweaks-btn" :class="[t.key, { active: current === t.key }]"
        @click="setTheme(t.key)"
      >
        <span class="tw-sw"></span>{{ t.label }}
      </button>
    </div>
    <div style="margin-top:10px;font-size:10px;color:var(--ink-mute);line-height:1.5">
      // terminal theme — 切換主題色，<br>整站即時套用
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const themes = [
  { key: 'amber', label: 'amber' },
  { key: 'phos',  label: 'phosphor' },
  { key: 'cyan',  label: 'cyan' },
]

const current = ref('cyan')

function setTheme(key) {
  current.value = key
  document.body.setAttribute('data-theme', key)
}

onMounted(() => {
  document.body.setAttribute('data-theme', current.value)
})
</script>
