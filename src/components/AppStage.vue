<template>
  <div class="visualizer-container">
    <h1 class="title">◢ ONE WAX WING VISUALIZER ◣</h1>
    
    <div class="canvas-wrapper">
      <canvas ref="canvas"></canvas>
      <div class="scanlines"></div>
    </div>

    <div class="controls">
      <div class="file-input-wrapper">
        <span class="file-input-label">
          {{ fileName || 'Select Audio File' }}
        </span>
        <input 
          type="file" 
          accept="audio/*" 
          @change="handleFileSelect"
        />
      </div>

      <div class="button-group">
        <button @click="togglePlay" :disabled="!audioLoaded">
          {{ isPlaying ? '⏸ Pause' : '▶ Play' }}
        </button>
        <button 
          @click="changeVisualizationMode" 
          :disabled="!audioLoaded"
          class="active"
        >
          Mode: {{ visualizationMode }}
        </button>
      </div>

      <div class="status" v-if="status">{{ status }}</div>
    </div>

    <audio ref="audio" style="display: none;"></audio>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

// Refs
const canvas = ref(null);
const audio = ref(null);
const isPlaying = ref(false);
const audioLoaded = ref(false);
const fileName = ref('');
const status = ref('');
const visualizationMode = ref('Particles');

// Non-reactive state
let ctx = null;
let audioContext = null;
let analyser = null;
let source = null;
let dataArray = null;
let bufferLength = null;
let animationId = null;
let startTime = Date.now();

const modes = ['Particles', 'Fluid', 'Spirograph', 'Kaleidoscope'];
let currentModeIndex = 0;

// Methods
const initCanvas = () => {
  if (!canvas.value) return;
  
  ctx = canvas.value.getContext('2d');
  const resizeCanvas = () => {
    const wrapper = canvas.value.parentElement;
    canvas.value.width = wrapper.offsetWidth;
    canvas.value.height = wrapper.offsetHeight;
  };
  
  resizeCanvas();
  window.addEventListener('resize', resizeCanvas);
};

const setupAudioContext = () => {
  audioContext = new (window.AudioContext || window.webkitAudioContext)();
  analyser = audioContext.createAnalyser();
  analyser.fftSize = 512;
  
  source = audioContext.createMediaElementSource(audio.value);
  source.connect(analyser);
  analyser.connect(audioContext.destination);
  
  bufferLength = analyser.frequencyBinCount;
  dataArray = new Uint8Array(bufferLength);
};

const handleFileSelect = (event) => {
  const file = event.target.files[0];
  if (!file) return;

  fileName.value = file.name;
  const url = URL.createObjectURL(file);
  audio.value.src = url;
  audio.value.load();
  audioLoaded.value = true;
  status.value = 'Audio loaded - Ready to play!';
  
  if (!audioContext) {
    setupAudioContext();
  }
};

const togglePlay = () => {
  if (isPlaying.value) {
    audio.value.pause();
    isPlaying.value = false;
    status.value = 'Paused';
    cancelAnimationFrame(animationId);
  } else {
    audio.value.play();
    isPlaying.value = true;
    status.value = 'Playing...';
    startTime = Date.now();
    visualize();
  }
};

const changeVisualizationMode = () => {
  currentModeIndex = (currentModeIndex + 1) % modes.length;
  visualizationMode.value = modes[currentModeIndex];
};

const visualize = () => {
  animationId = requestAnimationFrame(visualize);
  
  if (!analyser || !ctx) return;
  
  analyser.getByteFrequencyData(dataArray);
  
  const width = canvas.value.width;
  const height = canvas.value.height;
  
  // Subtle fade instead of complete clear
  ctx.fillStyle = 'rgba(0, 0, 0, 0.15)';
  ctx.fillRect(0, 0, width, height);
  
  switch(visualizationMode.value) {
    case 'Particles':
      drawParticles(width, height);
      break;
    case 'Fluid':
      drawFluid(width, height);
      break;
    case 'Spirograph':
      drawSpirograph(width, height);
      break;
    case 'Kaleidoscope':
      drawKaleidoscope(width, height);
      break;
  }
};

const drawParticles = (width, height) => {
  const time = (Date.now() - startTime) * 0.001;
  const centerY = height / 2;
  
  for (let i = 0; i < bufferLength; i += 2) {
    const frequency = dataArray[i] / 255;
    const x = (i / bufferLength) * width;
    const baseRadius = frequency * height * 0.4;
    
    // Create flowing particle trails
    for (let j = 0; j < 4; j++) {
      const offset = (time * 0.5 + i * 0.05 + j * 0.25) % 1;
      const angle = time + i * 0.1 + j * Math.PI * 0.5;
      const y = centerY + Math.sin(angle) * (baseRadius * 0.6);
      const size = baseRadius * (1 - offset) * 0.2;
      
      const hue = (i / bufferLength * 360 + time * 30 + j * 90) % 360;
      const gradient = ctx.createRadialGradient(x, y, 0, x, y, size * 2);
      gradient.addColorStop(0, `hsla(${hue}, 100%, 70%, ${0.8 * (1 - offset)})`);
      gradient.addColorStop(0.5, `hsla(${hue}, 100%, 50%, ${0.4 * (1 - offset)})`);
      gradient.addColorStop(1, `hsla(${hue}, 100%, 30%, 0)`);
      
      ctx.fillStyle = gradient;
      ctx.beginPath();
      ctx.arc(x, y, size * 2, 0, Math.PI * 2);
      ctx.fill();
    }
  }
};

