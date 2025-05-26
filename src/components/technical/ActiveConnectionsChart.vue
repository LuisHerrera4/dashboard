<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Conexiones Activas por Región</ion-card-title>
      <ion-card-subtitle>Usuarios conectados en tiempo real</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="white-content">
      <div class="connections-summary">
        <div class="total-connections">
          <div class="count">{{ totalConnections }}</div>
          <div class="label">conexiones totales</div>
        </div>
      </div>
      <div ref="chartContainer" class="chart-container"></div>
    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';
import { IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent } from '@ionic/vue';
import * as echarts from 'echarts/core';
import { BarChart } from 'echarts/charts';
import { TooltipComponent, LegendComponent, GridComponent } from 'echarts/components';
import { CanvasRenderer } from 'echarts/renderers';

echarts.use([BarChart, TooltipComponent, LegendComponent, GridComponent, CanvasRenderer]);

const chartContainer = ref(null);
const connectionsData = ref([
  { region: 'Madrid', connections: 145 },
  { region: 'Cataluña', connections: 98 },
  { region: 'Andalucía', connections: 87 },
  { region: 'Valencia', connections: 65 },
  { region: 'País Vasco', connections: 42 },
  { region: 'Galicia', connections: 38 }
]);

const totalConnections = computed(() => {
  return connectionsData.value.reduce((sum, item) => sum + item.connections, 0);
});

let chart = null;
let updateInterval = null;

const updateConnectionsData = () => {
  connectionsData.value = connectionsData.value.map(item => {
    const fluctuation = (Math.random() * 0.2) - 0.1;
    const newConnections = Math.max(10, Math.round(item.connections * (1 + fluctuation)));
    
    return {
      region: item.region,
      connections: newConnections
    };
  });
  
  if (chart) {
    chart.setOption({
      series: [{
        data: connectionsData.value.map(item => ({
          value: item.connections,
          name: item.region
        }))
      }]
    });
  }
};

onMounted(() => {
  chart = echarts.init(chartContainer.value);
  
  const option = {
    backgroundColor: '#ffffff',
    tooltip: {
      trigger: 'item',
      formatter: '{b}: {c} conexiones',
      backgroundColor: 'rgba(255, 255, 255, 0.9)',
      borderColor: '#2563eb',
      borderWidth: 1,
      textStyle: {
        color: '#333'
      }
    },
    grid: {
      left: '3%',
      right: '4%',
      bottom: '3%',
      top: '3%',
      containLabel: true
    },
    xAxis: {
      type: 'category',
      data: connectionsData.value.map(item => item.region),
      axisLine: {
        lineStyle: {
          color: '#ddd'
        }
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        color: '#666',
        rotate: 0
      }
    },
    yAxis: {
      type: 'value',
      name: 'Conexiones',
      nameTextStyle: {
        color: '#666'
      },
      axisLine: {
        show: true,
        lineStyle: {
          color: '#ddd'
        }
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        color: '#666'
      },
      splitLine: {
        lineStyle: {
          color: 'rgba(200, 200, 200, 0.3)'
        }
      }
    },
    series: [{
      type: 'bar',
      data: connectionsData.value.map(item => ({
        value: item.connections,
        name: item.region,
        itemStyle: {
          color: function(params) {
            const colors = ['#2563eb', '#3b82f6', '#60a5fa', '#93c5fd', '#dbeafe', '#eff6ff'];
            return colors[params.dataIndex % colors.length];
          }
        }
      })),
      barWidth: '50%',
      label: {
        show: true,
        position: 'top',
        color: '#666'
      },
      emphasis: {
        itemStyle: {
          shadowBlur: 10,
          shadowOffsetX: 0,
          shadowColor: 'rgba(0, 0, 0, 0.2)'
        }
      },
      animationDuration: 300,
      animationEasing: 'cubicOut'
    }]
  };
  
  chart.setOption(option);
  
  updateInterval = setInterval(updateConnectionsData, 3000);
  
  const resizeHandler = () => {
    if (chart) {
      chart.resize();
    }
  };
  
  window.addEventListener('resize', resizeHandler);
  
  const resizeObserver = new ResizeObserver(resizeHandler);
  resizeObserver.observe(chartContainer.value);
  
  onUnmounted(() => {
    if (updateInterval) {
      clearInterval(updateInterval);
    }
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
  position: relative;
}

.connections-summary {
  position: absolute;
  top: 10px;
  right: 20px;
  z-index: 10;
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 8px;
  padding: 8px 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  border: 1px solid #2563eb;
}

.total-connections {
  text-align: center;
}

.count {
  font-size: 1.8rem;
  font-weight: bold;
  color: #2563eb;
}

.label {
  font-size: 0.8rem;
  color: #666;
}

.chart-container {
  width: 100%;
  height: 100%;
  background-color: #ffffff !important;
}
</style>
