<template>
  <ion-app>
    <ion-split-pane content-id="main-content">
      <ion-menu content-id="main-content" type="overlay" class="custom-menu">
        <ion-content class="menu-content">
          <div class="menu-header">
            <div class="logo-container">
              <div class="logo-icon">
                <ion-icon :icon="schoolOutline" class="logo"></ion-icon>
              </div>
              <div class="logo-text">
                <h2>EasyFCT</h2>
                <p>Control de Prácticas FP</p>
              </div>
            </div>
          </div>

          <div class="menu-section">
            <div class="section-title">Paneles</div>
            <ion-list class="menu-list">
              <ion-menu-toggle :auto-hide="false" v-for="(p, i) in appPages" :key="i">
                <ion-item 
                  @click="selectedIndex = i" 
                  router-direction="root" 
                  :router-link="p.url" 
                  lines="none" 
                  :detail="false" 
                  class="menu-item"
                  :class="{ 'menu-item-selected': selectedIndex === i }"
                >
                  <div class="menu-item-content">
                    <div class="menu-icon-container">
                      <ion-icon :icon="p.icon" class="menu-icon"></ion-icon>
                    </div>
                    <div class="menu-text">
                      <span class="menu-title">{{ p.title }}</span>
                      <span class="menu-subtitle">{{ p.subtitle }}</span>
                    </div>
                    <div class="menu-indicator" v-if="selectedIndex === i">
                      <div class="indicator-dot"></div>
                    </div>
                  </div>
                </ion-item>
              </ion-menu-toggle>
            </ion-list>
          </div>
        
        </ion-content>
      </ion-menu>
      <ion-router-outlet id="main-content"></ion-router-outlet>
    </ion-split-pane>
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonContent, IonIcon, IonItem, IonList, IonMenu, IonMenuToggle, IonRouterOutlet, IonSplitPane } from '@ionic/vue';
import { 
  businessOutline, 
  settingsOutline, 
  analyticsOutline, 
  schoolOutline,
  personCircleOutline 
} from 'ionicons/icons';
import { ref, onMounted, watch } from 'vue';
import { useRoute } from 'vue-router';

const selectedIndex = ref(0);
const appPages = [
  {
    title: 'Negocio',
    subtitle: 'Métricas de prácticas',
    url: '/dashboard',
    icon: businessOutline,
  },
  {
    title: 'Técnico',
    subtitle: 'Rendimiento del sistema',
    url: '/tecnico',
    icon: settingsOutline,
  },
  {
    title: 'KPIs',
    subtitle: 'Indicadores clave',
    url: '/kpis',
    icon: analyticsOutline,
  },
];

const route = useRoute();

const updateSelectedIndex = () => {
  const currentPath = route.path;
  const index = appPages.findIndex(page => page.url === currentPath);
  if (index !== -1) {
    selectedIndex.value = index;
  }
};

onMounted(() => {
  updateSelectedIndex();
});

watch(() => route.path, updateSelectedIndex);
</script>

<style scoped>
ion-split-pane {
  --side-max-width: 320px;
}

.custom-menu {
  --background: #ffffff;
  border-right: 1px solid #e0e0e0;
}

.menu-content {
  --background: #ffffff;
  --padding-start: 0;
  --padding-end: 0;
  --padding-top: 0;
  --padding-bottom: 0;
}

.menu-header {
  background: linear-gradient(135deg, #ffcc00 0%, #ffd633 100%);
  padding: 24px 20px;
  border-bottom: 1px solid #e0e0e0;
}

.logo-container {
  display: flex;
  align-items: center;
  gap: 16px;
}

.logo-icon {
  width: 48px;
  height: 48px;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(10px);
}

.logo {
  font-size: 24px;
  color: #333;
}

.logo-text h2 {
  margin: 0;
  font-size: 20px;
  font-weight: 700;
  color: #333;
  line-height: 1.2;
}

.logo-text p {
  margin: 0;
  font-size: 12px;
  color: #666;
  font-weight: 500;
}

.menu-section {
  padding: 24px 0;
}

.section-title {
  padding: 0 20px 16px;
  font-size: 12px;
  font-weight: 600;
  color: #999;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.menu-list {
  padding: 0;
  background: transparent;
}

.menu-item {
  --background: transparent;
  --background-hover: rgba(255, 204, 0, 0.08);
  --background-activated: rgba(255, 204, 0, 0.12);
  --color: #333;
  --padding-start: 20px;
  --padding-end: 20px;
  --padding-top: 0;
  --padding-bottom: 0;
  --min-height: 64px;
  margin: 4px 16px;
  border-radius: 12px;
  transition: all 0.3s ease;
  cursor: pointer;
}

.menu-item:hover {
  --background: rgba(255, 204, 0, 0.08);
  transform: translateX(4px);
}

.menu-item-selected {
  --background: rgba(255, 204, 0, 0.12);
  --color: #333;
  box-shadow: 0 2px 8px rgba(255, 204, 0, 0.2);
}

.menu-item-content {
  display: flex;
  align-items: center;
  width: 100%;
  gap: 16px;
}

.menu-icon-container {
  width: 40px;
  height: 40px;
  background-color: rgba(255, 204, 0, 0.1);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.menu-item-selected .menu-icon-container {
  background-color: #ffcc00;
}

.menu-icon {
  font-size: 20px;
  color: #666;
  transition: all 0.3s ease;
}

.menu-item-selected .menu-icon {
  color: #333;
}

.menu-text {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.menu-title {
  font-size: 15px;
  font-weight: 600;
  color: #333;
  line-height: 1.2;
}

.menu-subtitle {
  font-size: 12px;
  color: #999;
  line-height: 1.2;
}

.menu-indicator {
  display: flex;
  align-items: center;
}

.indicator-dot {
  width: 8px;
  height: 8px;
  background-color: #ffcc00;
  border-radius: 50%;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(255, 204, 0, 0.7);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(255, 204, 0, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(255, 204, 0, 0);
  }
}

@media (max-width: 768px) {
  ion-split-pane {
    --side-max-width: 280px;
  }
  
  .menu-header {
    padding: 20px 16px;
  }
  
  .menu-item {
    margin: 4px 12px;
    --padding-start: 16px;
    --padding-end: 16px;
  }
  
  .menu-footer {
    padding: 12px 16px;
  }
}

.menu-item {
  animation: slideInLeft 0.3s ease forwards;
}

.menu-item:nth-child(1) { animation-delay: 0.1s; }
.menu-item:nth-child(2) { animation-delay: 0.2s; }
.menu-item:nth-child(3) { animation-delay: 0.3s; }

@keyframes slideInLeft {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.menu-item:hover .menu-icon-container {
  transform: scale(1.05);
  background-color: rgba(255, 204, 0, 0.2);
}

.menu-item:hover .menu-icon {
  color: #ffcc00;
}

.menu-item:active {
  transform: translateX(2px) scale(0.98);
}

.menu-item-selected {
  position: relative;
}

.menu-item-selected::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 4px;
  height: 24px;
  background-color: #ffcc00;
  border-radius: 0 4px 4px 0;
}
</style>
