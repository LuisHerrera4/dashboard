<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Mapa de Consultas de Soporte</ion-card-title>
      <ion-card-subtitle>Por hora y día de la semana</ion-card-subtitle>
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
import { HeatmapChart } from 'echarts/charts';
import { TooltipComponent, VisualMapComponent, GridComponent } from 'echarts/components';
import { CanvasRenderer } from 'echarts/renderers';

echarts.use([HeatmapChart, TooltipComponent, VisualMapComponent, GridComponent, CanvasRenderer]);

const chartContainer = ref(null);
let chart = null;

const generateHeatmapData = () => {
  const hours = ['8h', '10h', '12h', '14h', '16h', '18h'];
  const days = ['Lun', 'Mar', 'Mié', 'Jue', 'Vie'];
  
  const data = [];
  for (let i = 0; i < days.length; i++) {
    for (let j = 0; j < hours.length; j++) {
      let baseValue = 0;
      
      if (j >= 2 && j <= 4) {
        baseValue += 30;
      }
      
      if (i < 5) {
        baseValue += 20;
      }
      
      if (i === 0 && j === 2) {
        baseValue += 40;
      }
      
      if (i === 4 && j === 4) {
        baseValue += 50;
      }
      
      const value = baseValue + Math.round(Math.random() * 30);
      data.push([j, i, value]);
    }
  }
  
  return { hours, days, data };
};

onMounted(() => {
  const { hours, days, data } = generateHeatmapData();
  chart = echarts.init(chartContainer.value);
  
  const option = {
    backgroundColor: '#ffffff',
    tooltip: {
      position: 'top',
      formatter: function (params) {
        return `${days[params.value[1]]}, ${hours[params.value[0]]}<br>Consultas: ${params.value[2]}`;
      },
      backgroundColor: 'rgba(255, 255, 255, 0.9)',
      borderColor: '#ffcc00',
      borderWidth: 1,
      textStyle: {
        color: '#333'
      }
    },
    grid: {
      top: '10%',
      left: '3%',
      right: '5%',
      bottom: '15%',
      containLabel: true
    },
    xAxis: {
      type: 'category',
      data: hours,
      splitArea: {
        show: true
      },
      axisLabel: {
        fontSize: 10,
        color: '#666'
      }
    },
    yAxis: {
      type: 'category',
      data: days,
      splitArea: {
        show: true
      },
      axisLabel: {
        color: '#666'
      }
    },
    visualMap: {
      min: 0,
      max: 100,
      calculable: true,
      orient: 'horizontal',
      left: 'center',
      bottom: '0%',
      color: ['#ffcc00', '#ffd633', '#ffe066', '#ffeb99', '#fff5cc'],
      textStyle: {
        color: '#666'
      }
    },
    series: [{
      name: 'Consultas',
      type: 'heatmap',
      data: data,
      label: {
        show: false
      },
      emphasis: {
        itemStyle: {
          shadowBlur: 10,
          shadowColor: 'rgba(0, 0, 0, 0.5)'
        }
      }
    }]
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
