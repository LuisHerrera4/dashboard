<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="easyfct">
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>
          <div class="title-container">
             KPIs - EasyFCT
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
    
    <ion-content class="dashboard-content" :fullscreen="true">
      <div class="dashboard-container">
        <!-- KPIs de Negocio -->
        <div class="section-wrapper">
          <div class="section-header">
            <div class="section-badge business">
              <ion-icon :icon="trendingUpOutline"></ion-icon>
              <span>KPIs de Negocio</span>
            </div>
          </div>
          
          <div class="kpis-grid">
            <div v-for="item in businessKPIs" :key="item.id" class="kpi-card" @click="toggleKPI(item.id)">
              <div class="kpi-header">
                <div class="kpi-icon-wrapper">
                  <ion-icon :icon="item.icon" class="kpi-icon"></ion-icon>
                </div>
                <div class="kpi-info">
                  <h3 class="kpi-title">{{ item.title }}</h3>
                  <div class="kpi-value">{{ item.currentValue }}</div>
                </div>
                <div class="kpi-trend">
                  <span :class="['trend-value', item.trendColor]">
                    <ion-icon :icon="item.trendIcon"></ion-icon>
                    {{ item.trend }}
                  </span>
                </div>
              </div>
              
              <div v-if="expandedKPIs.includes(item.id)" class="kpi-details">
                <p class="kpi-description">{{ item.description }}</p>
                <div class="smart-objectives">
                  <div v-for="(element, index) in item.smart" :key="index" class="smart-item">
                    <span class="smart-letter">{{ element.letter }}</span>
                    <span class="smart-text">{{ element.content }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- KPIs Técnicos -->
        <div class="section-wrapper">
          <div class="section-header">
            <div class="section-badge technical">
              <ion-icon :icon="settingsOutline"></ion-icon>
              <span>KPIs Técnicos</span>
            </div>
          </div>
          
          <div class="kpis-grid">
            <div v-for="item in technicalKPIs" :key="item.id" class="kpi-card" @click="toggleKPI(item.id)">
              <div class="kpi-header">
                <div class="kpi-icon-wrapper">
                  <ion-icon :icon="item.icon" class="kpi-icon"></ion-icon>
                </div>
                <div class="kpi-info">
                  <h3 class="kpi-title">{{ item.title }}</h3>
                  <div class="kpi-value">{{ item.currentValue }}</div>
                </div>
                <div class="kpi-trend">
                  <span :class="['trend-value', item.trendColor]">
                    <ion-icon :icon="item.trendIcon"></ion-icon>
                    {{ item.trend }}
                  </span>
                </div>
              </div>
              
              <div v-if="expandedKPIs.includes(item.id)" class="kpi-details">
                <p class="kpi-description">{{ item.description }}</p>
                <div class="smart-objectives">
                  <div v-for="(element, index) in item.smart" :key="index" class="smart-item">
                    <span class="smart-letter">{{ element.letter }}</span>
                    <span class="smart-text">{{ element.content }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { 
  IonPage, 
  IonHeader, 
  IonToolbar, 
  IonTitle, 
  IonContent, 
  IonButtons, 
  IonButton, 
  IonIcon, 
  IonMenuButton
} from '@ionic/vue';
import { 
  notificationsOutline, 
  personCircleOutline,
  trendingUpOutline,
  trendingDownOutline,
  settingsOutline,
  schoolOutline,
  businessOutline,
  timeOutline,
  happyOutline,
  serverOutline,
  speedometerOutline,
  bugOutline,
  chatbubbleEllipsesOutline
} from 'ionicons/icons';

interface SmartElement {
  letter: string;
  content: string;
}

interface SmartGoal {
  id: number;
  title: string;
  description: string;
  currentValue: string;
  trend: string;
  trendColor: string;
  trendIcon: string;
  icon: string;
  smart: SmartElement[];
}

const expandedKPIs = ref<number[]>([]);

const toggleKPI = (id: number) => {
  const index = expandedKPIs.value.indexOf(id);
  if (index > -1) {
    expandedKPIs.value.splice(index, 1);
  } else {
    expandedKPIs.value.push(id);
  }
};

const businessKPIs = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Tasa de Finalización de Prácticas",
    description: "Aumentar la tasa de finalización exitosa de prácticas del 85% al 95% en los próximos 6 meses, implementando un sistema de seguimiento semanal y tutorías personalizadas para mejorar la empleabilidad de los estudiantes de FP.",
    currentValue: "87.3%",
    trend: "+5.2%",
    trendColor: "success",
    trendIcon: trendingUpOutline,
    icon: schoolOutline,
    smart: [
      { letter: "S", content: "Aumentar la tasa de finalización exitosa de prácticas del 85% al 95%" },
      { letter: "M", content: "Del 85% al 95% (incremento del 10%)" },
      { letter: "A", content: "Implementando sistema de seguimiento semanal y tutorías personalizadas" },
      { letter: "R", content: "Para mejorar la empleabilidad de los estudiantes de FP" },
      { letter: "T", content: "En los próximos 6 meses" }
    ]
  },
  {
    id: 2,
    title: "Participación de Empresas",
    description: "Incrementar la participación activa de empresas colaboradoras del 70% al 85% en 4 meses, mediante la creación de un portal empresarial mejorado y eventos de networking trimestrales para fortalecer las alianzas educación-empresa.",
    currentValue: "73.8%",
    trend: "+8.1%",
    trendColor: "success",
    trendIcon: trendingUpOutline,
    icon: businessOutline,
    smart: [
      { letter: "S", content: "Incrementar la participación activa de empresas colaboradoras del 70% al 85%" },
      { letter: "M", content: "Del 70% al 85% (incremento del 15%)" },
      { letter: "A", content: "Creando portal empresarial mejorado y eventos de networking trimestrales" },
      { letter: "R", content: "Para fortalecer las alianzas educación-empresa" },
      { letter: "T", content: "En 4 meses" }
    ]
  },
  {
    id: 3,
    title: "Tiempo de Asignación de Prácticas",
    description: "Reducir el tiempo medio de asignación de prácticas de 5 días a 2 días en los próximos 3 meses, automatizando el proceso de matching estudiante-empresa y optimizando los flujos de trabajo para mejorar la experiencia del usuario.",
    currentValue: "2.4 días",
    trend: "-12.5%",
    trendColor: "success",
    trendIcon: trendingDownOutline,
    icon: timeOutline,
    smart: [
      { letter: "S", content: "Reducir el tiempo medio de asignación de prácticas de 5 días a 2 días" },
      { letter: "M", content: "De 5 días a 2 días (reducción del 60%)" },
      { letter: "A", content: "Automatizando el proceso de matching estudiante-empresa" },
      { letter: "R", content: "Para mejorar la experiencia del usuario" },
      { letter: "T", content: "En los próximos 3 meses" }
    ]
  },
  {
    id: 4,
    title: "Satisfacción de Estudiantes",
    description: "Mantener la satisfacción de estudiantes por encima del 90% durante todo el año académico, implementando encuestas mensuales y un sistema de feedback continuo para asegurar la calidad del programa de prácticas.",
    currentValue: "91.2%",
    trend: "+3.7%",
    trendColor: "success",
    trendIcon: trendingUpOutline,
    icon: happyOutline,
    smart: [
      { letter: "S", content: "Mantener la satisfacción de estudiantes por encima del 90%" },
      { letter: "M", content: "Por encima del 90% (actualmente 91.2%)" },
      { letter: "A", content: "Implementando encuestas mensuales y sistema de feedback continuo" },
      { letter: "R", content: "Para asegurar la calidad del programa de prácticas" },
      { letter: "T", content: "Durante todo el año académico" }
    ]
  }
]);

