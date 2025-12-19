<template>
  <div class="visualizer-app">
    <div class="controls">
      <label class="file-input-label">
        Choose Audio
        <input type="file" @change="loadAudio" accept="audio/*">
      </label>
      <button class="pattern-btn" @click="cyclePattern">Pattern: {{ currentPatternName }}</button>
      <button class="palette-btn" @click="cyclePalette">Palette: {{ currentPaletteName }}</button>
      <button class="palette-btn" @click="cycleSoundwavePalette" v-if="audioLoaded">
        Wave: {{ palettes[soundwavePaletteIndex].name }}
      </button>
      <!-- <button class="pattern-btn" @click="toggleAutoRotate">
        Auto-Rotate: {{ autoRotate ? 'ON' : 'OFF' }}
      </button> -->
      <button class="pattern-btn" @click="togglePause" v-if="audioLoaded">
        {{ isPaused ? '▶ Play' : '⏸ Pause' }}
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
const isPaused = ref(false);
const soundwavePaletteIndex = ref(1); // Separate palette for soundwaves

let ctx, audioContext, analyser, dataArray, bufferLength;
let animationId;
let rotationAngle = 0;
let breathePhase = 0;
let lastRotateTime = Date.now();
let audioElement = null;
let particles = [];
let soundwaveHistory = []; // Store historical data for smoothing

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
  },
  {
    name: 'Acid Trip',
    colors: ['#00ff41', '#ff00ff', '#00ffff', '#ffff00', '#ff0080', '#00ff80', '#8000ff']
  },
  {
    name: 'Psychedelic',
    colors: ['#ff10f0', '#10ff10', '#ff8800', '#00ffff', '#ff0066', '#66ff00', '#8800ff']
  },
  {
    name: 'Rainbow Explosion',
    colors: ['#ff0000', '#ff7700', '#ffff00', '#00ff00', '#0088ff', '#4400ff', '#ff00ff']
  },
  {
    name: 'Vaporwave',
    colors: ['#ff6ad5', '#c774e8', '#ad8cff', '#8795e8', '#94d0ff', '#ff71ce']
  },
  {
    name: 'Electric Dreams',
    colors: ['#ff00de', '#00ff9f', '#00b8ff', '#ff3c00', '#d4ff00', '#ff0080']
  }
];

// Particle system for ephemeral effects
class Particle {
  constructor(x, y, energy, colorIndex, total) {
    this.x = x;
    this.y = y;
    this.vx = (Math.random() - 0.5) * 2;
    this.vy = (Math.random() - 0.5) * 2;
    this.life = 1.0;
    this.maxLife = 60 + Math.random() * 60;
    this.size = 5 + energy * 30;
    this.energy = energy;
    this.colorIndex = colorIndex;
    this.total = total;
    this.rotation = Math.random() * Math.PI * 2;
    this.rotationSpeed = (Math.random() - 0.5) * 0.1;
    this.pulsePhase = Math.random() * Math.PI * 2;
  }

  update() {
    this.x += this.vx;
    this.y += this.vy;
    this.vx *= 0.98;
    this.vy *= 0.98;
    this.life--;
    this.rotation += this.rotationSpeed;
    this.pulsePhase += 0.1;
  }

