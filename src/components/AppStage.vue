<template>
  <div class="visualizer-app">
    <div class="controls">
      <label class="file-input-label">
        Choose Audio
        <input type="file" @change="loadAudio" accept="audio/*">
      </label>
      <button class="pattern-btn" @click="cyclePattern">Pattern: {{ currentPatternName }}</button>
      <button class="palette-btn" @click="cyclePalette">Palette: {{ currentPaletteName }}</button>
      <button class="pattern-btn" @click="toggleAutoRotate">
        Auto-Rotate: {{ autoRotate ? 'ON' : 'OFF' }}
      </button>
    </div>

    <div class="visualizer-container">
      <canvas ref="canvas"></canvas>
    </div>

    <div class="info" v-if="audioLoaded">
      {{ fileName }}
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const canvas = ref(null);
const audioLoaded = ref(false);
const fileName = ref('');
const currentPatternIndex = ref(0);
const currentPaletteIndex = ref(0);
const autoRotate = ref(true);

let ctx, audioContext, analyser, dataArray, bufferLength;
let animationId;
let rotationAngle = 0;
let breathePhase = 0;
let lastRotateTime = Date.now();

// Color Palettes
const palettes = [
  {
    name: 'Neon',
    colors: ['#ff006e', '#8338ec', '#3a86ff', '#06ffa5', '#ffbe0b']
  },
  {
    name: 'Cosmic',
    colors: ['#4361ee', '#7209b7', '#f72585', '#b5179e', '#560bad']
  },
  {
    name: 'Sunset',
    colors: ['#ff0054', '#ff5400', '#ffbd00', '#ff006e', '#fb5607']
  },
  {
    name: 'Ocean',
    colors: ['#06ffa5', '#00d9ff', '#0077b6', '#023e8a', '#4cc9f0']
  },
  {
    name: 'Fire',
    colors: ['#ff006e', '#ff4800', '#ffa600', '#ffdd00', '#ff0054']
  },
  {
    name: 'Arctic',
    colors: ['#00f5ff', '#00d9ff', '#b8f2e6', '#ffa69e', '#ff006e']
  },
  {
    name: 'Gold',
    colors: ['#ffd60a', '#ffc300', '#ff9e00', '#ff6700', '#ff0054']
  }
];

// Pattern Definitions
const patterns = [
  { name: 'Cubic Grid', draw: drawCubicGrid },
  { name: 'Diamond Lattice', draw: drawDiamondLattice },
  { name: 'Hex Flowers', draw: drawHexFlowers },
  { name: 'Concentric Waves', draw: drawConcentricWaves },
  { name: 'Spiral Galaxy', draw: drawSpiralGalaxy }
];

const currentPatternName = ref(patterns[0].name);
const currentPaletteName = ref(palettes[0].name);

function loadAudio(event) {
  const file = event.target.files[0];
  if (!file) return;

  fileName.value = file.name;
  const audio = new Audio();
  const reader = new FileReader();

  reader.onload = (e) => {
    audio.src = e.target.result;
    audio.play();
    setupAudioContext(audio);
    audioLoaded.value = true;
  };

  reader.readAsDataURL(file);
}

function setupAudioContext(audio) {
  audioContext = new (window.AudioContext || window.webkitAudioContext)();
  analyser = audioContext.createAnalyser();
  analyser.fftSize = 512;
  
  const source = audioContext.createMediaElementSource(audio);
  source.connect(analyser);
  analyser.connect(audioContext.destination);

  bufferLength = analyser.frequencyBinCount;
  dataArray = new Uint8Array(bufferLength);
}

function getColor(index, total, intensity) {
  const palette = palettes[currentPaletteIndex.value].colors;
  const colorIndex = Math.floor((index / total) * palette.length);
  const color = palette[colorIndex % palette.length];
  
  // Convert hex to rgb and apply intensity
  const r = parseInt(color.slice(1, 3), 16);
  const g = parseInt(color.slice(3, 5), 16);
  const b = parseInt(color.slice(5, 7), 16);
  
  return `rgba(${r}, ${g}, ${b}, ${intensity})`;
}