const technicalKPIs = ref<SmartGoal[]>([
  {
    id: 5,
    title: "Disponibilidad del Sistema",
    description: "Mantener la disponibilidad del sistema EasyFCT por encima del 99.5% mensualmente, implementando monitoreo 24/7 y redundancia de servidores para garantizar el acceso continuo a la plataforma educativa.",
    currentValue: "99.7%",
    trend: "+0.2%",
    trendColor: "success",
    trendIcon: trendingUpOutline,
    icon: serverOutline,
    smart: [
      { letter: "S", content: "Mantener la disponibilidad del sistema EasyFCT por encima del 99.5%" },
      { letter: "M", content: "Por encima del 99.5% mensualmente (actualmente 99.7%)" },
      { letter: "A", content: "Implementando monitoreo 24/7 y redundancia de servidores" },
      { letter: "R", content: "Para garantizar el acceso continuo a la plataforma educativa" },
      { letter: "T", content: "Mensualmente" }
    ]
  },
  {
    id: 6,
    title: "Tiempo de Respuesta del Sistema",
    description: "Reducir el tiempo de respuesta promedio del sistema de 300ms a 200ms en los próximos 2 meses, optimizando consultas de base de datos y implementando caché para mejorar la experiencia del usuario.",
    currentValue: "245ms",
    trend: "+8.3%",
    trendColor: "warning",
    trendIcon: trendingUpOutline,
    icon: speedometerOutline,
    smart: [
      { letter: "S", content: "Reducir el tiempo de respuesta promedio del sistema de 300ms a 200ms" },
      { letter: "M", content: "De 300ms a 200ms (reducción del 33%)" },
      { letter: "A", content: "Optimizando consultas de base de datos e implementando caché" },
      { letter: "R", content: "Para mejorar la experiencia del usuario" },
      { letter: "T", content: "En los próximos 2 meses" }
    ]
  },
  {
    id: 7,
    title: "Tasa de Errores del Sistema",
    description: "Mantener la tasa de errores del sistema por debajo del 0.5% mensualmente, implementando testing automatizado y monitoreo proactivo para asegurar la estabilidad de la plataforma EasyFCT.",
    currentValue: "0.8%",
    trend: "+15.2%",
    trendColor: "danger",
    trendIcon: trendingUpOutline,
    icon: bugOutline,
    smart: [
      { letter: "S", content: "Mantener la tasa de errores del sistema por debajo del 0.5%" },
      { letter: "M", content: "Por debajo del 0.5% mensualmente (actualmente 0.8%)" },
      { letter: "A", content: "Implementando testing automatizado y monitoreo proactivo" },
      { letter: "R", content: "Para asegurar la estabilidad de la plataforma EasyFCT" },
      { letter: "T", content: "Mensualmente" }
    ]
  },
  {
    id: 8,
    title: "Tiempo de Resolución de Soporte",
    description: "Reducir el tiempo promedio de resolución de tickets de soporte de 6 horas a 4 horas en los próximos 30 días, implementando un sistema de priorización automática y ampliando el equipo de soporte técnico.",
    currentValue: "4.2h",
    trend: "-18.5%",
    trendColor: "success",
    trendIcon: trendingDownOutline,
    icon: chatbubbleEllipsesOutline,
    smart: [
      { letter: "S", content: "Reducir el tiempo promedio de resolución de tickets de soporte de 6h a 4h" },
      { letter: "M", content: "De 6 horas a 4 horas (reducción del 33%)" },
      { letter: "A", content: "Implementando sistema de priorización automática y ampliando equipo" },
      { letter: "R", content: "Para mejorar el soporte a usuarios de la plataforma" },
      { letter: "T", content: "En los próximos 30 días" }
    ]
  }
]);
</script>

