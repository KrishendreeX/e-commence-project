<template>
  <div class="auth-page">
    <div class="auth-card">
      <h1 class="auth-title">Create Your Account</h1>
      <p class="auth-subtitle">Sign up as a resident to join or start a CleanSpaces zone.</p>

      <form class="auth-form" @submit.prevent="handleSignup">
        <div class="input-group">
          <label for="signup-name">Full Name</label>
          <input id="signup-name" v-model="form.name" type="text" required />
        </div>
        <div class="input-group">
          <label for="signup-email">Email Address</label>
          <input id="signup-email" v-model="form.email" type="email" required />
        </div>
        <div class="input-group">
          <label for="signup-phone">Phone Number</label>
          <input id="signup-phone" v-model="form.phone" type="tel" required />
        </div>
        <div class="input-group">
          <label for="signup-password">Password</label>
          <input id="signup-password" v-model="form.password" type="password" required />
        </div>
        <div class="input-group">
          <label for="signup-confirm">Confirm Password</label>
          <input id="signup-confirm" v-model="form.confirmPassword" type="password" required />
        </div>

        <p v-if="errorMessage" class="error-text">{{ errorMessage }}</p>

        <button class="submit-btn" type="submit" :disabled="loading">
          {{ loading ? 'Creating Account...' : 'Sign Up' }}
        </button>
      </form>

      <p class="footer-text">
        Already have an account?
        <router-link to="/login">Log In</router-link>
      </p>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const form = reactive({ name: '', email: '', phone: '', password: '', confirmPassword: '' })
const loading = ref(false)
const errorMessage = ref('')

async function handleSignup() {
  errorMessage.value = ''

  if (form.password !== form.confirmPassword) {
    errorMessage.value = 'Passwords do not match.'
    return
  }

  loading.value = true
  try {
    const res = await fetch('/api/auth/signup', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        name: form.name,
        email: form.email,
        phone: form.phone,
        password: form.password,
      }),
    })
    const data = await res.json()
    if (!res.ok) throw new Error(data.message || 'Could not create account')

    localStorage.setItem('token', data.token)
    router.push('/resident')
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
.footer-text { margin: 2rem 0 0; text-align: center; font-size: .9rem; color: #a0b0ac; }
.footer-text a { color: #7cb342; text-decoration: none; font-weight: 600; }
</style>