  draw(ctx) {
    const alpha = this.life / this.maxLife;
    const pulse = Math.sin(this.pulsePhase) * 0.3 + 0.7;
    const size = this.size * pulse * alpha;

    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate(this.rotation);

    // Draw trailing streak
    const trailLength = 12;
    const gradient = ctx.createLinearGradient(-this.vx * trailLength, -this.vy * trailLength, 0, 0);
    gradient.addColorStop(0, 'rgba(0,0,0,0)');
    gradient.addColorStop(1, getColor(this.colorIndex, this.total, 0.3 * alpha));
    ctx.strokeStyle = gradient;
    ctx.lineWidth = size * 0.5;
    ctx.beginPath();
    ctx.moveTo(-this.vx * trailLength, -this.vy * trailLength);
    ctx.lineTo(0, 0);
    ctx.stroke();

    // Outer glow with chromatic aberration
    for (let offset = 0; offset < 3; offset++) {
      const aberrationGradient = ctx.createRadialGradient(0, 0, 0, 0, 0, size * 2.5);
      const offsetColor = getColor(this.colorIndex + offset * 30, this.total, 0.15 * alpha);
      aberrationGradient.addColorStop(0, offsetColor);
      aberrationGradient.addColorStop(1, 'rgba(0,0,0,0)');
      
      ctx.fillStyle = aberrationGradient;
      ctx.beginPath();
      ctx.arc(offset * 2, offset * 2, size * 2.5, 0, Math.PI * 2);
      ctx.fill();
    }

    // Inner hexagon with refraction effect
    ctx.fillStyle = getColor(this.colorIndex, this.total, 0.9 * alpha);
    ctx.strokeStyle = getColor(this.colorIndex + 50, this.total, 1 * alpha);
    ctx.lineWidth = 2;
    ctx.shadowBlur = 25;
    ctx.shadowColor = getColor(this.colorIndex, this.total, 0.9 * alpha);
    
    ctx.beginPath();
    for (let k = 0; k < 6; k++) {
      const angle = (k / 6) * Math.PI * 2;
      const px = Math.cos(angle) * size;
      const py = Math.sin(angle) * size;
      if (k === 0) ctx.moveTo(px, py);
      else ctx.lineTo(px, py);
    }
    ctx.closePath();
    ctx.fill();
    ctx.stroke();

    // Refraction rings with color shifts
    for (let r = 0; r < 4; r++) {
      const ringSize = size * (0.3 + r * 0.2);
      ctx.strokeStyle = getColor(this.colorIndex + r * 60, this.total, 0.4 * alpha);
      ctx.lineWidth = 1.5;
      ctx.beginPath();
      ctx.arc(0, 0, ringSize, 0, Math.PI * 2);
      ctx.stroke();
    }

    ctx.restore();
  }

  isDead() {
    return this.life <= 0;
  }
}

function createParticles(x, y, energy, colorIndex, total, count = 1) {
  for (let i = 0; i < count; i++) {
    particles.push(new Particle(x, y, energy, colorIndex, total));
  }
}

function updateParticles() {
  particles = particles.filter(p => !p.isDead());
  particles.forEach(p => p.update());
}

function drawParticles() {
  particles.forEach(p => p.draw(ctx));
}

function getSoundwaveColor(index, total, intensity, paletteIdx) {
  const palette = palettes[paletteIdx].colors;
  const colorIndex = Math.floor((index / total) * palette.length);
  const color = palette[colorIndex % palette.length];
  
  const r = parseInt(color.slice(1, 3), 16);
  const g = parseInt(color.slice(3, 5), 16);
  const b = parseInt(color.slice(5, 7), 16);
  
  return `rgba(${r}, ${g}, ${b}, ${intensity})`;
}

