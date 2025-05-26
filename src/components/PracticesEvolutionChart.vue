<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Evolución de Prácticas Asignadas</ion-card-title>
      <ion-card-subtitle>Últimos 6 meses</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="white-content">
      <canvas ref="chartCanvas"></canvas>
    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent } from '@ionic/vue';
import Chart from 'chart.js/auto';

const chartCanvas = ref(null);
let chart = null;
let resizeObserver = null;

const generatePracticesData = () => {
  const months = ['Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo', 'Junio'];
  const practicesAssigned = [45, 52, 38, 67, 73, 81];
  const practicesCompleted = [42, 48, 35, 61, 68, 75];

  return { months, practicesAssigned, practicesCompleted };
};

onMounted(() => {
  const { months, practicesAssigned, practicesCompleted } = generatePracticesData();
  
  const ctx = chartCanvas.value.getContext('2d');
  chart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: months,
      datasets: [{
          label: 'Prácticas Asignadas',
          data: practicesAssigned,
          borderColor: '#ffcc00',
          backgroundColor: 'rgba(255, 204, 0, 0.1)',
          borderWidth: 3,
          tension: 0.3,
          fill: true,
          pointBackgroundColor: '#ffcc00',
          pointRadius: 5
        },
        {
          label: 'Prácticas Completadas',
          data: practicesCompleted,
          borderColor: '#10ac84',
          backgroundColor: 'rgba(16, 172, 132, 0.1)',
          borderWidth: 2,
          tension: 0.3,
          fill: false,
          pointBackgroundColor: '#10ac84',
          pointRadius: 4
        }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          position: 'top',
        }
      }
    },
  });
  
  resizeObserver = new ResizeObserver(() => {
    if (chart) {
      chart.resize();
    }
  });
  
  resizeObserver.observe(chartCanvas.value.parentNode);
});

onUnmounted(() => {
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

ion-card-content {
  background-color: #ffffff !important;
}
</style>