function drawCubicGrid() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const size = 60;
  const cols = Math.ceil(width / size) + 2;
  const rows = Math.ceil(height / size) + 2;

  ctx.save();
  ctx.translate(width / 2, height / 2);
  ctx.rotate(rotationAngle * 0.2);
  ctx.translate(-width / 2, -height / 2);

  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      const x = (i - 1) * size;
      const y = (j - 1) * size;
      const index = i + j * cols;
      const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.1) * 0.3 + 0.5;
      
      const scale = audioLoaded.value ? 0.8 + value * 0.4 : 0.9 + value * 0.1;
      const depth = value * 30;

      ctx.save();
      ctx.translate(x + size / 2, y + size / 2);
      ctx.scale(scale, scale);

      // Draw 3D cube effect
      const cubeSize = size * 0.7;
      
      // Top face
      ctx.fillStyle = getColor(index, cols * rows, value * 0.8);
      ctx.beginPath();
      ctx.moveTo(-cubeSize / 2, -cubeSize / 2 - depth);
      ctx.lineTo(cubeSize / 2, -cubeSize / 2 - depth);
      ctx.lineTo(cubeSize / 2 + depth / 2, -cubeSize / 2 - depth / 2);
      ctx.lineTo(-cubeSize / 2 + depth / 2, -cubeSize / 2 - depth / 2);
      ctx.closePath();
      ctx.fill();

      // Front face
      ctx.fillStyle = getColor(index, cols * rows, value * 0.6);
      ctx.fillRect(-cubeSize / 2, -cubeSize / 2, cubeSize, cubeSize);

      // Side face
      ctx.fillStyle = getColor(index, cols * rows, value * 0.4);
      ctx.beginPath();
      ctx.moveTo(cubeSize / 2, -cubeSize / 2);
      ctx.lineTo(cubeSize / 2 + depth / 2, -cubeSize / 2 - depth / 2);
      ctx.lineTo(cubeSize / 2 + depth / 2, cubeSize / 2 - depth / 2);
      ctx.lineTo(cubeSize / 2, cubeSize / 2);
      ctx.closePath();
      ctx.fill();

      // Border
      ctx.strokeStyle = getColor(index, cols * rows, 1);
      ctx.lineWidth = 2;
      ctx.strokeRect(-cubeSize / 2, -cubeSize / 2, cubeSize, cubeSize);

      ctx.restore();
    }
  }

  ctx.restore();
}

function drawDiamondLattice() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const size = 70;
  const cols = Math.ceil(width / size) + 2;
  const rows = Math.ceil(height / size) + 2;

  ctx.save();
  ctx.translate(width / 2, height / 2);
  ctx.rotate(rotationAngle * 0.15);
  ctx.translate(-width / 2, -height / 2);

  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      const x = (i - 1) * size + size / 2;
      const y = (j - 1) * size + size / 2;
      const index = i + j * cols;
      const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.15) * 0.3 + 0.5;
      
      const octagonSize = size * 0.35 * (0.7 + value * 0.5);
      const diamondSize = size * 0.25 * (0.8 + value * 0.4);

      // Draw octagon
      ctx.fillStyle = getColor(index, cols * rows, value * 0.7);
      ctx.beginPath();
      for (let k = 0; k < 8; k++) {
        const angle = (k / 8) * Math.PI * 2;
        const px = x + Math.cos(angle) * octagonSize;
        const py = y + Math.sin(angle) * octagonSize;
        if (k === 0) ctx.moveTo(px, py);
        else ctx.lineTo(px, py);
      }
      ctx.closePath();
      ctx.fill();
      ctx.strokeStyle = getColor(index, cols * rows, 1);
      ctx.lineWidth = 2;
      ctx.stroke();

      // Draw center diamond
      ctx.fillStyle = getColor(index + 100, cols * rows, value);
      ctx.beginPath();
      ctx.moveTo(x, y - diamondSize);
      ctx.lineTo(x + diamondSize, y);
      ctx.lineTo(x, y + diamondSize);
      ctx.lineTo(x - diamondSize, y);
      ctx.closePath();
      ctx.fill();
      ctx.strokeStyle = getColor(index + 100, cols * rows, 1);
      ctx.lineWidth = 2;
      ctx.stroke();

      // Draw corner diamonds
      const corners = [
        [x, y - octagonSize - diamondSize / 2],
        [x + octagonSize + diamondSize / 2, y],
        [x, y + octagonSize + diamondSize / 2],
        [x - octagonSize - diamondSize / 2, y]
      ];

      corners.forEach(([cx, cy], ci) => {
        const cornerValue = audioLoaded.value ? dataArray[(dataIndex + ci) % bufferLength] / 255 : value;
        const cSize = diamondSize * 0.6 * (0.8 + cornerValue * 0.4);
        
        ctx.fillStyle = getColor(index + ci * 50, cols * rows, cornerValue * 0.8);
        ctx.beginPath();
        ctx.moveTo(cx, cy - cSize);
        ctx.lineTo(cx + cSize, cy);
        ctx.lineTo(cx, cy + cSize);
        ctx.lineTo(cx - cSize, cy);
        ctx.closePath();
        ctx.fill();
      });
    }
  }

  ctx.restore();
}

