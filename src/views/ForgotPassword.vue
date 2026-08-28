<template>
  <div class="auth-page">
    <div class="auth-card">
      <h1 class="auth-title">Reset Your Password</h1>
      <p class="auth-subtitle">Enter your email and we'll send you a link to reset your password.</p>

      <form v-if="!sent" class="auth-form" @submit.prevent="handleSubmit">
        <div class="input-group">
          <label for="reset-email">Email Address</label>
          <input id="reset-email" v-model="email" type="email" required />
        </div>

        <p v-if="errorMessage" class="error-text">{{ errorMessage }}</p>

        <button class="submit-btn" type="submit" :disabled="loading">
          {{ loading ? 'Sending...' : 'Send Reset Link' }}
        </button>
      </form>

      <div v-else class="confirmation">
        <p>If an account exists for <strong>{{ email }}</strong>, a reset link is on its way.</p>
      </div>

      <p class="footer-text">
        Remembered your password?
        <router-link to="/login">Log In</router-link>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const email = ref('')
const loading = ref(false)
const sent = ref(false)
const errorMessage = ref('')

async function handleSubmit() {
  errorMessage.value = ''
  loading.value = true
  try {
    const res = await fetch('/api/auth/forgot-password', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: email.value }),
    })
    if (!res.ok) {
      const data = await res.json().catch(() => ({}))
      throw new Error(data.message || 'Something went wrong. Please try again.')
    }
    sent.value = true
  } catch (err) {
    errorMessage.value = err.message
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.auth-page { min-height: 100vh; display: grid; place-items: center; padding: 2rem 1rem; background: #f4f6f5; }
.auth-card { width: min(100%, 440px); padding: 2.5rem; background: #12332d; color: #f4f6f5; border-radius: 16px; box-shadow: 0 15px 35px rgba(0,0,0,.35); }
.auth-title { margin: 0 0 .4rem; font-size: 1.7rem; }
.auth-subtitle { margin: 0 0 2rem; color: #a0b0ac; font-size: .9rem; }
.auth-form { display: grid; gap: 1.2rem; }
.input-group { display: grid; gap: .4rem; }
.input-group label { font-size: .88rem; font-weight: 600; }
.input-group input { padding: .8rem 1rem; color: #f4f6f5; background: #0b2a25; border: 1px solid rgba(255,255,255,.12); border-radius: 8px; font-size: 1rem; }
.input-group input:focus { outline: 2px solid #7cb342; outline-offset: 2px; }
.error-text { margin: 0; color: #ff8a80; font-size: .85rem; }
.submit-btn { padding: .9rem; color: #0b2a25; background: #7cb342; border: 0; border-radius: 8px; font-weight: 700; font-size: 1rem; cursor: pointer; }
.submit-btn:disabled { opacity: .7; cursor: wait; }
.confirmation { color: #d7e4de; line-height: 1.5; }
.footer-text { margin: 2rem 0 0; text-align: center; font-size: .9rem; color: #a0b0ac; }
.footer-text a { color: #7cb342; text-decoration: none; font-weight: 600; }
</style>