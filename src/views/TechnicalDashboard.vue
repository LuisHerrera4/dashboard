<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="easyfct">
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>
          <div class="title-container">
            <span>Dashboard Técnico - EasyFCT</span>
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
    
    <ion-content class="white-background" :scroll-y="false">
      <div class="dashboard-container">
        <ion-grid class="ion-no-padding">
          <ion-row class="ion-padding">
            <ion-col size="12" size-md="6" class="ion-padding-horizontal ion-padding-bottom">
              <SystemErrorsChart />
            </ion-col>
            <ion-col size="12" size-md="6" class="ion-padding-horizontal ion-padding-bottom">
              <ResponseTimeChart />
            </ion-col>
          </ion-row>
          
          <ion-row class="ion-padding">
            <ion-col size="12" size-md="4" class="ion-padding-horizontal ion-padding-bottom">
              <ServerResourcesChart />
            </ion-col>
            <ion-col size="12" size-md="4" class="ion-padding-horizontal ion-padding-bottom">
              <ActiveConnectionsChart />
            </ion-col>
            <ion-col size="12" size-md="4" class="ion-padding-horizontal ion-padding-bottom">
              <SystemAlertsChart />
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
import SystemErrorsChart from '../components/technical/SystemErrorsChart.vue';
import ResponseTimeChart from '../components/technical/ResponseTimeChart.vue';
import ServerResourcesChart from '../components/technical/ServerResourcesChart.vue';
import ActiveConnectionsChart from '../components/technical/ActiveConnectionsChart.vue';
import SystemAlertsChart from '../components/technical/SystemAlertsChart.vue';
</script>

<style scoped>
ion-page {
  --background: #ffffff !important;
}

ion-content {
  --background: #ffffff !important;
  --overflow: hidden;
}

.white-background {
  background-color: #ffffff !important;
}

ion-toolbar {
  --background: #2563eb;
  --color: white;
}

.title-container {
  display: flex;
  align-items: center;
  gap: 10px;
}

.dashboard-container {
  max-width: 1600px;
  margin: 0 auto;
  background-color: #ffffff;
  height: 100%;
  overflow: hidden;
}

ion-grid {
  height: 100%;
  background-color: #ffffff;
}

ion-row {
  margin: 0;
  height: 50%;
}

ion-col {
  display: flex;
  flex-direction: column;
}

@media (max-width: 768px) {
  ion-row {
    height: auto;
  }
  
  ion-col {
    padding: 8px;
  }
}
</style>
