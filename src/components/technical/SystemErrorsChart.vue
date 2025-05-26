<template>
  <ion-card class="chart-card white-card">
    <ion-card-header>
      <ion-card-title>Errores del Sistema de Prácticas</ion-card-title>
      <ion-card-subtitle>Tasa de errores por módulo</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content class="white-content">
      <div class="canvas-container">
        <canvas ref="chartCanvas" width="800" height="400"></canvas>
        <div v-if="hoveredModule" class="tooltip" :style="tooltipStyle">
          <div class="tooltip-title">{{ hoveredModule.module }}</div>
          <div class="tooltip-value">{{ hoveredModule.errorRate.toFixed(2) }}% errores</div>
        </div>
      </div>
    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent } from '@ionic/vue';

const chartCanvas = ref(null);
const hoveredModule = ref(null);
const tooltipStyle = ref({
  left: '0px',
  top: '0px'
});

// Datos simulados de errores por módulo del sistema EasyFCT
const errorData = [
  { module: 'Gestión Estudiantes', errorRate: 0.8 },
  { module: 'Portal Empresas', errorRate: 1.2 },
  { module: 'Asignación Prácticas', errorRate: 0.5 },
  { module: 'Seguimiento', errorRate: 0.9 },
  { module: 'Evaluaciones', errorRate: 0.3 },
  { module: 'Comunicaciones', errorRate: 1.5 },
  { module: 'Reportes', errorRate: 0.7 },
  { module: 'Autenticación', errorRate: 0.2 }
];

let pointCoordinates = [];

const drawChart = () => {
  const canvas = chartCanvas.value;
  if (!canvas) return;
  
  const ctx = canvas.getContext('2d');
  const width = canvas.width;
  const height = canvas.height;
  
  ctx.clearRect(0, 0, width, height);
  
  const padding = { top: 20, right: 30, bottom: 60, left: 80 };
  const chartWidth = width - padding.left - padding.right;
  const chartHeight = height - padding.top - padding.bottom;
  
  const maxErrorRate = Math.max(...errorData.map(d => d.errorRate)) * 1.2;
  
  const scaleX = (index) => padding.left + (index / (errorData.length - 1)) * chartWidth;
  const scaleY = (value) => height - padding.bottom - (value / maxErrorRate) * chartHeight;
  
  // Dibujar ejes
  ctx.beginPath();
  ctx.moveTo(padding.left, height - padding.bottom);
  ctx.lineTo(width - padding.right, height - padding.bottom);
  ctx.strokeStyle = '#ddd';
  ctx.lineWidth = 1;
  ctx.stroke();
  
  ctx.beginPath();
  ctx.moveTo(padding.left, padding.top);
  ctx.lineTo(padding.left, height - padding.bottom);
  ctx.strokeStyle = '#ddd';
  ctx.lineWidth = 1;
  ctx.stroke();
  
  // Líneas de cuadrícula
  const yTicks = 5;
  for (let i = 0; i <= yTicks; i++) {
    const y = padding.top + (chartHeight / yTicks) * i;
    const value = maxErrorRate - (i / yTicks) * maxErrorRate;
    
    ctx.beginPath();
    ctx.moveTo(padding.left, y);
    ctx.lineTo(width - padding.right, y);
    ctx.strokeStyle = 'rgba(200, 200, 200, 0.3)';
    ctx.lineWidth = 1;
    ctx.stroke();
    
    ctx.fillStyle = '#666';
    ctx.font = '12px Arial';
    ctx.textAlign = 'right';
    ctx.textBaseline = 'middle';
    ctx.fillText(value.toFixed(1) + '%', padding.left - 10, y);
  }
  
  // Etiquetas del eje X
  errorData.forEach((data, index) => {
    const x = scaleX(index);
    const y = height - padding.bottom;
    
    ctx.save();
    ctx.translate(x, y + 15);
    ctx.rotate(-Math.PI / 4);
    ctx.fillStyle = '#666';
    ctx.font = '11px Arial';
    ctx.textAlign = 'right';
    ctx.textBaseline = 'middle';
    ctx.fillText(data.module, 0, 0);
    ctx.restore();
  });
  
  // Área bajo la curva
  ctx.beginPath();
  ctx.moveTo(scaleX(0), height - padding.bottom);
  
  pointCoordinates = [];
  
  errorData.forEach((data, index) => {
    const x = scaleX(index);
    const y = scaleY(data.errorRate);
    
    if (index === 0) {
      ctx.moveTo(x, y);
    } else {
      ctx.lineTo(x, y);
    }
    
    pointCoordinates.push({ x, y, data });
  });
  
  ctx.lineTo(scaleX(errorData.length - 1), height - padding.bottom);
  ctx.closePath();
  
  const gradient = ctx.createLinearGradient(0, padding.top, 0, height - padding.bottom);
  gradient.addColorStop(0, 'rgba(255, 107, 107, 0.7)');
  gradient.addColorStop(1, 'rgba(255, 107, 107, 0.1)');
  ctx.fillStyle = gradient;
  ctx.fill();
  
  // Línea principal
  ctx.beginPath();
  errorData.forEach((data, index) => {
    const x = scaleX(index);
    const y = scaleY(data.errorRate);
    
    if (index === 0) {
      ctx.moveTo(x, y);
    } else {
      ctx.lineTo(x, y);
    }
  });
  ctx.strokeStyle = '#ff6b6b';
  ctx.lineWidth = 3;
  ctx.stroke();
  
  // Puntos
  errorData.forEach((data, index) => {
    const x = scaleX(index);
    const y = scaleY(data.errorRate);
    
    ctx.beginPath();
    ctx.arc(x, y, 6, 0, Math.PI * 2);
    ctx.fillStyle = '#ff6b6b';
    ctx.fill();
    ctx.strokeStyle = '#fff';
    ctx.lineWidth = 2;
    ctx.stroke();
  });
  
  // Títulos de ejes
  ctx.save();
  ctx.translate(padding.left - 50, height / 2);
  ctx.rotate(-Math.PI / 2);
  ctx.fillStyle = '#666';
  ctx.font = '14px Arial';
  ctx.textAlign = 'center';
  ctx.fillText('Tasa de Errores (%)', 0, 0);
  ctx.restore();
  
  ctx.fillStyle = '#666';
  ctx.font = '14px Arial';
  ctx.textAlign = 'center';
  ctx.fillText('Módulos del Sistema', width / 2, height - 5);
};

