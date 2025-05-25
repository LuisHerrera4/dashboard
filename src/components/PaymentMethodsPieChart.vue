<template>
  <ion-card class="chart-card">
    <ion-card-header>
      <ion-card-title>Métodos de Pago</ion-card-title>
      <ion-card-subtitle>Distribución de transacciones</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="chart-content">
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

// Datos simulados de métodos de pago
const paymentData = [
  { value: 58, name: 'Tarjeta de Crédito' },
  { value: 25, name: 'PayPal' },
  { value: 8, name: 'Bizum' },
  { value: 5, name: 'Apple Pay' },
  { value: 4, name: 'Google Pay' }
];

onMounted(() => {
  chart = echarts.init(chartContainer.value, null, {
    backgroundColor: '#ffffff'
  });
  
  const option = {
    backgroundColor: '#ffffff',
    tooltip: {
      trigger: 'item',
      formatter: '{b}: {c}% ({d}%)',
      backgroundColor: 'rgba(255, 255, 255, 0.95)',
      borderColor: '#ffcc00',
      borderWidth: 1,
      textStyle: {
        color: '#333'
      }
    },
    legend: {
      orient: 'vertical',
      right: '10%',
      top: 'center',
      data: paymentData.map(item => item.name),
      textStyle: {
        color: '#333',
        fontSize: 11
      },
      itemWidth: 12,
      itemHeight: 12,
      itemGap: 8,
      backgroundColor: '#ffffff'
    },
    series: [
      {
        name: 'Métodos de Pago',
        type: 'pie',
        radius: ['35%', '65%'],
        center: ['35%', '50%'],
        avoidLabelOverlap: false,
        itemStyle: {
          borderRadius: 8,
          borderColor: '#ffffff',
          borderWidth: 2
        },
        label: {
          show: false
        },
        emphasis: {
          label: {
            show: true,
            fontSize: '14',
            fontWeight: 'bold',
            color: '#333'
          },
          itemStyle: {
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.2)'
          }
        },
        labelLine: {
          show: false
        },
        data: paymentData,
        color: ['#ffcc00', '#ff9f43', '#ee5253', '#0abde3', '#10ac84']
      }
    ]
  };
  
  chart.setOption(option);
  
  // Hacer responsivo
  const resizeHandler = () => {
    if (chart) {
      chart.resize();
    }
  };
  
  window.addEventListener('resize', resizeHandler);
  
  // Usar ResizeObserver para detectar cambios en el contenedor
  const resizeObserver = new ResizeObserver(resizeHandler);
  resizeObserver.observe(chartContainer.value);
  
  // Limpiar al desmontar
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

.chart-container {
  width: 100%;
  height: 100%;
  background-color: #ffffff;
  flex: 1;
  min-height: 0;
}
</style>