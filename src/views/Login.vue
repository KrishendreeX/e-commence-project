<template>
  <div class="login-container">
    <div class="bg-circle top-left"></div>
    <div class="bg-circle bottom-right"></div>

    <div class="login-card">
      <div class="logo-section">
        <img 
          src="https://i.ibb.co/RpJFKCJX/cleanspaces-removebg-preview.png" 
          alt="CleanSpaces Logo" 
          class="logo"
        />
        <h1 class="brand-title">CLEAN<span class="accent">SPACES</span></h1>
        <p class="tagline">Cleaner Spaces. Stronger Communities.</p>
      </div>

      <form @submit.prevent="handleLogin" class="login-form">
        <div class="input-group">
          <label for="email">Email Address</label>
          <input 
            type="email" 
            id="email" 
            v-model="form.email" 
            placeholder="you@cleanspaces.com"
            required
          />
        </div>

        <div class="input-group">
          <label for="password">Password</label>
          <input 
            type="password" 
            id="password" 
            v-model="form.password" 
            placeholder="Enter your password"
            required
          />
        </div>

        <div class="form-options">
          <label class="remember-me">
            <input type="checkbox" v-model="form.rememberMe" />
            <span>Remember me</span>
          </label>
          <a href="#" class="forgot-password">Forgot Password?</a>
        </div>

        <button type="submit" class="submit-btn" :disabled="isLoading">
          {{ isLoading ? 'Signing In...' : 'Sign In' }}
        </button>
      </form>

      <div class="footer-text">
        <p>Don't have an account? <a href="#" class="request-link">Request Access</a></p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue';

const form = reactive({
  email: '',
  password: '',
  rememberMe: false
});

const isLoading = ref(false);

const handleLogin = () => {
  isLoading.value = true;
  setTimeout(() => {
    isLoading.value = false;
  }, 1000);
};
</script>

<style scoped>
/* RESET */
* {
  box-sizing: border-box !important;
  margin: 0 !important;
  padding: 0 !important;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif !important;
}

.login-container {
  width: 100% !important;
  min-height: 100vh !important;
  background-color: #F4F6F5 !important;
  display: flex !important;
  justify-content: center !important;
  align-items: center !important;
  position: relative !important;
  overflow: hidden !important;
}

.bg-circle {
  position: absolute !important;
  border-radius: 50% !important;
  background-color: rgba(18, 51, 45, 0.04) !important;
}
.top-left {
  width: 500px !important;
  height: 500px !important;
  top: -150px !important;
  left: -150px !important;
}
.bottom-right {
  width: 400px !important;
  height: 400px !important;
  bottom: -100px !important;
  right: -100px !important;
}

.login-card {
  background-color: #12332D !important; 
  border: 1px solid #2A4A43 !important;
  padding: 3rem !important;
  border-radius: 20px !important;
  width: 100% !important;
  max-width: 450px !important;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4) !important;
  z-index: 10 !important;
}

.logo-section { text-align: center !important; margin-bottom: 2.5rem !important; }
.logo { width: 120px !important; height: 120px !important; object-fit: contain !important; margin-bottom: 1rem !important; }

.brand-title {
  color: #ffffff !important; 
  font-size: 2.4rem !important;
  font-weight: 800 !important;
  letter-spacing: -1px !important;
  margin-bottom: 0.5rem !important;
  text-transform: uppercase !important;
}
.brand-title .accent { color: #7CB342 !important; }

.tagline {
  color: #a0b0ac !important; 
  font-size: 0.9rem !important;
  font-weight: 500 !important;
  letter-spacing: 0.5px !important;
}

.input-group { margin-bottom: 1.5rem !important; }
.input-group label { display: block !important; color: #ffffff !important; font-size: 0.9rem !important; font-weight: 600 !important; margin-bottom: 0.5rem !important; }
.input-group input { 
  width: 100% !important; 
  padding: 0.9rem 1rem !important; 
  background-color: #0B2A25 !important; 
  border: 1px solid #2A4A43 !important; 
  border-radius: 8px !important; 
  color: #ffffff !important; 
  font-size: 1rem !important; 
  outline: none !important; 
}
.input-group input::placeholder { color: #6A7A76 !important; }
.input-group input:focus { border-color: #7CB342 !important; box-shadow: 0 0 0 3px rgba(124, 179, 66, 0.2) !important; }

.form-options { display: flex !important; justify-content: space-between !important; align-items: center !important; margin-bottom: 2rem !important; font-size: 0.9rem !important; }
.remember-me { display: flex !important; align-items: center !important; color: #a0b0ac !important; cursor: pointer !important; }
.remember-me input { margin-right: 0.5rem !important; accent-color: #7CB342 !important; cursor: pointer !important; }
.forgot-password, .request-link {
  color: #7CB342 !important; 
  text-decoration: none !important;
  font-weight: 500 !important;
  cursor: pointer !important;
}
.forgot-password:hover, .request-link:hover {
  color: #8bc34a !important;
  text-decoration: underline !important;
}

.submit-btn {
  width: 100% !important;
  padding: 1rem !important;
  background-color: #7CB342 !important; 
  color: #ffffff !important; 
  border: none !important;
  border-radius: 8px !important;
  font-size: 1.1rem !important;
  font-weight: 700 !important;
  cursor: pointer !important;
  transition: all 0.3s ease !important;
  letter-spacing: 0.5px !important;
  margin-bottom: 1.5rem !important;
}
.submit-btn:hover { background-color: #8bc34a !important; transform: translateY(-2px) !important; }

.footer-text { text-align: center !important; color: #a0b0ac !important; font-size: 0.9rem !important; }

@media (max-width: 500px) {
  .login-card { padding: 2rem !important; margin: 1rem !important; }
}
</style>