<style scoped>
ion-page {
  --background: #ffffff !important;
}

.dashboard-content {
  --background: #ffffff !important;
}

ion-toolbar {
  --background: #2563eb;
  --color: white;
}

.title-container {
  display: flex;
  align-items: center;
  gap: 8px;
  font-weight: 600;
}

.dashboard-container {
  padding: 16px;
  max-width: 1200px;
  margin: 0 auto;
  background: #ffffff;
}

.section-wrapper {
  margin-bottom: 32px;
}

.section-header {
  margin-bottom: 16px;
}

.section-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 600;
  color: white;
}

.section-badge.business {
  background: linear-gradient(135deg, #2563eb 0%, #3b82f6 100%);
}

.section-badge.technical {
  background: linear-gradient(135deg, #1e40af 0%, #2563eb 100%);
}

.kpis-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 16px;
}

.kpi-card {
  background: white;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  overflow: hidden;
  transition: all 0.2s ease;
  cursor: pointer;
}

.kpi-card:hover {
  border-color: #2563eb;
  box-shadow: 0 4px 12px rgba(37, 99, 235, 0.15);
  transform: translateY(-2px);
}

.kpi-header {
  display: flex;
  align-items: center;
  padding: 16px;
  gap: 12px;
}

.kpi-icon-wrapper {
  width: 40px;
  height: 40px;
  background: #2563eb;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.kpi-icon {
  font-size: 20px;
  color: white;
}

.kpi-info {
  flex: 1;
  min-width: 0;
}

.kpi-title {
  font-size: 0.95rem;
  font-weight: 600;
  color: #374151;
  margin: 0 0 4px 0;
  line-height: 1.3;
}

.kpi-value {
  font-size: 1.25rem;
  font-weight: 700;
  color: #2563eb;
}

.kpi-trend {
  flex-shrink: 0;
}

.trend-value {
  display: flex;
  align-items: center;
  gap: 4px;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: 600;
}

.trend-value.success {
  background: #dcfce7;
  color: #166534;
}

.trend-value.warning {
  background: #fef3c7;
  color: #92400e;
}

.trend-value.danger {
  background: #fee2e2;
  color: #991b1b;
}

.kpi-details {
  border-top: 1px solid #f3f4f6;
  padding: 16px;
  background: #f9fafb;
}

.kpi-description {
  font-size: 0.85rem;
  line-height: 1.5;
  color: #6b7280;
  margin: 0 0 16px 0;
}

.smart-objectives {
  display: grid;
  gap: 8px;
}

.smart-item {
  display: flex;
  align-items: flex-start;
  gap: 8px;
  font-size: 0.8rem;
  line-height: 1.4;
}

.smart-letter {
  width: 20px;
  height: 20px;
  background: #2563eb;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 0.7rem;
  flex-shrink: 0;
}

.smart-text {
  color: #4b5563;
  flex: 1;
}

@media (max-width: 768px) {
  .dashboard-container {
    padding: 12px;
  }
  
  .kpis-grid {
    grid-template-columns: 1fr;
    gap: 12px;
  }
  
  .kpi-header {
    padding: 12px;
  }
  
  .kpi-details {
    padding: 12px;
  }
}
</style>
