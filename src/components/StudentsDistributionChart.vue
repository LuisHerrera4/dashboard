<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Distribución de Estudiantes</ion-card-title>
      <ion-card-subtitle>Por especialidad FP</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="white-content">
      <div ref="chartContainer" class="chart-container"></div>
    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent } from '@ionic/vue';
import * as echarts from 'echarts/core';
import { PieChart } from 'echarts/charts';
import { TooltipComponent, LegendComponent, TitleComponent } from 'echarts/components';
import { CanvasRenderer } from 'echarts/renderers';

echarts.use([PieChart, TooltipComponent, LegendComponent, TitleComponent, CanvasRenderer]);

const chartContainer = ref(null);
let chart = null;

const studentsData = [
  { value: 35, name: 'Desarrollo Web' },
  { value: 28, name: 'Sistemas Microinformáticos' },
  { value: 18, name: 'Administración de Sistemas' },
  { value: 12, name: 'Ciberseguridad' },
  { value: 7, name: 'Inteligencia Artificial' }
];

onMounted(() => {
  chart = echarts.init(chartContainer.value);
  
  const option = {
    backgroundColor: '#ffffff',
    tooltip: {
      trigger: 'item',
      formatter: '{b}: {c} estudiantes ({d}%)',
      backgroundColor: 'rgba(255, 255, 255, 0.9)',
      borderColor: '#ffcc00',
      borderWidth: 1,
      textStyle: {
        color: '#333'
      }
    },
    legend: {
      orient: 'vertical',
      right: '5%',
      top: 'center',
      data: studentsData.map(item => item.name),
      textStyle: {
        color: '#666',
        fontSize: 12
      }
    },
    series: [
      {
        name: 'Estudiantes',
        type: 'pie',
        radius: ['40%', '70%'],
        center: ['40%', '50%'],
        data: studentsData,
        emphasis: {
          itemStyle: {
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.5)',
          },
        },
        color: ['#ffcc00', '#10ac84', '#54a0ff', '#ff6b6b', '#5f27cd']
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

.chart-container {
  width: 100%;
  height: 100%;
  background-color: #ffffff !important;
}
</style>
