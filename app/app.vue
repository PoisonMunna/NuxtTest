<template>
  <div class="pro-container" :style="dynamicStyles">
    <!-- AMBIENT BACKGROUND -->
    <div class="mesh-gradient"></div>
    <div class="noise-overlay"></div>

    <div class="layout">
      <!-- SIDEBAR (Navigation Simulation) -->
      <aside class="sidebar">
        <div class="logo">
          <div class="logo-square">N</div>
          <span>NUXT_CORE</span>
        </div>
        <nav>
          <div class="nav-item active">Dashboard</div>
          <div class="nav-item">System</div>
          <div class="nav-item">Analytics</div>
          <div class="nav-item">Security</div>
        </nav>
        <div class="user-pill">
          <div class="avatar"></div>
          <span>Admin_01</span>
        </div>
      </aside>

      <!-- MAIN CONTENT -->
      <main class="main-content">
        <header class="top-bar">
          <div class="breadcrumb">SYSTEM / OVERVIEW / <span class="active">LIVE_FEED</span></div>
          <div class="system-time">{{ currentTime }}</div>
        </header>

        <div class="dashboard-grid">
          <!-- 1. NEURAL COUNTER -->
          <section class="module glass-card">
            <div class="module-header">
              <span class="icon">⚡</span>
              <h3>Neural Capacity</h3>
            </div>
            <div class="display-main">
              <div class="big-number">{{ count }}<span>%</span></div>
              <div class="controls">
                <button @click="count = Math.max(0, count - 5)" class="ctrl-btn">—</button>
                <button @click="count = Math.min(100, count + 5)" class="ctrl-btn">+</button>
              </div>
            </div>
            <div class="progress-track">
              <div class="progress-fill" :style="{ width: count + '%' }"></div>
            </div>
          </section>

          <!-- 2. ATMOSPHERE OVERRIDE -->
          <section class="module glass-card">
            <div class="module-header">
              <span class="icon">🎨</span>
              <h3>Chroma Link</h3>
            </div>
            <p class="desc">Global CSS Variable Sync</p>
            <div class="color-control">
              <div class="color-preview" :style="{ background: accentColor }"></div>
              <input type="color" v-model="accentColor" />
              <code class="hex-display">{{ accentColor.toUpperCase() }}</code>
            </div>
          </section>

          <!-- 3. SYSTEM LOGIC (Full Width) -->
          <section class="module glass-card full-row">
            <div class="module-header">
              <span class="icon">🧠</span>
              <h3>Compute Status</h3>
            </div>
            <div class="status-grid">
              <div class="status-box">
                <label>Current Mode</label>
                <div class="status-value" :class="statusColorClass">{{ systemStatus }}</div>
              </div>
              <div class="status-box">
                <label>Data Integrity</label>
                <div class="status-value text-green">SECURE</div>
              </div>
              <div class="status-box">
                <label>Node Uptime</label>
                <div class="status-value">99.9%</div>
              </div>
            </div>
          </section>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup>
// NUXT 3 AUTO-IMPORTS: ref, computed, onMounted are automatic.

const count = ref(45);
const accentColor = ref('#00dc82');
const currentTime = ref('');

// Logic for system status
const systemStatus = computed(() => {
  if (count.value > 85) return 'OVERDRIVE';
  if (count.value > 40) return 'NOMINAL';
  return 'LOW_POWER';
});

const statusColorClass = computed(() => {
  return {
    'text-red': systemStatus.value === 'OVERDRIVE',
    'text-green': systemStatus.value === 'NOMINAL',
    'text-blue': systemStatus.value === 'LOW_POWER'
  };
});

// Dynamic styles for the whole app
const dynamicStyles = computed(() => ({
  '--brand': accentColor.value,
  '--brand-glow': `${accentColor.value}44`
}));

