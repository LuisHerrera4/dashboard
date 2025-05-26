<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Tiempo de Respuesta de la Plataforma</ion-card-title>
      <ion-card-subtitle>Por funcionalidad (en milisegundos)</ion-card-subtitle>
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
import { BarChart } from 'echarts/charts';
import { TooltipComponent, LegendComponent, GridComponent } from 'echarts/components';
import { CanvasRenderer } from 'echarts/renderers';

echarts.use([BarChart, TooltipComponent, LegendComponent, GridComponent, CanvasRenderer]);

const chartContainer = ref(null);
let chart = null;

// Datos simulados de tiempos de respuesta por funcionalidad de EasyFCT
const responseTimeData = [
  { functionality: 'Login Estudiantes', responseTime: 180 },
  { functionality: 'Portal Empresas', responseTime: 320 },
  { functionality: 'Búsqueda Ofertas', responseTime: 450 },
  { functionality: 'Asignar Práctica', responseTime: 280 },
  { functionality: 'Subir Documentos', responseTime: 620 },
  { functionality: 'Evaluaciones', responseTime: 240 },
  { functionality: 'Generar Reportes', responseTime: 890 },
  { functionality: 'Notificaciones', responseTime: 150 },
  { functionality: 'Chat Soporte', responseTime: 380 },
  { functionality: 'Calendario', responseTime: 220 }
];

const kpiTarget = 200; // KPI: 200ms máximo

responseTimeData.sort((a, b) => b.responseTime - a.responseTime);

onMounted(() => {
  chart = echarts.init(chartContainer.value);
  
  const option = {
    backgroundColor: '#ffffff',
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'shadow'
      },
      formatter: function(params) {
        return `<div style="font-weight:bold;margin-bottom:5px;">${params[0].name}</div>` +
               `<div>Tiempo de respuesta: <span style="font-weight:bold;color:#2563eb;">${params[0].value} ms</span></div>`;
      },
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
      bottom: '8%',
      top: '3%',
      containLabel: true
    },
    xAxis: {
      type: 'value',
      name: 'Tiempo (ms)',
      nameLocation: 'middle',
      nameGap: 30,
      nameTextStyle: {
        color: '#666',
        fontSize: 12
      },
      axisLine: {
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
    yAxis: {
      type: 'category',
      data: responseTimeData.map(item => item.functionality),
      axisLine: {
        lineStyle: {
          color: '#ddd'
        }
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        color: '#666'
      }
    },
    series: [
      {
        name: 'Tiempo de Respuesta',
        type: 'bar',
        data: responseTimeData.map(item => item.responseTime),
        itemStyle: {
          color: function(params) {
            const value = params.value;
            if (value > 600) return '#ff6b6b'; // Rojo para tiempos lentos
            if (value > 300) return '#ffd166'; // Amarillo para tiempos medios
            return '#2563eb'; // Azul para tiempos rápidos
          },
          borderRadius: [0, 4, 4, 0]
        },
        barWidth: '60%',
        label: {
          show: true,
          position: 'right',
          formatter: '{c} ms',
          color: '#666'
        },
        markLine: {
          data: [
            {
              xAxis: kpiTarget,
              lineStyle: {
                color: '#ef4444',
                type: 'dashed',
                width: 2
              },
              label: {
                formatter: 'KPI: 200ms',
                position: 'insideEndTop',
                color: '#ef4444',
                fontWeight: 'bold'
              }
            }
          ]
        }
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
}

.chart-container {
  width: 100%;
  height: 100%;
  background-color: #ffffff !important;
}
</style>
