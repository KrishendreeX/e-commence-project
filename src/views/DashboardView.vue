<template>
  <div class="dashboard-container">
    <header class="dashboard-header">
      <div class="logo-brand">
        <img src="https://i.ibb.co/RpJFKCJX/cleanspaces-removebg-preview.png" alt="CleanSpaces Logo" class="small-logo" />
        <h1>CLEAN<span>SPACES</span></h1>
      </div>
      <button class="logout-btn" type="button" @click="emit('logout')">Log Out</button>
    </header>

    <main class="dashboard-content">
      <section class="welcome-banner">
        <h2>Welcome back, <span>Admin</span>!</h2>
        <p>Here is what's happening with your cleaning teams today.</p>
      </section>
      <section class="stats-grid" aria-label="Dashboard statistics">
        <div v-for="stat in stats" :key="stat.title" class="stat-card" :style="{ '--stat-color': stat.color }">
          <div class="stat-icon">{{ stat.icon }}</div>
          <div><p class="stat-value">{{ stat.value }}</p><p class="stat-title">{{ stat.title }}</p></div>
        </div>
      </section>
      <section class="activity-card">
        <div class="card-header"><h3>Recent Job Orders</h3><button class="text-btn" type="button">View All</button></div>
        <div class="table-responsive">
          <table><thead><tr><th>Client</th><th>Location</th><th>Status</th><th>Date</th></tr></thead>
            <tbody><tr v-for="job in recentJobs" :key="job.id"><td>{{ job.client }}</td><td>{{ job.location }}</td><td><span class="status-badge" :class="job.status.toLowerCase().replace(' ', '-')">{{ job.status }}</span></td><td>{{ job.date }}</td></tr></tbody>
          </table>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
const emit = defineEmits(['logout'])
const stats = [
  { title: 'Active Jobs', value: '24', icon: '🧹', color: '#7cb342' },
  { title: 'Cleaners On Duty', value: '15', icon: '👷', color: '#4caf50' },
  { title: 'Pending Requests', value: '8', icon: '📝', color: '#ff9800' },
  { title: 'Monthly Revenue', value: '$12.4k', icon: '💰', color: '#2196f3' },
]
const recentJobs = [
  { id: 1, client: 'Google HQ', location: 'Mountain View', status: 'Completed', date: 'Aug 25, 2026' },
  { id: 2, client: 'Starbucks', location: 'Downtown', status: 'In Progress', date: 'Aug 25, 2026' },
  { id: 3, client: 'City Hospital', location: 'Northside', status: 'Pending', date: 'Aug 26, 2026' },
  { id: 4, client: 'Amazon Warehouse', location: 'East End', status: 'Scheduled', date: 'Aug 27, 2026' },
]
</script>

<style scoped>
.dashboard-container { min-height: 100vh; padding: 2rem; color: #12332d; background: #f4f8f5; }
.dashboard-header { display: flex; align-items: center; justify-content: space-between; max-width: 1200px; margin: 0 auto 2rem; padding: 1rem 2rem; color: white; background: #12332d; border-radius: 12px; box-shadow: 0 10px 25px rgba(0,0,0,.15); }
.logo-brand { display: flex; align-items: center; gap: 15px; }.small-logo { width: 60px; height: 60px; }.logo-brand h1 { font-size: 1.5rem; }.logo-brand span, .welcome-banner span { color: #7cb342; }
.logout-btn, .text-btn { cursor: pointer; }.logout-btn { padding: .5rem 1.5rem; color: #7cb342; background: transparent; border: 2px solid #7cb342; border-radius: 8px; font-weight: 700; }.logout-btn:hover { color: #12332d; background: #7cb342; }
.dashboard-content { max-width: 1200px; margin: auto; }.welcome-banner { margin-bottom: 2rem; }.welcome-banner h2 { margin: 0 0 .5rem; font-size: 2.5rem; }.welcome-banner p { color: #6a7a76; font-size: 1.1rem; }
.stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 1.5rem; margin-bottom: 2rem; }.stat-card, .activity-card { background: white; border-radius: 12px; box-shadow: 0 5px 15px rgba(0,0,0,.05); }.stat-card { display: flex; align-items: center; gap: 1rem; padding: 1.5rem; border-left: 5px solid var(--stat-color); }.stat-icon { display: grid; place-items: center; width: 50px; height: 50px; background: color-mix(in srgb, var(--stat-color) 25%, white); border-radius: 12px; font-size: 1.5rem; }.stat-value { margin: 0; font-size: 1.8rem; font-weight: 800; }.stat-title { margin: .3rem 0 0; color: #6a7a76; font-size: .9rem; }.activity-card { padding: 1.5rem; }.card-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1.5rem; }.card-header h3 { margin: 0; }.text-btn { color: #7cb342; background: none; border: 0; font-weight: 600; }.table-responsive { overflow-x: auto; } table { width: 100%; border-collapse: collapse; text-align: left; } th { padding: 1rem; color: white; background: #12332d; font-size: .9rem; text-transform: uppercase; } td { padding: 1rem; border-bottom: 1px solid #f0f0f0; }.status-badge { padding: .3rem .8rem; border-radius: 20px; font-size: .8rem; font-weight: 700; }.completed { color: #155724; background: #d4edda; }.in-progress { color: #856404; background: #fff3cd; }.pending { color: #721c24; background: #f8d7da; }.scheduled { color: #0c5460; background: #d1ecf1; }
@media (max-width: 768px) { .dashboard-container { padding: 1rem; }.dashboard-header { flex-direction: column; gap: 1rem; }.welcome-banner h2 { font-size: 1.8rem; } }
</style>
