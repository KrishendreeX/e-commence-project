<template>
  <div class="login-container">
    <div class="bg-circle top-left"></div>
    <div class="bg-circle bottom-right"></div>
    <div class="login-card">
      <div class="logo-section">
        <img src="https://i.ibb.co/RpJFKCJX/cleanspaces-removebg-preview.png" alt="CleanSpaces Logo" class="logo" />
        <h1 class="brand-title">CLEAN<span>SPACES</span></h1>
        <p class="tagline">Cleaner Spaces. Stronger Communities.</p>
      </div>

      <form class="login-form" @submit.prevent="handleLogin">
        <div class="input-group">
          <label for="email">Email Address</label>
          <input id="email" v-model="form.email" type="email" placeholder="you@cleanspaces.com" required />
        </div>
        <div class="input-group">
          <label for="password">Password</label>
          <input id="password" v-model="form.password" type="password" placeholder="Enter your password" required />
        </div>
        <div class="form-options">
          <label><input v-model="form.rememberMe" type="checkbox" /> Remember me</label>
          <a href="#" @click.prevent>Forgot Password?</a>
        </div>
        <button class="submit-btn" type="submit" :disabled="isLoading">
          {{ isLoading ? 'Signing In...' : 'Sign In' }}
        </button>
      </form>
      <p class="footer-text">Don't have an account? <a href="#" @click.prevent>Request Access</a></p>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'

const emit = defineEmits(['login'])
const form = reactive({ email: '', password: '', rememberMe: false })
const isLoading = ref(false)

function handleLogin() {
  isLoading.value = true
  window.setTimeout(() => {
    isLoading.value = false
    emit('login')
  }, 500)
}
</script>

<style scoped>
* { box-sizing: border-box; }
.login-container { position: relative; min-height: 100vh; display: grid; place-items: center; padding: 1rem; overflow: hidden; background: #f4f6f5; }
.bg-circle { position: absolute; border-radius: 50%; background: rgba(18, 51, 45, .04); pointer-events: none; }
.top-left { width: 500px; height: 500px; top: -150px; left: -150px; }
.bottom-right { width: 400px; height: 400px; right: -100px; bottom: -100px; }
.login-card { width: min(100%, 450px); padding: 3rem; color: #f4f6f5; background: #12332d; border: 1px solid rgba(255,255,255,.12); border-radius: 20px; box-shadow: 0 15px 35px rgba(0,0,0,.35); }
.logo-section { margin-bottom: 2.5rem; text-align: center; }
.logo { width: 100px; height: 100px; object-fit: contain; margin-bottom: 1rem; padding: 5px; background: white; border-radius: 50%; }
.brand-title { margin: 0 0 .5rem; font-size: 2.5rem; font-weight: 800; }
.brand-title span, .form-options a, .footer-text a { color: #7cb342; }
.tagline, .footer-text, .form-options { color: #a0b0ac; }
.login-form { display: grid; gap: 1.5rem; }
.input-group { display: grid; gap: .5rem; }
.input-group label { font-size: .9rem; font-weight: 600; }
.input-group input { width: 100%; padding: .9rem 1rem; color: #f4f6f5; background: #0b2a25; border: 1px solid rgba(255,255,255,.12); border-radius: 8px; font-size: 1rem; }
.input-group input:focus { outline: 2px solid #7cb342; outline-offset: 2px; }
.form-options { display: flex; justify-content: space-between; align-items: center; gap: 1rem; font-size: .9rem; }
.form-options label { display: flex; align-items: center; gap: .5rem; }
a { text-decoration: none; }
.submit-btn { padding: 1rem; color: #0b2a25; background: #7cb342; border: 0; border-radius: 8px; font-size: 1.1rem; font-weight: 700; cursor: pointer; }
.submit-btn:disabled { opacity: .7; cursor: wait; }
.footer-text { margin: 2rem 0 0; text-align: center; font-size: .9rem; }
@media (max-width: 500px) { .login-card { padding: 2rem 1.5rem; } .brand-title { font-size: 2rem; } .form-options { align-items: flex-start; flex-direction: column; } }
</style>