function drawHexFlowers() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const hexRadius = 40;
  const hexHeight = hexRadius * Math.sqrt(3);
  const cols = Math.ceil(width / (hexRadius * 1.5)) + 2;
  const rows = Math.ceil(height / hexHeight) + 2;

  ctx.save();
  ctx.translate(width / 2, height / 2);
  ctx.rotate(rotationAngle * 0.1);
  ctx.translate(-width / 2, -height / 2);

  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      const x = (i - 1) * hexRadius * 1.5;
      const y = (j - 1) * hexHeight + (i % 2) * hexHeight / 2;
      const index = i + j * cols;
      const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.12) * 0.3 + 0.5;
      
      // Draw flower pattern with hexagons
      const pattern = [
        { angle: 0, dist: 0, scale: 1.2 },
        { angle: 0, dist: hexRadius * 0.8, scale: 0.5 },
        { angle: 60, dist: hexRadius * 0.8, scale: 0.5 },
        { angle: 120, dist: hexRadius * 0.8, scale: 0.5 },
        { angle: 180, dist: hexRadius * 0.8, scale: 0.5 },
        { angle: 240, dist: hexRadius * 0.8, scale: 0.5 },
        { angle: 300, dist: hexRadius * 0.8, scale: 0.5 }
      ];

      pattern.forEach((p, pi) => {
        const angle = (p.angle * Math.PI / 180) + rotationAngle;
        const hx = x + Math.cos(angle) * p.dist;
        const hy = y + Math.sin(angle) * p.dist;
        const size = hexRadius * p.scale * (0.7 + value * 0.5);
        const intensity = pi === 0 ? value : value * 0.6;

        // Draw hexagon
        ctx.fillStyle = getColor(index + pi * 30, cols * rows, intensity);
        ctx.beginPath();
        for (let k = 0; k < 6; k++) {
          const hexAngle = (k / 6) * Math.PI * 2;
          const px = hx + Math.cos(hexAngle) * size;
          const py = hy + Math.sin(hexAngle) * size;
          if (k === 0) ctx.moveTo(px, py);
          else ctx.lineTo(px, py);
        }
        ctx.closePath();
        ctx.fill();
        ctx.strokeStyle = getColor(index + pi * 30, cols * rows, 1);
        ctx.lineWidth = pi === 0 ? 2 : 1;
        ctx.stroke();
      });
    }
  }

  ctx.restore();
}

function drawConcentricWaves() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const size = 90;
  const cols = Math.ceil(width / size) + 2;
  const rows = Math.ceil(height / size) + 2;

  ctx.save();
  ctx.translate(width / 2, height / 2);
  ctx.rotate(rotationAngle * 0.08);
  ctx.translate(-width / 2, -height / 2);

  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      const x = (i - 1) * size + size / 2;
      const y = (j - 1) * size + size / 2;
      const index = i + j * cols;
      const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.1) * 0.3 + 0.5;
      
      const rings = 8;
      const maxRadius = size * 0.45;

      for (let r = 0; r < rings; r++) {
        const radius = (maxRadius / rings) * (r + 1) * (0.8 + value * 0.4);
        const ringValue = audioLoaded.value ? dataArray[(dataIndex + r * 10) % bufferLength] / 255 : value;
        const thickness = (maxRadius / rings) * 0.7;

        // Hexagonal ring
        ctx.strokeStyle = getColor(index + r * 20, cols * rows, ringValue);
        ctx.lineWidth = thickness;
        ctx.beginPath();
        for (let k = 0; k <= 6; k++) {
          const angle = (k / 6) * Math.PI * 2;
          const px = x + Math.cos(angle) * radius;
          const py = y + Math.sin(angle) * radius;
          if (k === 0) ctx.moveTo(px, py);
          else ctx.lineTo(px, py);
        }
        ctx.stroke();
      }

      // Center hexagon
      const centerSize = maxRadius * 0.25 * (0.8 + value * 0.4);
      ctx.fillStyle = getColor(index + 200, cols * rows, value);
      ctx.beginPath();
      for (let k = 0; k < 6; k++) {
        const angle = (k / 6) * Math.PI * 2;
        const px = x + Math.cos(angle) * centerSize;
        const py = y + Math.sin(angle) * centerSize;
        if (k === 0) ctx.moveTo(px, py);
        else ctx.lineTo(px, py);
      }
      ctx.closePath();
      ctx.fill();
    }
  }

  ctx.restore();
}

