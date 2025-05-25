<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="vueling">
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>
          <div class="title-container">
            <span>Dashboard Técnico</span>
          </div>
        </ion-title>
        <ion-buttons slot="end">
          <ion-button>
            <ion-icon :icon="notificationsOutline"></ion-icon>
          </ion-button>
          <ion-button>
            <ion-icon :icon="personCircleOutline"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    
    <ion-content class="dashboard-content">
      <div class="dashboard-container">
        <ion-grid class="dashboard-grid">
          <ion-row class="dashboard-row">
            <ion-col size="12" size-md="6" class="chart-col">
              <ErrorRateChart />
            </ion-col>
            <ion-col size="12" size-md="6" class="chart-col">
              <LoadTimeChart />
            </ion-col>
          </ion-row>
          
          <ion-row class="dashboard-row">
            <ion-col size="12" size-md="4" class="chart-col">
              <ResourceUsageChart />
            </ion-col>
            <ion-col size="12" size-md="4" class="chart-col">
              <RealtimeConnectionsChart />
            </ion-col>
            <ion-col size="12" size-md="4" class="chart-col">
              <AlertsChart />
            </ion-col>
          </ion-row>
        </ion-grid>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonButtons, IonButton, IonIcon, IonMenuButton } from '@ionic/vue';
import { notificationsOutline, personCircleOutline } from 'ionicons/icons';
import ErrorRateChart from '../components/technical/ErrorRateChart.vue';
import LoadTimeChart from '../components/technical/LoadTimeChart.vue';
import ResourceUsageChart from '../components/technical/ResourceUsageChart.vue';
import RealtimeConnectionsChart from '../components/technical/RealTimeConnectionsChart.vue';
import AlertsChart from '../components/technical/AlertsChart.vue';
</script>

<style scoped>
/* Forzar fondo blanco en toda la página */
ion-page {
  --background: #ffffff !important;
}

.dashboard-content {
  --background: #ffffff !important;
  --padding-top: 0;
  --padding-bottom: 0;
  --padding-start: 0;
  --padding-end: 0;
  height: 100%;
  overflow: hidden;
}

ion-toolbar {
  --background: #ffcc00;
  --color: #333;
}

.title-container {
  display: flex;
  align-items: center;
  gap: 10px;
}

.dashboard-container {
  height: 100%;
  width: 100%;
  background-color: #ffffff;
  overflow: hidden;
}

.dashboard-grid {
  height: 100%;
  margin: 0;
  padding: 8px;
  display: flex;
  flex-direction: column;
}

.dashboard-row {
  flex: 1;
  margin: 0;
  padding: 0;
  display: flex;
  min-height: 0;
}

.chart-col {
  padding: 4px;
  display: flex;
  flex-direction: column;
  min-height: 0;
  height: 100%;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .dashboard-grid {
    padding: 4px;
  }
  
  .chart-col {
    padding: 2px;
  }
  
  .dashboard-row {
    flex-direction: column;
  }
  
  .dashboard-row:first-child {
    flex: 1.2;
  }
  
  .dashboard-row:last-child {
    flex: 1.8;
  }
}

@media (min-width: 769px) {
  .dashboard-row:first-child {
    flex: 1;
  }
  
  .dashboard-row:last-child {
    flex: 1;
  }
}
</style>