const drawFluid = (width, height) => {
  const time = (Date.now() - startTime) * 0.001;
  const centerX = width / 2;
  const centerY = height / 2;
  
  ctx.globalCompositeOperation = 'lighter';
  
  for (let i = 0; i < bufferLength; i += 3) {
    const frequency = dataArray[i] / 255;
    const angle = (i / bufferLength) * Math.PI * 2;
    const radius = 50 + frequency * Math.min(width, height) * 0.35;
    
    const x = centerX + Math.cos(angle + time * 0.5) * radius;
    const y = centerY + Math.sin(angle + time * 0.5) * radius;
    
    // Create flowing blobs
    const blobSize = frequency * 40 + 10;
    const hue = (i / bufferLength * 360 + time * 20) % 360;
    
    const gradient = ctx.createRadialGradient(x, y, 0, x, y, blobSize);
    gradient.addColorStop(0, `hsla(${hue}, 100%, 60%, 0.6)`);
    gradient.addColorStop(0.5, `hsla(${(hue + 60) % 360}, 100%, 50%, 0.3)`);
    gradient.addColorStop(1, `hsla(${(hue + 120) % 360}, 100%, 40%, 0)`);
    
    ctx.fillStyle = gradient;
    ctx.beginPath();
    
    // Create organic shapes
    for (let j = 0; j < 6; j++) {
      const a = (j / 6) * Math.PI * 2;
      const r = blobSize * (0.8 + Math.sin(time * 2 + j + i * 0.1) * 0.2);
      const px = x + Math.cos(a) * r;
      const py = y + Math.sin(a) * r;
      
      if (j === 0) {
        ctx.moveTo(px, py);
      } else {
        ctx.lineTo(px, py);
      }
    }
    ctx.closePath();
    ctx.fill();
  }
  
  ctx.globalCompositeOperation = 'source-over';
};

const drawSpirograph = (width, height) => {
  const time = (Date.now() - startTime) * 0.001;
  const centerX = width / 2;
  const centerY = height / 2;
  const maxRadius = Math.min(width, height) * 0.4;
  
  ctx.globalCompositeOperation = 'lighter';
  
  // Create multiple spirograph patterns
  for (let layer = 0; layer < 3; layer++) {
    ctx.beginPath();
    
    for (let i = 0; i < bufferLength; i++) {
      const frequency = dataArray[i] / 255;
      const angle = (i / bufferLength) * Math.PI * 2 * 5;
      
      const r1 = maxRadius * 0.5 + layer * 20;
      const r2 = r1 * 0.5 * (1 + frequency);
      const r3 = r2 * 0.3;
      
      const x = centerX + 
        Math.cos(angle + time * (0.5 + layer * 0.2)) * r1 +
        Math.cos(angle * -3 + time) * r2 +
        Math.cos(angle * 7 + time * 2) * r3;
      const y = centerY + 
        Math.sin(angle + time * (0.5 + layer * 0.2)) * r1 +
        Math.sin(angle * -3 + time) * r2 +
        Math.sin(angle * 7 + time * 2) * r3;
      
      if (i === 0) {
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }
    }
    
    const hue = (time * 30 + layer * 120) % 360;
    ctx.strokeStyle = `hsla(${hue}, 100%, 60%, 0.5)`;
    ctx.lineWidth = 2;
    ctx.shadowBlur = 15;
    ctx.shadowColor = `hsla(${hue}, 100%, 50%, 0.8)`;
    ctx.stroke();
  }
  
  ctx.globalCompositeOperation = 'source-over';
  ctx.shadowBlur = 0;
};

const drawKaleidoscope = (width, height) => {
  const time = (Date.now() - startTime) * 0.001;
  const centerX = width / 2;
  const centerY = height / 2;
  const segments = 8;
  
  ctx.save();
  ctx.translate(centerX, centerY);
  
  for (let seg = 0; seg < segments; seg++) {
    ctx.save();
    ctx.rotate((Math.PI * 2 / segments) * seg);
    
    if (seg % 2 === 0) {
      ctx.scale(1, -1);
    }
    
    ctx.globalCompositeOperation = 'lighter';
    
    for (let i = 0; i < bufferLength; i += 4) {
      const frequency = dataArray[i] / 255;
      const distance = (i / bufferLength) * Math.min(width, height) * 0.4;
      const angle = time + i * 0.1;
      
      const x = Math.cos(angle) * distance;
      const y = Math.sin(angle) * distance * frequency;
      const size = frequency * 15 + 5;
      
      const hue = (i / bufferLength * 360 + time * 50 + seg * 45) % 360;
      const gradient = ctx.createRadialGradient(x, y, 0, x, y, size);
      gradient.addColorStop(0, `hsla(${hue}, 100%, 70%, 0.8)`);
      gradient.addColorStop(1, `hsla(${hue}, 100%, 50%, 0)`);
      
      ctx.fillStyle = gradient;
      ctx.beginPath();
      ctx.arc(x, y, size, 0, Math.PI * 2);
      ctx.fill();
    }
    
    ctx.restore();
  }
  
  ctx.restore();
  ctx.globalCompositeOperation = 'source-over';
};

