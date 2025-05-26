<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Distribución de Estudiantes</ion-card-title>
      <ion-card-subtitle>Por especialidad FP</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="white-content">
      <div class="chart-wrapper">
        <div ref="chartContainer" class="chart-container"></div>
        <div class="legend-container">
          <div v-for="(item, index) in studentsData" :key="item.name" class="legend-item">
            <div class="legend-color" :style="{ backgroundColor: colors[index] }"></div>
            <div class="legend-text">
              <div class="legend-name">{{ item.name }}</div>
              <div class="legend-value">{{ item.value }} estudiantes</div>
            </div>
          </div>
        </div>
      </div>
    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent } from '@ionic/vue';
import * as echarts from 'echarts/core';
import { PieChart } from 'echarts/charts';
import { TooltipComponent } from 'echarts/components';
import { CanvasRenderer } from 'echarts/renderers';

echarts.use([PieChart, TooltipComponent, CanvasRenderer]);

const chartContainer = ref(null);
let chart = null;

const studentsData = [
  { value: 35, name: 'Desarrollo Web' },
  { value: 28, name: 'Sistemas Microinformáticos' },
  { value: 18, name: 'Administración de Sistemas' },
  { value: 12, name: 'Ciberseguridad' },
  { value: 7, name: 'Inteligencia Artificial' }
];

const colors = ['#2563eb', '#10ac84', '#54a0ff', '#ff6b6b', '#5f27cd'];

onMounted(() => {
  chart = echarts.init(chartContainer.value);
  
  const option = {
    backgroundColor: '#ffffff',
    tooltip: {
      trigger: 'item',
      formatter: '{b}: {c} estudiantes ({d}%)',
      backgroundColor: 'rgba(255, 255, 255, 0.9)',
      borderColor: '#2563eb',
      borderWidth: 1,
      textStyle: {
        color: '#333'
      }
    },
    series: [
      {
        name: 'Estudiantes',
        type: 'pie',
        radius: ['40%', '70%'],
        center: ['50%', '50%'],
        data: studentsData,
        emphasis: {
          itemStyle: {
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.5)',
          },
        },
        label: {
          show: false
        },
        labelLine: {
          show: false
        },
        color: colors
      }
    ]
  };
  
  chart.setOption(option);
  
  const resizeHandler = () => {
    if (chart) {
      chart.resize();
    }
  };
  
  window.addEventListener('resize', resizeHandler);
  
  const resizeObserver = new ResizeObserver(resizeHandler);
  resizeObserver.observe(chartContainer.value);
  
  onUnmounted(() => {
    window.removeEventListener('resize', resizeHandler);
    resizeObserver.disconnect();
    if (chart) {
      chart.dispose();
    }
  });
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
  padding: 8px;
}

.chart-wrapper {
  display: flex;
  height: 100%;
  gap: 16px;
}

.chart-container {
  flex: 1;
  background-color: #ffffff !important;
  min-width: 0;
}

.legend-container {
  width: 140px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 8px;
  padding: 8px;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 8px;
}

.legend-color {
  width: 12px;
  height: 12px;
  border-radius: 2px;
  flex-shrink: 0;
}

.legend-text {
  flex: 1;
  min-width: 0;
}

.legend-name {
  font-size: 11px;
  font-weight: 600;
  color: #374151;
  line-height: 1.2;
  margin-bottom: 2px;
}

.legend-value {
  font-size: 10px;
  color: #6b7280;
  line-height: 1.2;
}

@media (max-width: 768px) {
  .chart-wrapper {
    flex-direction: column;
    gap: 12px;
  }
  
  .legend-container {
    width: 100%;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    gap: 12px;
  }
  
  .legend-item {
    flex-direction: column;
    text-align: center;
    gap: 4px;
  }
  
  .legend-name {
    font-size: 10px;
  }
  
  .legend-value {
    font-size: 9px;
  }
}
</style>