const handleMouseMove = (event) => {
  const canvas = chartCanvas.value;
  if (!canvas) return;
  
  const rect = canvas.getBoundingClientRect();
  const mouseX = event.clientX - rect.left;
  const mouseY = event.clientY - rect.top;
  
  let found = false;
  for (const point of pointCoordinates) {
    const distance = Math.sqrt(Math.pow(mouseX - point.x, 2) + Math.pow(mouseY - point.y, 2));
    if (distance < 20) {
      hoveredModule.value = point.data;
      tooltipStyle.value = {
        left: `${point.x + 10}px`,
        top: `${point.y - 40}px`
      };
      found = true;
      break;
    }
  }
  
  if (!found) {
    hoveredModule.value = null;
  }
};

const handleResize = () => {
  const canvas = chartCanvas.value;
  if (!canvas) return;
  
  const container = canvas.parentElement;
  const ratio = window.devicePixelRatio || 1;
  
  canvas.width = container.clientWidth * ratio;
  canvas.height = container.clientHeight * ratio;
  
  const ctx = canvas.getContext('2d');
  ctx.scale(ratio, ratio);
  
  canvas.style.width = container.clientWidth + 'px';
  canvas.style.height = container.clientHeight + 'px';
  
  drawChart();
};

onMounted(() => {
  const canvas = chartCanvas.value;
  if (canvas) {
    canvas.addEventListener('mousemove', handleMouseMove);
    
    const resizeObserver = new ResizeObserver(handleResize);
    resizeObserver.observe(canvas.parentElement);
    
    handleResize();
    
    onUnmounted(() => {
      canvas.removeEventListener('mousemove', handleMouseMove);
      resizeObserver.disconnect();
    });
  }
});
</script>

<style scoped>
.chart-card {
  height: 100%;
  border-top: 3px solid #ff6b6b;
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

.canvas-container {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #ffffff !important;
}

canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.tooltip {
  position: absolute;
  background-color: rgba(255, 255, 255, 0.95);
  border: 1px solid #ff6b6b;
  border-radius: 8px;
  padding: 8px 12px;
  pointer-events: none;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  z-index: 10;
  min-width: 120px;
  transition: all 0.1s ease;
}

.tooltip-title {
  font-weight: bold;
  margin-bottom: 4px;
  color: #333;
}

.tooltip-value {
  color: #666;
}
</style>