function drawSpiralGalaxy() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const centerX = width / 2;
  const centerY = height / 2;
  const spirals = 5;
  const points = 120;

  for (let s = 0; s < spirals; s++) {
    const spiralAngle = (s / spirals) * Math.PI * 2;
    
    for (let i = 0; i < points; i++) {
      const t = i / points;
      const angle = t * Math.PI * 6 + spiralAngle + rotationAngle;
      const radius = t * Math.min(width, height) * 0.7;
      
      const dataIndex = Math.floor(t * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + i * 0.1 + s) * 0.3 + 0.5;
      
      const x = centerX + Math.cos(angle) * radius;
      const y = centerY + Math.sin(angle) * radius;
      const size = 8 + value * 25;

      // Draw hexagon at each point
      ctx.fillStyle = getColor(i + s * 100, points * spirals, value * 0.8);
      ctx.beginPath();
      for (let k = 0; k < 6; k++) {
        const hexAngle = (k / 6) * Math.PI * 2 + angle;
        const px = x + Math.cos(hexAngle) * size;
        const py = y + Math.sin(hexAngle) * size;
        if (k === 0) ctx.moveTo(px, py);
        else ctx.lineTo(px, py);
      }
      ctx.closePath();
      ctx.fill();

      // Add glow effect
      if (value > 0.6) {
        ctx.shadowBlur = 15;
        ctx.shadowColor = getColor(i + s * 100, points * spirals, 1);
        ctx.fill();
        ctx.shadowBlur = 0;
      }
    }
  }
}

function animate() {
  const width = canvas.value.width;
  const height = canvas.value.height;

  // Clear with fade effect
  ctx.fillStyle = 'rgba(10, 10, 10, 0.3)';
  ctx.fillRect(0, 0, width, height);

  if (audioLoaded.value) {
    analyser.getByteFrequencyData(dataArray);
    rotationAngle += 0.005;
  } else {
    // Breathing animation when no audio
    breathePhase += 0.02;
    rotationAngle += 0.002;
  }

  // Auto-rotate patterns and palettes
  if (autoRotate.value && audioLoaded.value) {
    const now = Date.now();
    if (now - lastRotateTime > 15000) { // 15 seconds
      cyclePattern();
      if (Math.random() > 0.5) cyclePalette();
      lastRotateTime = now;
    }
  }

  // Draw current pattern
  patterns[currentPatternIndex.value].draw();

  animationId = requestAnimationFrame(animate);
}

function cyclePattern() {
  currentPatternIndex.value = (currentPatternIndex.value + 1) % patterns.length;
  currentPatternName.value = patterns[currentPatternIndex.value].name;
}

function cyclePalette() {
  currentPaletteIndex.value = (currentPaletteIndex.value + 1) % palettes.length;
  currentPaletteName.value = palettes[currentPaletteIndex.value].name;
}

function toggleAutoRotate() {
  autoRotate.value = !autoRotate.value;
  if (autoRotate.value) {
    lastRotateTime = Date.now();
  }
}

function resize() {
  canvas.value.width = window.innerWidth;
  canvas.value.height = window.innerHeight;
}

onMounted(() => {
  ctx = canvas.value.getContext('2d');
  resize();
  window.addEventListener('resize', resize);
  animate();
});

onUnmounted(() => {
  window.removeEventListener('resize', resize);
  if (animationId) cancelAnimationFrame(animationId);
  if (audioContext) audioContext.close();
});
</script>

<style scoped>
.visualizer-app {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  background: #0a0a0a;
  color: #fff;
  overflow: hidden;
}

.controls {
  position: absolute;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 100;
  display: flex;
  gap: 15px;
  align-items: center;
  background: rgba(20, 20, 20, 0.8);
  padding: 15px 25px;
  border-radius: 50px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.file-input-label {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 10px 20px;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 600;
  font-size: 14px;
}

.file-input-label:hover {
  transform: scale(1.05);
  box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
}

input[type="file"] {
  display: none;
}

.pattern-btn, .palette-btn {
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: white;
  padding: 8px 16px;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 13px;
  font-weight: 500;
}

.pattern-btn:hover, .palette-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.pattern-btn.active, .palette-btn.active {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  border-color: transparent;
}

.visualizer-container {
  flex: 1;
  position: relative;
  overflow: hidden;
}

canvas {
  display: block;
  width: 100%;
  height: 100%;
}

.info {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(20, 20, 20, 0.8);
  padding: 10px 20px;
  border-radius: 20px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  font-size: 13px;
  color: rgba(255, 255, 255, 0.7);
}

@keyframes breathe {
  0%, 100% { opacity: 0.6; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.02); }
}

.breathing {
  animation: breathe 3s ease-in-out infinite;
}
</style>