function drawSoundwaves() {
  if (!audioLoaded.value) return;
  
  const width = canvas.value.width;
  const height = canvas.value.height;
  const waveCount = 4;
  
  // Store current data in history
  if (soundwaveHistory.length > 3) {
    soundwaveHistory.shift();
  }
  soundwaveHistory.push([...dataArray]);
  
  for (let w = 0; w < waveCount; w++) {
    const yOffset = height * (0.3 + w * 0.15);
    const waveHeight = 80 + w * 20;
    const alpha = 0.8 - w * 0.15;
    const thickness = 4 - w * 0.5;
    
    // Create gradient for the wave
    const gradient = ctx.createLinearGradient(0, yOffset - waveHeight, 0, yOffset + waveHeight);
    gradient.addColorStop(0, getSoundwaveColor(w * 50, waveCount * 50, alpha * 0.3, soundwavePaletteIndex.value));
    gradient.addColorStop(0.5, getSoundwaveColor(w * 50, waveCount * 50, alpha, soundwavePaletteIndex.value));
    gradient.addColorStop(1, getSoundwaveColor(w * 50, waveCount * 50, alpha * 0.3, soundwavePaletteIndex.value));
    
    // Draw main wave
    ctx.strokeStyle = gradient;
    ctx.lineWidth = thickness;
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    
    // Add glow
    ctx.shadowBlur = 15;
    ctx.shadowColor = getSoundwaveColor(w * 50, waveCount * 50, alpha * 0.6, soundwavePaletteIndex.value);
    
    ctx.beginPath();
    
    const segments = 200;
    const dataStep = Math.floor(bufferLength / segments);
    
    for (let i = 0; i < segments; i++) {
      const x = (i / segments) * width;
      const dataIndex = i * dataStep;
      const value = dataArray[dataIndex] / 255;
      
      // Smooth with history
      let smoothValue = value;
      if (soundwaveHistory.length > 0) {
        const historyAvg = soundwaveHistory.reduce((sum, hist) => sum + hist[dataIndex] / 255, 0) / soundwaveHistory.length;
        smoothValue = value * 0.6 + historyAvg * 0.4;
      }
      
      // Create wave motion
      const wavePhase = (i / segments) * Math.PI * 4 + rotationAngle * (w + 1);
      const baseWave = Math.sin(wavePhase) * 15;
      const y = yOffset + baseWave + (smoothValue - 0.5) * waveHeight;
      
      if (i === 0) {
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }
    }
    
    ctx.stroke();
    ctx.shadowBlur = 0;
    
    // Draw reflection wave (mirrored)
    const reflectionAlpha = alpha * 0.3;
    ctx.strokeStyle = getSoundwaveColor(w * 50 + 25, waveCount * 50, reflectionAlpha, soundwavePaletteIndex.value);
    ctx.lineWidth = thickness * 0.7;
    ctx.shadowBlur = 8;
    ctx.shadowColor = getSoundwaveColor(w * 50 + 25, waveCount * 50, reflectionAlpha * 0.5, soundwavePaletteIndex.value);
    
    ctx.beginPath();
    for (let i = 0; i < segments; i++) {
      const x = (i / segments) * width;
      const dataIndex = i * dataStep;
      const value = dataArray[dataIndex] / 255;
      
      let smoothValue = value;
      if (soundwaveHistory.length > 0) {
        const historyAvg = soundwaveHistory.reduce((sum, hist) => sum + hist[dataIndex] / 255, 0) / soundwaveHistory.length;
        smoothValue = value * 0.6 + historyAvg * 0.4;
      }
      
      const wavePhase = (i / segments) * Math.PI * 4 + rotationAngle * (w + 1);
      const baseWave = Math.sin(wavePhase) * 15;
      const y = yOffset - baseWave - (smoothValue - 0.5) * waveHeight * 0.5;
      
      if (i === 0) {
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }
    }
    
    ctx.stroke();
    ctx.shadowBlur = 0;
    
    // Add peak indicators
    for (let i = 0; i < segments; i += 10) {
      const x = (i / segments) * width;
      const dataIndex = i * dataStep;
      const value = dataArray[dataIndex] / 255;
      
      if (value > 0.75) {
        const wavePhase = (i / segments) * Math.PI * 4 + rotationAngle * (w + 1);
        const baseWave = Math.sin(wavePhase) * 15;
        const y = yOffset + baseWave + (value - 0.5) * waveHeight;
        
        // Draw glowing dot
        const dotSize = 4 + value * 6;
        ctx.fillStyle = getSoundwaveColor(w * 50 + i, waveCount * 50, alpha, soundwavePaletteIndex.value);
        ctx.shadowBlur = 12;
        ctx.shadowColor = getSoundwaveColor(w * 50 + i, waveCount * 50, alpha, soundwavePaletteIndex.value);
        ctx.beginPath();
        ctx.arc(x, y, dotSize, 0, Math.PI * 2);
        ctx.fill();
        ctx.shadowBlur = 0;
        
        // Draw energy burst lines
        for (let a = 0; a < 6; a++) {
          const angle = (a / 6) * Math.PI * 2 + rotationAngle;
          const length = value * 20;
          ctx.strokeStyle = getSoundwaveColor(w * 50 + i + a, waveCount * 50, alpha * 0.4, soundwavePaletteIndex.value);
          ctx.lineWidth = 1;
          ctx.beginPath();
          ctx.moveTo(x, y);
          ctx.lineTo(x + Math.cos(angle) * length, y + Math.sin(angle) * length);
          ctx.stroke();
        }
      }
    }
  }
}

// Pattern Definitions
const patterns = [
  { name: 'Spiral Galaxy', draw: drawSpiralGalaxy },
  // { name: 'Cubic Grid', draw: drawCubicGrid },
  { name: 'Diamond Lattice', draw: drawDiamondLattice },
  { name: 'Hex Flowers', draw: drawHexFlowers },
  { name: 'Concentric Waves', draw: drawConcentricWaves },
  { name: 'Kaleidoscope', draw: drawKaleidoscope },
  // { name: 'Fractal Burst', draw: drawFractalBurst }
];

const currentPatternName = ref(patterns[0].name);
const currentPaletteName = ref(palettes[0].name);