const handleAudioEnded = () => {
  isPlaying.value = false;
  status.value = 'Track ended';
  cancelAnimationFrame(animationId);
};

const cleanup = () => {
  if (animationId) {
    cancelAnimationFrame(animationId);
  }
  if (audioContext) {
    audioContext.close();
  }
};

// Lifecycle Hooks
onMounted(() => {
  initCanvas();
  audio.value.addEventListener('ended', handleAudioEnded);
});

onUnmounted(() => {
  cleanup();
});
</script>

<style scoped>
.visualizer-container {
  position: relative;
  width: 100%;
  max-width: 900px;
  background: rgba(0, 0, 0, 0.6);
  border: 3px solid #ff00ff;
  border-radius: 10px;
  padding: 30px;
  box-shadow: 
    0 0 20px rgba(255, 0, 255, 0.5),
    0 0 40px rgba(0, 255, 255, 0.3),
    inset 0 0 30px rgba(255, 0, 255, 0.1);
}

.title {
  text-align: center;
  color: #00ffff;
  font-size: clamp(24px, 5vw, 42px);
  text-shadow: 
    0 0 10px #00ffff,
    0 0 20px #00ffff,
    0 0 30px #ff00ff;
  margin-bottom: 30px;
  letter-spacing: 3px;
  animation: glow 2s ease-in-out infinite alternate;
}

@keyframes glow {
  from {
    text-shadow: 
      0 0 10px #00ffff,
      0 0 20px #00ffff,
      0 0 30px #ff00ff;
  }
  to {
    text-shadow: 
      0 0 20px #00ffff,
      0 0 30px #00ffff,
      0 0 40px #ff00ff,
      0 0 50px #ff00ff;
  }
}

.canvas-wrapper {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
  background: rgba(0, 0, 0, 0.8);
  border: 2px solid #00ffff;
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 
    inset 0 0 20px rgba(0, 255, 255, 0.2),
    0 0 15px rgba(255, 0, 255, 0.3);
}

canvas {
  display: block;
  width: 100%;
  height: 100%;
}

.controls {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;
}

.file-input-wrapper {
  position: relative;
  display: inline-block;
  overflow: hidden;
}

.file-input-label {
  display: inline-block;
  padding: 15px 40px;
  background: linear-gradient(135deg, #ff00ff, #00ffff);
  color: #000;
  font-weight: bold;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  text-transform: uppercase;
  letter-spacing: 2px;
  box-shadow: 0 0 15px rgba(255, 0, 255, 0.5);
  position: relative;
  z-index: 1;
}

.file-input-label:hover {
  transform: translateY(-2px);
  box-shadow: 0 0 25px rgba(255, 0, 255, 0.8);
}

input[type="file"] {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  cursor: pointer;
  z-index: 2;
}

.button-group {
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
  justify-content: center;
}

button {
  padding: 12px 30px;
  background: rgba(0, 255, 255, 0.2);
  border: 2px solid #00ffff;
  color: #00ffff;
  font-family: 'Courier New', monospace;
  font-size: 14px;
  font-weight: bold;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  text-transform: uppercase;
  letter-spacing: 1px;
  box-shadow: 0 0 10px rgba(0, 255, 255, 0.3);
}

button:hover:not(:disabled) {
  background: rgba(0, 255, 255, 0.4);
  box-shadow: 0 0 20px rgba(0, 255, 255, 0.6);
  transform: translateY(-2px);
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

button.active {
  background: rgba(255, 0, 255, 0.4);
  border-color: #ff00ff;
  color: #ff00ff;
  box-shadow: 0 0 15px rgba(255, 0, 255, 0.6);
}

.status {
  margin-top: 20px;
  color: #00ff00;
  font-size: 14px;
  text-align: center;
  text-shadow: 0 0 5px #00ff00;
}

.scanlines {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: repeating-linear-gradient(
    0deg,
    rgba(0, 0, 0, 0.15),
    rgba(0, 0, 0, 0.15) 1px,
    transparent 1px,
    transparent 2px
  );
  pointer-events: none;
  z-index: 10;
}

@media (max-width: 768px) {
  .visualizer-container {
    padding: 20px;
  }

  .title {
    margin-bottom: 20px;
  }

  .controls {
    margin-top: 20px;
    gap: 15px;
  }

  button, .file-input-label {
    padding: 10px 20px;
    font-size: 12px;
  }
}
</style>
