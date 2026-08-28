<template>
  <div class="home">
    <HeroSlider />
    <StatsCounter />

    <section class="section-block">
      <div class="section-heading">
        <h2>Where We're Working</h2>
        <p>Active and pending zones across the Cape Flats.</p>
      </div>
      <ZoneMap />
    </section>

    <section class="section-block testimonials" v-if="testimonials.length">
      <div class="section-heading">
        <h2>What Residents Say</h2>
      </div>
      <div class="testimonial-carousel">
        <div v-for="t in testimonials" :key="t.id" class="testimonial-card">
          <p class="quote">&ldquo;{{ t.quote }}&rdquo;</p>
          <p class="author">— {{ t.name }}</p>
          <p class="rating" aria-hidden="true">{{ '★'.repeat(t.rating) }}{{ '☆'.repeat(5 - t.rating) }}</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import HeroSlider from '../components/HeroSlider.vue'
import StatsCounter from '../components/StatsCounter.vue'
import ZoneMap from '../components/ZoneMap.vue'

const testimonials = ref([])

async function loadTestimonials() {
  try {
    const res = await fetch('/api/testimonials')
    if (!res.ok) throw new Error('bad response')
    testimonials.value = await res.json()
  } catch (err) {
    testimonials.value = []
  }
}

onMounted(loadTestimonials)
</script>

<style scoped>
.home { background: #f4f8f5; }
.section-block { max-width: 1100px; margin: 0 auto; padding: 3rem 1.5rem; }
.section-heading { text-align: center; margin-bottom: 2rem; }
.section-heading h2 { margin: 0 0 .4rem; font-size: 2rem; color: #12332d; }
.section-heading p { color: #6a7a76; margin: 0; }

.testimonial-carousel { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 1.5rem; }
.testimonial-card { background: white; border-radius: 12px; padding: 1.75rem; box-shadow: 0 5px 15px rgba(0,0,0,.05); }
.quote { font-style: italic; color: #2e3d39; margin: 0 0 1rem; }
.author { font-weight: 700; margin: 0; color: #12332d; }
.rating { color: #ff9800; margin: .3rem 0 0; }
</style>