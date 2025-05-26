<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="vueling">
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>
          <div class="title-container">
            <span>KPIs - QBid FP</span>
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
          <!-- KPIs de Negocio -->
          <ion-row class="kpi-section">
            <ion-col size="12">
              <h2 class="section-title">KPIs de Negocio</h2>
            </ion-col>
            <ion-col size="12" size-md="3" class="kpi-col">
              <div class="kpi-card success">
                <div class="kpi-content">
                  <div class="kpi-value">87.3%</div>
                  <div class="kpi-label">Tasa de Finalización</div>
                  <div class="kpi-trend positive">+5.2%</div>
                </div>
              </div>
            </ion-col>
            <ion-col size="12" size-md="3" class="kpi-col">
              <div class="kpi-card warning">
                <div class="kpi-content">
                  <div class="kpi-value">73.8%</div>
                  <div class="kpi-label">Participación Empresas</div>
                  <div class="kpi-trend positive">+8.1%</div>
                </div>
              </div>
            </ion-col>
            <ion-col size="12" size-md="3" class="kpi-col">
              <div class="kpi-card info">
                <div class="kpi-content">
                  <div class="kpi-value">2.4 días</div>
                  <div class="kpi-label">Tiempo Medio Asignación</div>
                  <div class="kpi-trend negative">-12.5%</div>
                </div>
              </div>
            </ion-col>
            <ion-col size="12" size-md="3" class="kpi-col">
              <div class="kpi-card primary">
                <div class="kpi-content">
                  <div class="kpi-value">91.2%</div>
                  <div class="kpi-label">Satisfacción Estudiantes</div>
                  <div class="kpi-trend positive">+3.7%</div>
                </div>
              </div>
            </ion-col>
          </ion-row>

          <!-- KPIs Técnicos -->
          <ion-row class="kpi-section">
            <ion-col size="12">
              <h2 class="section-title">KPIs Técnicos</h2>
            </ion-col>
            <ion-col size="12" size-md="3" class="kpi-col">
              <div class="kpi-card success">
                <div class="kpi-content">
                  <div class="kpi-value">99.7%</div>
                  <div class="kpi-label">Disponibilidad Sistema</div>
                  <div class="kpi-trend positive">+0.2%</div>
                </div>
              </div>
            </ion-col>
            <ion-col size="12" size-md="3" class="kpi-col">
              <div class="kpi-card warning">
                <div class="kpi-content">
                  <div class="kpi-value">245ms</div>
                  <div class="kpi-label">Tiempo Respuesta Medio</div>
                  <div class="kpi-trend negative">+8.3%</div>
                </div>
              </div>
            </ion-col>
            <ion-col size="12" size-md="3" class="kpi-col">
              <div class="kpi-card danger">
                <div class="kpi-content">
                  <div class="kpi-value">0.8%</div>
                  <div class="kpi-label">Tasa de Errores</div>
                  <div class="kpi-trend negative">+15.2%</div>
                </div>
              </div>
            </ion-col>
            <ion-col size="12" size-md="3" class="kpi-col">
              <div class="kpi-card info">
                <div class="kpi-content">
                  <div class="kpi-value">4.2h</div>
                  <div class="kpi-label">Resolución Soporte</div>
                  <div class="kpi-trend positive">-18.5%</div>
                </div>
              </div>
            </ion-col>
          </ion-row>
        </ion-grid>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup>
import { ref } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonButtons, IonButton, IonIcon, IonMenuButton } from '@ionic/vue';
import { 
  notificationsOutline, 
  personCircleOutline,
  schoolOutline,
  businessOutline,
  timeOutline,
  peopleOutline,
  checkmarkCircleOutline,
  speedometerOutline,
  bugOutline,
  chatbubbleEllipsesOutline,
  trendingUpOutline,
  trendingDownOutline
} from 'ionicons/icons';
import KPITrendChart from '../components/KPITrendChart.vue';
import PerformanceMetricsChart from '../components/PerformanceMetricsChart.vue';

// KPIs de Negocio
const businessKPIs = ref({
  practicesCompleted: {
    value: 87.3,
    label: 'Tasa de Finalización',
    trend: 5.2
  },
  companyEngagement: {
    value: 73.8,
    label: 'Participación Empresas',
    trend: 8.1
  },
  avgAssignmentTime: {
    value: '2.4 días',
    label: 'Tiempo Medio Asignación',
    trend: 12.5
  },
  studentSatisfaction: {
    value: 91.2,
    label: 'Satisfacción Estudiantes',
    trend: 3.7
  }
});

// KPIs Técnicos
const technicalKPIs = ref({
  systemUptime: {
    value: 99.7,
    label: 'Disponibilidad Sistema',
    trend: 0.2
  },
  avgResponseTime: {
    value: 245,
    label: 'Tiempo Respuesta Medio',
    trend: 8.3
  },
  errorRate: {
    value: 0.8,
    label: 'Tasa de Errores',
    trend: 15.2
  },
  supportResolution: {
    value: 4.2,
    label: 'Resolución Soporte',
    trend: 18.5
  }
});
</script>

<style scoped>
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
  overflow-y: auto;
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
}

.dashboard-grid {
  margin: 0;
  padding: 16px;
}

.section-title {
  font-size: 1.5rem;
  font-weight: 600;
  color: #333;
  margin: 0 0 16px 0;
  padding-bottom: 8px;
  border-bottom: 2px solid #ffcc00;
}

.kpi-section {
  margin-bottom: 32px;
}

.kpi-col {
  padding: 8px;
}

.kpi-card {
  background: #ffffff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  border-left: 4px solid;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.kpi-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.kpi-card.success {
  border-left-color: #10ac84;
}

.kpi-card.warning {
  border-left-color: #ffcc00;
}

.kpi-card.info {
  border-left-color: #54a0ff;
}

.kpi-card.primary {
  border-left-color: #5f27cd;
}

.kpi-card.danger {
  border-left-color: #ff6b6b;
}

.kpi-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
}

.success .kpi-icon {
  background-color: rgba(16, 172, 132, 0.1);
  color: #10ac84;
}

.warning .kpi-icon {
  background-color: rgba(255, 204, 0, 0.1);
  color: #ffcc00;
}

.info .kpi-icon {
  background-color: rgba(84, 160, 255, 0.1);
  color: #54a0ff;
}

.primary .kpi-icon {
  background-color: rgba(95, 39, 205, 0.1);
  color: #5f27cd;
}

.danger .kpi-icon {
  background-color: rgba(255, 107, 107, 0.1);
  color: #ff6b6b;
}

.kpi-content {
  flex: 1;
}

.kpi-value {
  font-size: 2rem;
  font-weight: bold;
  color: #333;
  line-height: 1;
  margin-bottom: 4px;
}

.kpi-label {
  font-size: 0.9rem;
  color: #666;
  margin-bottom: 8px;
}

.kpi-trend {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 0.8rem;
  font-weight: 600;
}

.kpi-trend.positive {
  color: #10ac84;
}

.kpi-trend.negative {
  color: #ff6b6b;
}

.dashboard-row {
  margin: 0;
  padding: 0;
}

.chart-col {
  padding: 8px;
  display: flex;
  flex-direction: column;
  min-height: 400px;
}

@media (max-width: 768px) {
  .dashboard-grid {
    padding: 8px;
  }
  
  .kpi-col {
    padding: 4px;
  }
  
  .kpi-card {
    padding: 16px;
  }
  
  .kpi-value {
    font-size: 1.5rem;
  }
  
  .section-title {
    font-size: 1.3rem;
  }
}
</style>