function drawKaleidoscope() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const centerX = width / 2;
  const centerY = height / 2;
  const segments = 8;
  
  ctx.save();
  ctx.translate(centerX, centerY);
  
  for (let seg = 0; seg < segments; seg++) {
    ctx.save();
    ctx.rotate((seg / segments) * Math.PI * 2);
    
    // Mirror every other segment for kaleidoscope effect
    if (seg % 2 === 1) {
      ctx.scale(-1, 1);
    }
    
    const rings = 12;
    for (let r = 0; r < rings; r++) {
      const radius = (r / rings) * Math.min(width, height) * 0.7;
      const dataIndex = Math.floor((r / rings) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + r * 0.2) * 0.3 + 0.5;
      
      const points = 6 + Math.floor(r / 2);
      
      for (let p = 0; p < points; p++) {
        const angle = (p / points) * Math.PI / segments + rotationAngle * (r % 2 === 0 ? 1 : -1);
        const x = Math.cos(angle) * radius;
        const y = Math.sin(angle) * radius;
        const size = 8 + value * 30;
        
        // Spawn particles
        if (audioLoaded.value && value > 0.8 && Math.random() > 0.96) {
          createParticles(centerX + x, centerY + y, value, r * points + p, rings * points, 1);
        }
        
        // Draw hexagon
        ctx.fillStyle = getColor(r * points + p + seg * 50, rings * points * segments, value * 0.8);
        ctx.shadowBlur = value * 20;
        ctx.shadowColor = getColor(r * points + p + seg * 50, rings * points * segments, value);
        
        ctx.beginPath();
        for (let k = 0; k < 6; k++) {
          const hexAngle = (k / 6) * Math.PI * 2 + rotationAngle;
          const px = x + Math.cos(hexAngle) * size;
          const py = y + Math.sin(hexAngle) * size;
          if (k === 0) ctx.moveTo(px, py);
          else ctx.lineTo(px, py);
        }
        ctx.closePath();
        ctx.fill();
        
        // Connecting lines
        if (value > 0.6) {
          ctx.strokeStyle = getColor(r * points + p + seg * 50, rings * points * segments, value * 0.3);
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.moveTo(0, 0);
          ctx.lineTo(x, y);
          ctx.stroke();
        }
        
        ctx.shadowBlur = 0;
      }
    }
    
    ctx.restore();
  }
  
  ctx.restore();
}

// function drawFractalBurst() {
//   const width = canvas.value.width;
//   const height = canvas.value.height;
//   const centerX = width / 2;
//   const centerY = height / 2;
  
//   function drawFractalLevel(x, y, size, depth, angle, colorOffset) {
//     if (depth === 0 || size < 2) return;
    
//     const dataIndex = Math.floor((depth / 6) * bufferLength);
//     const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + depth * 0.3) * 0.3 + 0.5;
    
//     // Draw center shape
//     ctx.fillStyle = getColor(colorOffset + depth * 30, 300, value * 0.7);
//     ctx.shadowBlur = value * 15;
//     ctx.shadowColor = getColor(colorOffset + depth * 30, 300, value);
    
//     ctx.beginPath();
//     for (let k = 0; k < 6; k++) {
//       const hexAngle = (k / 6) * Math.PI * 2 + angle;
//       const px = x + Math.cos(hexAngle) * size;
//       const py = y + Math.sin(hexAngle) * size;
//       if (k === 0) ctx.moveTo(px, py);
//       else ctx.lineTo(px, py);
//     }
//     ctx.closePath();
//     ctx.fill();
//     ctx.shadowBlur = 0;
    
//     // Spawn particles at high energy
//     if (audioLoaded.value && value > 0.75 && depth === 3 && Math.random() > 0.95) {
//       createParticles(x, y, value, colorOffset, 100, 2);
//     }
    
//     // Recurse to create fractal branches
//     const branches = 6;
//     const newSize = size * 0.5;
//     const dist = size * 2.5 * (0.8 + value * 0.4);
    
//     for (let b = 0; b < branches; b++) {
//       const branchAngle = (b / branches) * Math.PI * 2 + angle + rotationAngle * (depth % 2 === 0 ? 1 : -1);
//       const nx = x + Math.cos(branchAngle) * dist;
//       const ny = y + Math.sin(branchAngle) * dist;
      