// Clock update logic
onMounted(() => {
  setInterval(() => {
    currentTime.value = new Date().toLocaleTimeString('en-US', { hour12: false });
  }, 1000);
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;500;700&family=JetBrains+Mono&display=swap');

:root {
  --bg: #050608;
  --card-bg: rgba(15, 18, 25, 0.7);
  --border: rgba(255, 255, 255, 0.08);
  --text: #e2e8f0;
}

body {
  margin: 0;
  font-family: 'Space Grotesk', sans-serif;
  background: var(--bg);
  color: var(--text);
  overflow: hidden;
}

/* BACKGROUND EFFECTS */
.mesh-gradient {
  position: fixed;
  inset: 0;
  background: 
    radial-gradient(circle at 80% 20%, var(--brand-glow) 0%, transparent 40%),
    radial-gradient(circle at 20% 80%, rgba(66, 153, 225, 0.1) 0%, transparent 40%);
  z-index: -1;
}

.noise-overlay {
  position: fixed;
  inset: 0;
  opacity: 0.03;
  pointer-events: none;
  background-image: url('https://grainy-gradients.vercel.app/noise.svg');
  z-index: -1;
}

.layout {
  display: flex;
  height: 100vh;
}

/* SIDEBAR */
.sidebar {
  width: 240px;
  border-right: 1px solid var(--border);
  padding: 30px 20px;
  display: flex;
  flex-direction: column;
  background: rgba(0,0,0,0.3);
}

.logo {
  display: flex;
  align-items: center;
  gap: 12px;
  font-weight: 700;
  font-size: 0.9rem;
  letter-spacing: 2px;
  margin-bottom: 50px;
}

.logo-square {
  background: var(--brand);
  color: #000;
  width: 24px;
  height: 24px;
  display: grid;
  place-items: center;
  border-radius: 4px;
}

.nav-item {
  padding: 12px 15px;
  border-radius: 10px;
  color: #64748b;
  cursor: pointer;
  margin-bottom: 5px;
  transition: all 0.2s;
}

.nav-item:hover, .nav-item.active {
  background: rgba(255,255,255,0.05);
  color: white;
}

.user-pill {
  margin-top: auto;
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  background: rgba(255,255,255,0.03);
  border-radius: 15px;
}

.avatar {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: linear-gradient(45deg, var(--brand), #3b82f6);
}

/* MAIN AREA */
.main-content {
  flex: 1;
  padding: 30px 40px;
  overflow-y: auto;
}

.top-bar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 40px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.7rem;
  color: #64748b;
}

.top-bar .active { color: var(--brand); }

.dashboard-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 25px;
}

.full-row { grid-column: span 2; }

/* MODULE CARDS */
.module {
  background: var(--card-bg);
  border: 1px solid var(--border);
  border-radius: 24px;
  padding: 30px;
  backdrop-filter: blur(12px);
}

.module-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 25px;
}

.module-header h3 {
  margin: 0;
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: #94a3b8;
}

.display-main {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.big-number {
  font-size: 4rem;
  font-weight: 700;
  line-height: 1;
}

.big-number span {
  font-size: 1.5rem;
  color: var(--brand);
}

.controls { display: flex; gap: 10px; }

.ctrl-btn {
  width: 45px;
  height: 45px;
  border-radius: 12px;
  border: 1px solid var(--border);
  background: transparent;
  color: white;
  cursor: pointer;
  font-size: 1.2rem;
  transition: all 0.2s;
}

.ctrl-btn:hover { background: var(--brand); color: black; }

.progress-track {
  height: 6px;
  background: rgba(255,255,255,0.05);
  border-radius: 10px;
  margin-top: 25px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: var(--brand);
  box-shadow: 0 0 15px var(--brand);
  transition: width 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.color-control {
  display: flex;
  align-items: center;
  gap: 15px;
  background: rgba(0,0,0,0.2);
  padding: 15px;
  border-radius: 15px;
}

.color-preview {
  width: 40px;
  height: 40px;
  border-radius: 10px;
}

input[type="color"] {
  border: none;
  background: none;
  width: 30px;
  height: 30px;
  cursor: pointer;
}

.hex-display { font-family: 'JetBrains Mono', monospace; font-size: 0.9rem; }

.status-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.status-box label {
  display: block;
  font-size: 0.7rem;
  color: #64748b;
  margin-bottom: 5px;
}

.status-value {
  font-size: 1.2rem;
  font-weight: 700;
  letter-spacing: 1px;
}

.text-green { color: #00dc82; }
.text-red { color: #ff4757; }
.text-blue { color: #54a0ff; }
</style>