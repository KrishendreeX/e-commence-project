<template>
  <section class="stats-counter" aria-label="CleanSpaces impact statistics">
    <div v-for="stat in displayStats" :key="stat.label" class="stat-block">
      <p class="stat-number">{{ stat.display }}</p>
      <p class="stat-label">{{ stat.label }}</p>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Fallback values used until /api/stats responds, or if it's not up yet in dev.
const fallback = { activeZones: 12, householdsSubscribed: 640, cleanupsCompleted: 208 }

const raw = ref({ ...fallback })
const displayStats = ref([
  { key: 'activeZones', label: 'Active Zones', display: '0' },
  { key: 'householdsSubscribed', label: 'Households Subscribed', display: '0' },
  { key: 'cleanupsCompleted', label: 'Cleanups Completed', display: '0' },
])

let animationFrame = null

function animateCounts(target) {
  const duration = 1200
  const start = performance.now()
  const from = { activeZones: 0, householdsSubscribed: 0, cleanupsCompleted: 0 }

  function tick(now) {
    const progress = Math.min((now - start) / duration, 1)
    const eased = 1 - Math.pow(1 - progress, 3) // ease-out cubic

    displayStats.value = displayStats.value.map((stat) => {
      const value = Math.round(from[stat.key] + (target[stat.key] - from[stat.key]) * eased)
      return { ...stat, display: value.toLocaleString() }
    })

    if (progress < 1) {
      animationFrame = requestAnimationFrame(tick)
    }
  }
  animationFrame = requestAnimationFrame(tick)
}

async function loadStats() {
  try {
    const res = await fetch('/api/stats')
    if (!res.ok) throw new Error('bad response')
    const data = await res.json()
    raw.value = {
      activeZones: data.activeZones ?? fallback.activeZones,
      householdsSubscribed: data.householdsSubscribed ?? fallback.householdsSubscribed,
      cleanupsCompleted: data.cleanupsCompleted ?? fallback.cleanupsCompleted,
    }
  } catch (err) {
    // Backend not reachable yet during dev — fall back silently so the page still demos.
    raw.value = { ...fallback }
  }
  animateCounts(raw.value)
}

onMounted(loadStats)
onUnmounted(() => {
  if (animationFrame) cancelAnimationFrame(animationFrame)
})
</script>

<style scoped>
.stats-counter {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 2rem;
  max-width: 1000px;
  margin: 0 auto;
  padding: 2.5rem 1rem;
  text-align: center;
}
.stat-number { margin: 0; font-size: 2.75rem; font-weight: 800; color: #12332d; font-variant-numeric: tabular-nums; }
.stat-label { margin: .4rem 0 0; color: #6a7a76; font-size: .95rem; text-transform: uppercase; letter-spacing: .04em; }
@media (max-width: 600px) { .stat-number { font-size: 2.1rem; } }
</style>
