<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Consumo de Recursos del Servidor</ion-card-title>
      <ion-card-subtitle>CPU, RAM y Almacenamiento</ion-card-subtitle>
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

// Datos simulados de consumo de recursos para diferentes servidores de EasyFCT
const resourceData = [
  {
    label: 'Servidor Web',
    cpu: 45,
    ram: 62,
    storage: 38
  },
  {
    label: 'Base de Datos',
    cpu: 68,
    ram: 78,
    storage: 55
  },
  {
    label: 'Servidor Archivos',
    cpu: 25,
    ram: 40,
    storage: 82
  },
  {
    label: 'API Gateway',
    cpu: 35,
    ram: 48,
    storage: 25
  },
  {
    label: 'Servidor Backup',
    cpu: 15,
    ram: 30,
    storage: 90
  }
];

// KPIs para recursos del servidor
const kpiLimits = {
  cpu: 80,      // 80% máximo CPU
  ram: 85,      // 85% máximo RAM  
  storage: 90   // 90% máximo almacenamiento
};

onMounted(() => {
  const ctx = chartCanvas.value.getContext('2d');
  
  chart = new Chart(ctx, {
    type: 'radar',
    data: {
      labels: ['CPU (%)', 'RAM (%)', 'Almacenamiento (%)'],
      datasets: [
        ...resourceData.map((server, index) => ({
          label: server.label,
          data: [server.cpu, server.ram, server.storage],
          backgroundColor: `rgba(37, 99, 235, ${0.7 - index * 0.1})`,
          borderColor: index === 0 ? '#2563eb' : `rgba(${51 + index * 40}, ${51 + index * 40}, ${51 + index * 40}, 0.8)`,
          borderWidth: 2,
          pointBackgroundColor: index === 0 ? '#2563eb' : `rgba(${51 + index * 40}, ${51 + index * 40}, ${51 + index * 40}, 0.8)`,
          pointBorderColor: '#fff',
          pointHoverBackgroundColor: '#fff',
          pointHoverBorderColor: index === 0 ? '#2563eb' : `rgba(${51 + index * 40}, ${51 + index * 40}, ${51 + index * 40}, 0.8)`,
          pointRadius: 4,
          pointHoverRadius: 6
        })),
        {
          label: 'KPI Límites',
          data: [kpiLimits.cpu, kpiLimits.ram, kpiLimits.storage],
          backgroundColor: 'rgba(239, 68, 68, 0.1)',
          borderColor: '#ef4444',
          borderWidth: 3,
          borderDash: [5, 5],
          pointBackgroundColor: '#ef4444',
          pointBorderColor: '#fff',
          pointRadius: 0,
          pointHoverRadius: 0,
          fill: false
        }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          position: 'top',
          labels: {
            boxWidth: 12,
            padding: 15,
            color: '#666'
          }
        },
        tooltip: {
          backgroundColor: 'rgba(255, 255, 255, 0.9)',
          titleColor: '#333',
          bodyColor: '#666',
          borderColor: '#2563eb',
          borderWidth: 1,
          padding: 10,
          displayColors: true,
          callbacks: {
            label: function(context) {
              const label = context.dataset.label || '';
              const value = context.raw;
              const metric = context.chart.data.labels[context.dataIndex];
              
              if (label === 'KPI Límites') {
                return `${label}: ${value}% límite de ${metric.split(' ')[0]}`;
              }
              
              return `${label}: ${value}% de ${metric.split(' ')[0]}`;
            }
          }
        }
      },
      scales: {
        r: {
          min: 0,
          max: 100,
          ticks: {
            stepSize: 20,
            backdropColor: 'transparent',
            color: '#666'
          },
          grid: {
            color: 'rgba(200, 200, 200, 0.3)'
          },
          angleLines: {
            color: 'rgba(200, 200, 200, 0.3)'
          },
          pointLabels: {
            color: '#333',
            font: {
              size: 12,
              weight: 'bold'
            }
          }
        }
      }
    }
  });
  
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
  display: flex;
  align-items: center;
  justify-content: center;
}

canvas {
  max-width: 100%;
  max-height: 100%;
}
</style>
