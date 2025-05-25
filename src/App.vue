<template>
  <ion-app>
    <ion-split-pane content-id="main-content">
      <ion-menu content-id="main-content" type="overlay" class="custom-menu">
        <ion-content class="menu-content">
          <ion-list id="inbox-list">
            <ion-list-header>EasyFCT Dashboard</ion-list-header>
            <ion-note>Control de Prácticas FP</ion-note>

            <div class="menu-section">
              <div class="section-title">PANELES</div>
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
            </div>
          </ion-list>
        </ion-content>
      </ion-menu>
      <ion-router-outlet id="main-content"></ion-router-outlet>
    </ion-split-pane>
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonContent, IonIcon, IonItem, IonLabel, IonList, IonListHeader, IonMenu, IonMenuToggle, IonNote, IonRouterOutlet, IonSplitPane } from '@ionic/vue';
import { 
  businessOutline, 
  settingsOutline, 
  analyticsOutline
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

onMounted(updateSelectedIndex);
watch(route, updateSelectedIndex);
</script>

<style scoped>
ion-split-pane {
  --side-max-width: 280px;
}

.custom-menu {
  --background: #ffffff;
}

.menu-content {
  --background: var(--ion-item-background, var(--ion-background-color, #fff));
}

ion-menu.md ion-content {
  --padding-start: 8px;
  --padding-end: 8px;
  --padding-top: 20px;
  --padding-bottom: 20px;
}

ion-menu.md ion-list {
  padding: 20px 0;
}

ion-menu.md ion-note {
  margin-bottom: 30px;
}

ion-menu.md ion-list-header,
ion-menu.md ion-note {
  padding-left: 10px;
}

ion-menu.md ion-list#inbox-list {
  border-bottom: 1px solid var(--ion-background-color-step-150, #d7d8da);
}

ion-menu.md ion-list#inbox-list ion-list-header {
  font-size: 22px;
  font-weight: 600;
  min-height: 20px;
}

ion-note {
  display: inline-block;
  font-size: 16px;
  color: var(--ion-color-medium-shade);
}

.menu-section {
  padding: 20px 0 0 0;
}

.section-title {
  padding: 0 10px 16px;
  font-size: 12px;
  font-weight: 600;
  color: #999;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.menu-item {
  --background: transparent;
  --background-hover: rgba(255, 204, 0, 0.08);
  --background-activated: rgba(255, 204, 0, 0.12);
  --color: #333;
  --padding-start: 10px;
  --padding-end: 10px;
  --padding-top: 0;
  --padding-bottom: 0;
  --min-height: 64px;
  margin: 4px 8px;
  border-radius: 8px;
  transition: all 0.3s ease;
  cursor: pointer;
  position: relative;
}

.menu-item:hover {
  --background: rgba(255, 204, 0, 0.08);
}

.menu-item-selected {
  --background: rgba(255, 204, 0, 0.12);
  --color: #333;
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

.menu-item-content {
  display: flex;
  align-items: center;
  width: 100%;
  gap: 12px;
}

.menu-icon-container {
  width: 36px;
  height: 36px;
  background-color: rgba(255, 204, 0, 0.1);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.menu-item-selected .menu-icon-container {
  background-color: #ffcc00;
}

.menu-icon {
  font-size: 18px;
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
}

ion-menu.ios ion-content {
  --padding-bottom: 20px;
}

ion-menu.ios ion-list {
  padding: 20px 0 0 0;
}

ion-menu.ios ion-note {
  line-height: 24px;
  margin-bottom: 20px;
}

ion-menu.ios ion-item {
  --padding-start: 16px;
  --padding-end: 16px;
  --min-height: 50px;
}

ion-menu.ios ion-item.selected ion-icon {
  color: var(--ion-color-primary);
}

ion-menu.ios ion-item ion-icon {
  font-size: 24px;
  color: #73849a;
}

ion-menu.ios ion-list-header,
ion-menu.ios ion-note {
  padding-left: 16px;
  padding-right: 16px;
}

ion-menu.ios ion-note {
  margin-bottom: 8px;
}

.menu-item:hover .menu-icon-container {
  background-color: rgba(255, 204, 0, 0.2);
}

.menu-item:hover .menu-icon {
  color: #ffcc00;
}
</style>