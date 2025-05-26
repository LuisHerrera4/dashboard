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
  const kpiTarget = [77, 77, 77, 77, 77, 77]; // KPI: 95% de 81 prácticas = 77

  return { months, practicesAssigned, practicesCompleted, kpiTarget };
};

onMounted(() => {
  const { months, practicesAssigned, practicesCompleted, kpiTarget } = generatePracticesData();
  
  const ctx = chartCanvas.value.getContext('2d');
  chart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: months,
      datasets: [{
          label: 'Prácticas Asignadas',
          data: practicesAssigned,
          borderColor: '#2563eb',
          backgroundColor: 'rgba(37, 99, 235, 0.1)',
          borderWidth: 3,
          tension: 0.3,
          fill: false,
          pointBackgroundColor: '#2563eb',
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
        },
        {
          label: 'KPI Objetivo (95%)',
          data: kpiTarget,
          borderColor: '#ef4444',
          backgroundColor: 'transparent',
          borderWidth: 2,
          borderDash: [5, 5],
          tension: 0,
          fill: false,
          pointRadius: 0,
          pointHoverRadius: 0
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
      },
      scales: {
        y: {
          beginAtZero: true
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
  border-top: 3px solid #2563eb;
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