//       // Draw connecting line
//       if (value > 0.5) {
//         ctx.strokeStyle = getColor(colorOffset + depth * 30 + b * 10, 300, value * 0.3);
//         ctx.lineWidth = depth;
//         ctx.beginPath();
//         ctx.moveTo(x, y);
//         ctx.lineTo(nx, ny);
//         ctx.stroke();
//       }
      
//       drawFractalLevel(nx, ny, newSize, depth - 1, branchAngle, colorOffset + b * 20);
//     }
//   }
  
//   drawFractalLevel(centerX, centerY, 40, 5, rotationAngle, 0);
// }

function loadAudio(event) {
  const file = event.target.files[0];
  if (!file) return;

  fileName.value = file.name;
  audioElement = new Audio();
  const reader = new FileReader();

  reader.onload = (e) => {
    audioElement.src = e.target.result;
    audioElement.play();
    setupAudioContext(audioElement);
    audioLoaded.value = true;
    isPaused.value = false;
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
  // Add color cycling for psychedelic effect
  const cycleOffset = Math.floor(rotationAngle * 10) % palette.length;
  const colorIndex = Math.floor((index / total) * palette.length) + cycleOffset;
  const color = palette[colorIndex % palette.length];
  
  // Convert hex to rgb and apply intensity
  const r = parseInt(color.slice(1, 3), 16);
  const g = parseInt(color.slice(3, 5), 16);
  const b = parseInt(color.slice(5, 7), 16);
  
  // Add subtle color shifting based on audio intensity for psychedelic effect
  const shift = audioLoaded.value ? Math.sin(rotationAngle + index * 0.1) * 12 : 0;
  const rShifted = Math.max(0, Math.min(255, r + shift));
  const gShifted = Math.max(0, Math.min(255, g + shift * 0.5));
  const bShifted = Math.max(0, Math.min(255, b - shift * 0.5));
  
  return `rgba(${rShifted}, ${gShifted}, ${bShifted}, ${intensity})`;
}

// function drawCubicGrid() {
//   const width = canvas.value.width;
//   const height = canvas.value.height;
//   const size = 60;
//   const cols = Math.ceil(width / size) + 2;
//   const rows = Math.ceil(height / size) + 2;

//   ctx.save();
//   ctx.translate(width / 2, height / 2);
//   ctx.rotate(rotationAngle * 0.2);
//   ctx.translate(-width / 2, -height / 2);

//   for (let i = 0; i < cols; i++) {
//     for (let j = 0; j < rows; j++) {
//       const x = (i - 1) * size;
//       const y = (j - 1) * size;
//       const index = i + j * cols;
//       const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
//       const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.1) * 0.3 + 0.5;
      
//       const scale = audioLoaded.value ? 0.8 + value * 0.4 : 0.9 + value * 0.1;
//       const depth = value * 30;

//       // Spawn particles on high energy
//       if (audioLoaded.value && value > 0.73 && Math.random() > 0.93) {
//         const worldX = x + size / 2;
//         const worldY = y + size / 2;
//         createParticles(worldX, worldY, value, index, cols * rows, 2);
//       }

//       ctx.save();
//       ctx.translate(x + size / 2, y + size / 2);
//       ctx.scale(scale, scale);

//       // Draw 3D cube effect
//       const cubeSize = size * 0.7;
      
//       // Glow effect for high energy
//       if (value > 0.6) {
//         ctx.shadowBlur = 20 * value;
//         ctx.shadowColor = getColor(index, cols * rows, value);
//       }
      
//       // Top face
//       ctx.fillStyle = getColor(index, cols * rows, value * 0.8);
//       ctx.beginPath();
//       ctx.moveTo(-cubeSize / 2, -cubeSize / 2 - depth);
//       ctx.lineTo(cubeSize / 2, -cubeSize / 2 - depth);
//       ctx.lineTo(cubeSize / 2 + depth / 2, -cubeSize / 2 - depth / 2);
//       ctx.lineTo(-cubeSize / 2 + depth / 2, -cubeSize / 2 - depth / 2);
//       ctx.closePath();
//       ctx.fill();

//       // Front face
//       ctx.fillStyle = getColor(index, cols * rows, value * 0.6);
//       ctx.fillRect(-cubeSize / 2, -cubeSize / 2, cubeSize, cubeSize);

//       // Side face
//       ctx.fillStyle = getColor(index, cols * rows, value * 0.4);
//       ctx.beginPath();
//       ctx.moveTo(cubeSize / 2, -cubeSize / 2);
//       ctx.lineTo(cubeSize / 2 + depth / 2, -cubeSize / 2 - depth / 2);
//       ctx.lineTo(cubeSize / 2 + depth / 2, cubeSize / 2 - depth / 2);
//       ctx.lineTo(cubeSize / 2, cubeSize / 2);
//       ctx.closePath();
//       ctx.fill();

//       // Border with glow
//       ctx.strokeStyle = getColor(index, cols * rows, 1);
//       ctx.lineWidth = value > 0.7 ? 3 : 2;
//       ctx.strokeRect(-cubeSize / 2, -cubeSize / 2, cubeSize, cubeSize);
      
//       ctx.shadowBlur = 0;

//       // Energy pulse lines
//       if (value > 0.5) {
//         ctx.strokeStyle = getColor(index + 100, cols * rows, value * 0.5);
//         ctx.lineWidth = 1;
//         ctx.setLineDash([5, 5]);
//         ctx.beginPath();
//         ctx.moveTo(-cubeSize / 2, 0);
//         ctx.lineTo(cubeSize / 2, 0);
//         ctx.moveTo(0, -cubeSize / 2);
//         ctx.lineTo(0, cubeSize / 2);
//         ctx.stroke();
//         ctx.setLineDash([]);
//       }

//       ctx.restore();
//     }
//   }

//   ctx.restore();
// }

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

      // Spawn particles
      if (audioLoaded.value && value > 0.77 && Math.random() > 0.9) {
        createParticles(x, y, value, index, cols * rows, 3);
      }

      // Outer glow ring
      if (value > 0.5) {
        ctx.strokeStyle = getColor(index, cols * rows, (value - 0.5) * 0.3);
        ctx.lineWidth = 8;
        ctx.shadowBlur = 15;
        ctx.shadowColor = getColor(index, cols * rows, value);
        ctx.beginPath();
        for (let k = 0; k < 8; k++) {
          const angle = (k / 8) * Math.PI * 2;
          const px = x + Math.cos(angle) * (octagonSize + 10);
          const py = y + Math.sin(angle) * (octagonSize + 10);
          if (k === 0) ctx.moveTo(px, py);
          else ctx.lineTo(px, py);
        }
        ctx.closePath();
        ctx.stroke();
        ctx.shadowBlur = 0;
      }

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
      ctx.lineWidth = value > 0.6 ? 3 : 2;
      ctx.stroke();

      // Draw center diamond with glow
      if (value > 0.6) {
        ctx.shadowBlur = 15;
        ctx.shadowColor = getColor(index + 100, cols * rows, value);
      }
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
      ctx.shadowBlur = 0;

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

        // Energy lines connecting to center
        if (value > 0.6) {
          ctx.strokeStyle = getColor(index, cols * rows, value * 0.3);
          ctx.lineWidth = 1;
          ctx.beginPath();
          ctx.moveTo(cx, cy);
          ctx.lineTo(x, y);
          ctx.stroke();
        }
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
      
      // Spawn particles at center
      if (audioLoaded.value && value > 0.75 && Math.random() > 0.88) {
        createParticles(x, y, value, index, cols * rows, 2);
      }

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

      // Draw connecting lines
      if (value > 0.5) {
        ctx.strokeStyle = getColor(index, cols * rows, value * 0.2);
        ctx.lineWidth = 2;
        pattern.slice(1).forEach(p => {
          const angle = (p.angle * Math.PI / 180) + rotationAngle;
          const hx = x + Math.cos(angle) * p.dist;
          const hy = y + Math.sin(angle) * p.dist;
          ctx.beginPath();
          ctx.moveTo(x, y);
          ctx.lineTo(hx, hy);
          ctx.stroke();
        });
      }

      pattern.forEach((p, pi) => {
        const angle = (p.angle * Math.PI / 180) + rotationAngle;
        const hx = x + Math.cos(angle) * p.dist;
        const hy = y + Math.sin(angle) * p.dist;
        const size = hexRadius * p.scale * (0.7 + value * 0.5);
        const intensity = pi === 0 ? value : value * 0.6;

        // Glow for high energy
        if (intensity > 0.6) {
          ctx.shadowBlur = 15;
          ctx.shadowColor = getColor(index + pi * 30, cols * rows, intensity);
        }

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
        ctx.shadowBlur = 0;

        // Draw refraction rings for center hexagon
        if (pi === 0 && value > 0.4) {
          for (let r = 1; r <= 3; r++) {
            ctx.strokeStyle = getColor(index + r * 40, cols * rows, (value - 0.4) * 0.3);
            ctx.lineWidth = 1;
            ctx.beginPath();
            for (let k = 0; k < 6; k++) {
              const hexAngle = (k / 6) * Math.PI * 2;
              const px = hx + Math.cos(hexAngle) * (size + r * 8);
              const py = hy + Math.sin(hexAngle) * (size + r * 8);
              if (k === 0) ctx.moveTo(px, py);
              else ctx.lineTo(px, py);
            }
            ctx.closePath();
            ctx.stroke();
          }
        }
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

      // Spawn particles from rings
      if (audioLoaded.value && value > 0.82 && Math.random() > 0.88) {
        const angle = Math.random() * Math.PI * 2;
        const dist = maxRadius * 0.8;
        const px = x + Math.cos(angle) * dist;
        const py = y + Math.sin(angle) * dist;
        createParticles(px, py, value, index, cols * rows, 4);
      }

      // Outer glow pulse
      if (value > 0.5) {
        const glowRadius = maxRadius * 1.2 * (1 + (value - 0.5));
        const gradient = ctx.createRadialGradient(x, y, maxRadius * 0.8, x, y, glowRadius);
        gradient.addColorStop(0, 'rgba(0,0,0,0)');
        gradient.addColorStop(1, getColor(index, cols * rows, (value - 0.5) * 0.4));
        ctx.fillStyle = gradient;
        ctx.fillRect(x - glowRadius, y - glowRadius, glowRadius * 2, glowRadius * 2);
      }

      for (let r = 0; r < rings; r++) {
        const radius = (maxRadius / rings) * (r + 1) * (0.8 + value * 0.4);
        const ringValue = audioLoaded.value ? dataArray[(dataIndex + r * 10) % bufferLength] / 255 : value;
        const thickness = (maxRadius / rings) * 0.7 * (0.8 + ringValue * 0.4);

        // Add glow to high energy rings
        if (ringValue > 0.7) {
          ctx.shadowBlur = 10;
          ctx.shadowColor = getColor(index + r * 20, cols * rows, ringValue);
        }

        // Hexagonal ring
        ctx.strokeStyle = getColor(index + r * 20, cols * rows, ringValue);
        ctx.lineWidth = thickness;
        ctx.beginPath();
        for (let k = 0; k <= 6; k++) {
          const angle = (k / 6) * Math.PI * 2 + rotationAngle * (r + 1) * 0.1;
          const px = x + Math.cos(angle) * radius;
          const py = y + Math.sin(angle) * radius;
          if (k === 0) ctx.moveTo(px, py);
          else ctx.lineTo(px, py);
        }
        ctx.stroke();
        ctx.shadowBlur = 0;

        // Draw nodes at intersections
        if (ringValue > 0.6) {
          for (let k = 0; k < 6; k++) {
            const angle = (k / 6) * Math.PI * 2 + rotationAngle * (r + 1) * 0.1;
            const px = x + Math.cos(angle) * radius;
            const py = y + Math.sin(angle) * radius;
            const nodeSize = 3 + ringValue * 4;
            
            ctx.fillStyle = getColor(index + r * 20 + k * 10, cols * rows, ringValue);
            ctx.beginPath();
            ctx.arc(px, py, nodeSize, 0, Math.PI * 2);
            ctx.fill();
          }
        }
      }

      // Center hexagon
      const centerSize = maxRadius * 0.25 * (0.8 + value * 0.4);
      if (value > 0.6) {
        ctx.shadowBlur = 20;
        ctx.shadowColor = getColor(index + 200, cols * rows, value);
      }
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
      ctx.shadowBlur = 0;
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

      // Spawn particles along spiral
      if (audioLoaded.value && value > 0.75 && Math.random() > 0.97) {
        createParticles(x, y, value, i + s * 100, points * spirals, 1);
      }

      // Trail effect
      if (i > 0 && value > 0.4) {
        const prevT = (i - 1) / points;
        const prevAngle = prevT * Math.PI * 6 + spiralAngle + rotationAngle;
        const prevRadius = prevT * Math.min(width, height) * 0.7;
        const prevX = centerX + Math.cos(prevAngle) * prevRadius;
        const prevY = centerY + Math.sin(prevAngle) * prevRadius;

        ctx.strokeStyle = getColor(i + s * 100, points * spirals, value * 0.3);
        ctx.lineWidth = 2 + value * 3;
        ctx.beginPath();
        ctx.moveTo(prevX, prevY);
        ctx.lineTo(x, y);
        ctx.stroke();
      }

      // Draw hexagon at each point
      if (value > 0.6) {
        ctx.shadowBlur = 15;
        ctx.shadowColor = getColor(i + s * 100, points * spirals, value);
      }
      
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

      // Add border
      ctx.strokeStyle = getColor(i + s * 100, points * spirals, 1);
      ctx.lineWidth = 1;
      ctx.stroke();

      ctx.shadowBlur = 0;

      // Add refraction circles for high energy
      if (value > 0.7) {
        for (let r = 1; r <= 2; r++) {
          ctx.strokeStyle = getColor(i + s * 100 + r * 30, points * spirals, (value - 0.7) * 0.4);
          ctx.lineWidth = 1;
          ctx.beginPath();
          ctx.arc(x, y, size + r * 6, 0, Math.PI * 2);
          ctx.stroke();
        }
      }
    }
  }

  // Draw central core
  const coreSize = 40 + Math.sin(breathePhase) * 10;
  const coreValue = audioLoaded.value ? dataArray[0] / 255 : 0.5;
  
  ctx.shadowBlur = 30;
  ctx.shadowColor = getColor(0, 1, coreValue);
  ctx.fillStyle = getColor(0, 1, coreValue * 0.8);
  ctx.beginPath();
  ctx.arc(centerX, centerY, coreSize, 0, Math.PI * 2);
  ctx.fill();
  ctx.shadowBlur = 0;

  // Core rings
  for (let r = 1; r <= 4; r++) {
    ctx.strokeStyle = getColor(r * 50, 200, 0.3 + coreValue * 0.3);
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.arc(centerX, centerY, coreSize + r * 10, 0, Math.PI * 2);
    ctx.stroke();
  }
}

