<template>
  <section class="hero">
    <div class="hero-copy">
      <h1>Cleaner Streets Start <span>With Your Block</span></h1>
      <p>CleanSpaces organises weekly cleanups for Cape Flats communities — funded street by street, run by local crews.</p>
      <div class="hero-actions">
        <router-link to="/signup" class="btn-primary">Register Your Zone</router-link>
        <router-link to="/how-it-works" class="btn-secondary">See How It Works</router-link>
      </div>
    </div>

    <div class="slider" :style="{ '--pos': sliderPos + '%' }">
      <div class="slide-img after" :style="{ backgroundImage: `url(${slides[current].after})` }">
        <span class="tag tag-after">After</span>
      </div>
      <div class="slide-img before" :style="{ backgroundImage: `url(${slides[current].before})`, clipPath: `inset(0 ${100 - sliderPos}% 0 0)` }">
        <span class="tag tag-before">Before</span>
      </div>
      <input
        type="range"
        min="0"
        max="100"
        v-model.number="sliderPos"
        class="slider-handle"
        aria-label="Drag to compare before and after"
      />
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const slides = [
  {
    before: 'https://images.unsplash.com/photo-1580913428023-02c429014349?w=900&q=60',
    after: 'https://images.unsplash.com/photo-1610030181087-540a1c9f5750?w=900&q=60',
  },
]

const current = ref(0)
const sliderPos = ref(50)
</script>

<style scoped>
.hero { display: grid; grid-template-columns: 1fr 1fr; gap: 2.5rem; align-items: center; max-width: 1200px; margin: 0 auto; padding: 3rem 1.5rem; }
.hero-copy h1 { margin: 0 0 1rem; font-size: 2.6rem; line-height: 1.15; color: #12332d; }
.hero-copy h1 span { color: #7cb342; }
.hero-copy p { color: #4d5d59; font-size: 1.05rem; margin: 0 0 1.75rem; max-width: 40ch; }
.hero-actions { display: flex; gap: 1rem; flex-wrap: wrap; }
.btn-primary, .btn-secondary { display: inline-block; padding: .85rem 1.6rem; border-radius: 8px; font-weight: 700; text-decoration: none; }
.btn-primary { color: #0b2a25; background: #7cb342; }
.btn-secondary { color: #12332d; background: transparent; border: 2px solid #12332d; }

.slider { position: relative; width: 100%; aspect-ratio: 4 / 3; border-radius: 16px; overflow: hidden; box-shadow: 0 15px 35px rgba(0,0,0,.15); }
.slide-img { position: absolute; inset: 0; background-size: cover; background-position: center; }
.tag { position: absolute; top: 1rem; padding: .3rem .9rem; border-radius: 20px; font-size: .8rem; font-weight: 700; color: white; }
.tag-after { right: 1rem; background: #12332d; }
.tag-before { left: 1rem; background: #7cb342; color: #0b2a25; }
.slider-handle { position: absolute; inset: 0; width: 100%; height: 100%; margin: 0; opacity: 0; cursor: ew-resize; }

@media (max-width: 860px) {
  .hero { grid-template-columns: 1fr; padding: 2rem 1.25rem; }
  .hero-copy h1 { font-size: 2rem; }
}
</style>
