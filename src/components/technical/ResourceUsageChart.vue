<template>
  <ion-card class="chart-card">
    <ion-card-header>
      <ion-card-title>Consumo de Recursos</ion-card-title>
      <ion-card-subtitle>CPU, RAM y Batería</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="chart-content">
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

// Datos simulados de consumo de recursos para diferentes dispositivos
const resourceData = [
  {
    label: 'iPhone 13',
    cpu: 15,
    ram: 22,
    battery: 8
  },
  {
    label: 'Samsung S21',
    cpu: 18,
    ram: 25,
    battery: 10
  },
  {
    label: 'Pixel 6',
    cpu: 16,
    ram: 20,
    battery: 9
  },
  {
    label: 'Xiaomi Mi 11',
    cpu: 20,
    ram: 28,
    battery: 12
  },
  {
    label: 'iPhone 12',
    cpu: 17,
    ram: 24,
    battery: 9
  }
];

onMounted(() => {
  const ctx = chartCanvas.value.getContext('2d');
  
  chart = new Chart(ctx, {
    type: 'radar',
    data: {
      labels: ['CPU (%)', 'RAM (MB)', 'Batería (%/h)'],
      datasets: resourceData.map((device, index) => ({
        label: device.label,
        data: [device.cpu, device.ram, device.battery],
        backgroundColor: `rgba(255, 204, 0, ${0.7 - index * 0.1})`,
        borderColor: index === 0 ? '#ffcc00' : `rgba(${51 + index * 40}, ${51 + index * 40}, ${51 + index * 40}, 0.8)`,
        borderWidth: 2,
        pointBackgroundColor: index === 0 ? '#ffcc00' : `rgba(${51 + index * 40}, ${51 + index * 40}, ${51 + index * 40}, 0.8)`,
        pointBorderColor: '#fff',
        pointHoverBackgroundColor: '#fff',
        pointHoverBorderColor: index === 0 ? '#ffcc00' : `rgba(${51 + index * 40}, ${51 + index * 40}, ${51 + index * 40}, 0.8)`,
        pointRadius: 4,
        pointHoverRadius: 6
      }))
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
          backgroundColor: 'rgba(255, 255, 255, 0.95)',
          titleColor: '#333',
          bodyColor: '#666',
          borderColor: '#ffcc00',
          borderWidth: 1,
          padding: 10,
          displayColors: true,
          callbacks: {
            label: function(context) {
              const label = context.dataset.label || '';
              const value = context.raw;
              const metric = context.chart.data.labels[context.dataIndex];
              
              if (metric.includes('CPU')) {
                return `${label}: ${value}% de CPU`;
              } else if (metric.includes('RAM')) {
                return `${label}: ${value} MB de RAM`;
              } else if (metric.includes('Batería')) {
                return `${label}: ${value}% de batería por hora`;
              }
              return `${label}: ${value}`;
            }
          }
        }
      },
      scales: {
        r: {
          min: 0,
          max: 30,
          ticks: {
            stepSize: 5,
            backdropColor: 'rgba(255, 255, 255, 0.8)',
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
  
  // Usar ResizeObserver para manejar cambios de tamaño
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
  border-top: 3px solid #ffcc00;
  background-color: #ffffff;
  display: flex;
  flex-direction: column;
}

.chart-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 0;
  background-color: #ffffff;
}

canvas {
  flex: 1;
  min-height: 0;
  background-color: #ffffff;
}
</style>