function animate() {
  const width = canvas.value.width;
  const height = canvas.value.height;

  // Pulsing background for psychedelic effect
  if (audioLoaded.value && !isPaused.value) {
    const avgEnergy = dataArray.reduce((sum, val) => sum + val, 0) / bufferLength / 255;
    const bgPulse = Math.floor(10 + avgEnergy * 15);
    ctx.fillStyle = `rgba(${bgPulse}, ${bgPulse * 0.5}, ${bgPulse * 1.5}, 0.25)`;
  } else {
    ctx.fillStyle = 'rgba(10, 10, 10, 0.25)';
  }
  ctx.fillRect(0, 0, width, height);

  if (audioLoaded.value && !isPaused.value) {
    analyser.getByteFrequencyData(dataArray);
    rotationAngle += 0.007; // Medium rotation speed
  } else {
    // Breathing animation when no audio
    breathePhase += 0.02;
    rotationAngle += 0.003; // Medium idle rotation
  }

  // Auto-rotate patterns and palettes
  if (autoRotate.value && audioLoaded.value && !isPaused.value) {
    const now = Date.now();
    if (now - lastRotateTime > 15000) { // 15 seconds
      cyclePattern();
      if (Math.random() > 0.5) cyclePalette();
      lastRotateTime = now;
    }
  }

  // Draw current pattern
  patterns[currentPatternIndex.value].draw();

  // Update and draw particles
  updateParticles();
  drawParticles();

  // Draw soundwaves on top
  drawSoundwaves();

  animationId = requestAnimationFrame(animate);
}

function togglePause() {
  if (!audioElement) return;
  
  if (isPaused.value) {
    audioElement.play();
    isPaused.value = false;
  } else {
    audioElement.pause();
    isPaused.value = true;
  }
}

function cyclePattern() {
  currentPatternIndex.value = (currentPatternIndex.value + 1) % patterns.length;
  currentPatternName.value = patterns[currentPatternIndex.value].name;
}

function cyclePalette() {
  currentPaletteIndex.value = (currentPaletteIndex.value + 1) % palettes.length;
  currentPaletteName.value = palettes[currentPaletteIndex.value].name;
}

function cycleSoundwavePalette() {
  soundwavePaletteIndex.value = (soundwavePaletteIndex.value + 1) % palettes.length;
}

// function toggleAutoRotate() {
//   autoRotate.value = !autoRotate.value;
//   if (autoRotate.value) {
//     lastRotateTime = Date.now();
//   }
// }

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
