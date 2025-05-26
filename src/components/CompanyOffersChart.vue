<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Ofertas Publicadas por Empresa</ion-card-title>
      <ion-card-subtitle>Últimos 3 meses</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="white-content">
      <div id="company-offers-chart"></div>
    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { onMounted, onUnmounted } from 'vue';
import { IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent } from '@ionic/vue';
import ApexCharts from 'apexcharts';

const offersData = [
  {
    name: 'TechCorp',
    data: [12, 15, 18]
  },
  {
    name: 'InnovaSoft',
    data: [8, 11, 14]
  },
  {
    name: 'DataSystems',
    data: [6, 9, 12]
  },
  {
    name: 'WebSolutions',
    data: [5, 7, 10]
  },
  {
    name: 'CloudTech',
    data: [9, 12, 15]
  }
];

let chart = null;

onMounted(() => {
  const options = {
    series: offersData,
    chart: {
      type: 'bar',
      height: 300,
      background: '#ffffff'
    },
    plotOptions: {
      bar: {
        horizontal: false,
        columnWidth: '55%',
        borderRadius: 5
      },
    },
    dataLabels: {
      enabled: false
    },
    stroke: {
      show: true,
      width: 2,
      colors: ['transparent']
    },
    xaxis: {
      categories: ['Abril', 'Mayo', 'Junio'],
    },
    yaxis: {
      title: {
        text: 'Número de Ofertas',
      }
    },
    fill: {
      opacity: 1
    },
    tooltip: {
      y: {
        formatter: function (val) {
          return val + " ofertas"
        }
      }
    },
    colors: ['#ffcc00', '#10ac84', '#54a0ff', '#ff6b6b', '#5f27cd']
  };

  chart = new ApexCharts(document.querySelector("#company-offers-chart"), options);
  chart.render();
});

onUnmounted(() => {
  if (chart) {
    chart.destroy();
  }
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

#company-offers-chart {
  width: 100%;
  height: 100%;
  background-color: #ffffff !important;
}
</style>
