<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Usuarios Activos por Rol</ion-card-title>
      <ion-card-subtitle>Profesores, Empresas y Admins</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="white-content">
      <div class="realtime-container">
        <div class="users-count">
          <div class="count">{{ currentUsers }}</div>
          <div class="label">usuarios activos</div>
        </div>
        <div class="role-indicators">
          <div class="role-indicator" v-for="role in userRoles" :key="role.name">
            <div class="role-dot" :style="{ backgroundColor: role.color }"></div>
            <span class="role-count">{{ role.count }}</span>
            <span class="role-name">{{ role.name }}</span>
          </div>
        </div>
        <canvas ref="chartCanvas"></canvas>
      </div>
    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent } from '@ionic/vue';
import Chart from 'chart.js/auto';

const chartCanvas = ref(null);
const currentUsers = ref(0);
const userRoles = ref([
  { name: 'Profesores', count: 12, color: '#ffcc00' },
  { name: 'Empresas', count: 8, color: '#10ac84' },
  { name: 'Admins', count: 3, color: '#ff6b6b' }
]);

let chart = null;
let updateInterval = null;
let resizeObserver = null;

const labels = Array(20).fill('').map((_, i) => `${i}`);
const data = Array(20).fill(0);

const updateRealtimeData = () => {
  const newUsers = Math.floor(Math.random() * 30) + 15;
  currentUsers.value = newUsers;
  
  data.shift();
  data.push(newUsers);
  
  userRoles.value.forEach(role => {
    role.count = Math.floor(Math.random() * (15 - 1 + 1)) + 1;
  });
  
  chart.update();
};

onMounted(() => {
  const ctx = chartCanvas.value.getContext('2d');
  
  chart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: labels,
      datasets: [{
        label: 'Usuarios Activos',
        data: data,
        borderColor: '#ffcc00',
        backgroundColor: 'rgba(255, 204, 0, 0.1)',
        borderWidth: 2,
        tension: 0.4,
        fill: true,
        pointRadius: 0
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          display: false
        }
      },
      scales: {
        y: {
          beginAtZero: false,
          min: 10,
          max: 50,
          display: false
        },
        x: {
          display: false
        }
      },
      animation: {
        duration: 300
      }
    }
  });
  
  updateRealtimeData();
  updateInterval = setInterval(updateRealtimeData, 2000);
  
  resizeObserver = new ResizeObserver(() => {
    if (chart) {
      chart.resize();
    }
  });
  
  if (chartCanvas.value.parentNode) {
    resizeObserver.observe(chartCanvas.value.parentNode);
  }
});

onUnmounted(() => {
  if (updateInterval) {
    clearInterval(updateInterval);
  }
  if (resizeObserver) {
    resizeObserver.disconnect();
  }
  if (chart) {
    chart.destroy();
  }
});
</script>

<style scoped>
.chart-card {
  height: 100%;
  border-top: 3px solid #ffcc00;
  background-color: #ffffff !important;
}

.white-card {
  --background: #ffffff !important;
  background: #ffffff !important;
}

.white-content {
  background-color: #ffffff !important;
  height: 300px;
}

.realtime-container {
  position: relative;
  height: 100%;
  width: 100%;
  background-color: #ffffff;
}

.users-count {
  position: absolute;
  top: 20px;
  left: 20px;
  z-index: 10;
  background-color: rgba(255, 255, 255, 0.9);
  padding: 10px 15px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.count {
  font-size: 2rem;
  font-weight: bold;
  color: #ffcc00;
}

.label {
  font-size: 0.8rem;
  color: #666;
}

.role-indicators {
  position: absolute;
  bottom: 20px;
  left: 20px;
  right: 20px;
  display: flex;
  justify-content: space-around;
  z-index: 10;
}

.role-indicator {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.9);
  padding: 8px;
  border-radius: 6px;
}

.role-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  margin-bottom: 4px;
}

.role-count {
  font-weight: bold;
  font-size: 0.9em;
}

.role-name {
  font-size: 0.7em;
  color: #666;
}
</style>
