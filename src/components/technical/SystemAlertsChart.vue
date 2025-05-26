<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Alertas Técnicas del Sistema</ion-card-title>
      <ion-card-subtitle>Últimas 24 horas</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="white-content">
      <div id="system-alerts-chart"></div>
    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { onMounted, onUnmounted } from 'vue';
import { IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent } from '@ionic/vue';
import ApexCharts from 'apexcharts';

const alertsData = [
  { type: 'Timeout Base Datos', count: 28 },
  { type: 'Error Subida Archivos', count: 22 },
  { type: 'Fallo Autenticación', count: 18 },
  { type: 'Memoria Servidor Llena', count: 15 },
  { type: 'API Empresas Lenta', count: 12 },
  { type: 'Error Notificaciones', count: 9 },
  { type: 'Backup Fallido', count: 7 },
  { type: 'SSL Certificado', count: 4 }
];

alertsData.sort((a, b) => b.count - a.count);

let chart = null;

onMounted(() => {
  const options = {
    series: [{
      name: 'Ocurrencias',
      data: alertsData.map(item => item.count)
    }],
    chart: {
      type: 'bar',
      height: 300,
      toolbar: {
        show: false
      },
      fontFamily: 'inherit',
      background: '#ffffff'
    },
    plotOptions: {
      bar: {
        horizontal: true,
        distributed: true,
        barHeight: '70%',
        borderRadius: 4
      }
    },
    dataLabels: {
      enabled: true,
      textAnchor: 'start',
      style: {
        colors: ['#fff'],
        fontWeight: 'bold',
        fontSize: '12px'
      },
      formatter: function(val, opt) {
        return val;
      },
      offsetX: 0
    },
    colors: [
      '#ef4444', // Rojo para el más crítico
      '#f97316', // Naranja
      '#eab308', // Amarillo
      '#22c55e', // Verde
      '#3b82f6', // Azul
      '#8b5cf6', // Púrpura
      '#06b6d4', // Cian
      '#84cc16'  // Lima
    ],
    xaxis: {
      categories: alertsData.map(item => item.type),
      labels: {
        style: {
          colors: '#666',
          fontSize: '12px'
        }
      },
      axisBorder: {
        show: false
      },
      axisTicks: {
        show: false
      }
    },
    yaxis: {
      labels: {
        style: {
          colors: '#666',
          fontSize: '12px'
        }
      }
    },
    grid: {
      borderColor: 'rgba(200, 200, 200, 0.2)',
      row: {
        colors: ['transparent', 'transparent']
      },
      xaxis: {
        lines: {
          show: false
        }
      },
      yaxis: {
        lines: {
          show: false
        }
      }
    },
    tooltip: {
      theme: 'light',
      x: {
        show: false
      },
      y: {
        title: {
          formatter: function() {
            return 'Ocurrencias:';
          }
        }
      },
      style: {
        fontSize: '12px'
      }
    },
    legend: {
      show: false
    },
    states: {
      hover: {
        filter: {
          type: 'lighten',
          value: 0.1
        }
      },
      active: {
        filter: {
          type: 'darken',
          value: 0.1
        }
      }
    }
  };

  chart = new ApexCharts(document.querySelector("#system-alerts-chart"), options);
  chart.render();
  
  const resizeHandler = () => {
    if (chart) {
      chart.updateOptions({
        chart: {
          height: document.querySelector("#system-alerts-chart").offsetHeight
        }
      });
    }
  };
  
  window.addEventListener('resize', resizeHandler);
  
  const resizeObserver = new ResizeObserver(resizeHandler);
  if (document.querySelector("#system-alerts-chart")) {
    resizeObserver.observe(document.querySelector("#system-alerts-chart").parentElement);
  }
  
  onUnmounted(() => {
    window.removeEventListener('resize', resizeHandler);
    resizeObserver.disconnect();
    if (chart) {
      chart.destroy();
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

#system-alerts-chart {
  width: 100%;
  height: 100%;
  background-color: #ffffff !important;
}
</style>
