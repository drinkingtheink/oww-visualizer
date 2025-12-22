<template>
  <div class="visualizer-app">
    <div class="controls">
      <!-- <label class="file-input-label">
        Choose Audio
        <input type="file" @change="loadAudio" accept="audio/*">
      </label> -->
      <button class="pattern-btn" @click="cyclePattern">Pattern: {{ currentPatternName }}</button>
      <button class="palette-btn" @click="cyclePalette">Palette: {{ currentPaletteName }}</button>
      <!-- <button class="pattern-btn" @click="togglePause" v-if="audioLoaded">
        {{ isPaused ? '▶ Play' : '⏸ Pause' }}
      </button> -->
    </div>

    <!-- Music Player Component -->
    <MusicPlayer
      v-if="useMusicPlayer"
      :tracks="tracks"
      :current-track-index="currentTrackIndex"
      :is-playing="!isPaused && audioLoaded"
      @track-change="handleTrackChange"
      @play-pause="togglePause"
      @next="nextTrack"
      @previous="previousTrack"
    />

    <div class="visualizer-container">
      <canvas ref="canvas"></canvas>
    </div>

    <div class="info">
      <span class="hint"> Use ← → to change Pattern | ↑ ↓ to change Colors</span>
    </div>

    <Seraphim
      v-if="audioLoaded"
      :audioData="dataArray" 
      :audioLoaded="audioLoaded"
      :isPaused="isPaused"
      />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import MusicPlayer from './MusicPlayer.vue';
import Seraphim from './Seraphim.vue';

const canvas = ref(null);
const audioLoaded = ref(false);
const fileName = ref('');
const currentPatternIndex = ref(0);
const currentPaletteIndex = ref(0);
const isPaused = ref(false);

// Music player state
const useMusicPlayer = ref(true); // Set to false to use file input instead
const currentTrackIndex = ref(0);
const tracks = ref([
  { 
    name: 'Crate Diggers Local 227', 
    url: 'https://pub-9bbd55405c2f4d19af472cb366881d09.r2.dev/LANDR-crate-diggers-local-227-MASTER-Open-Low.wav' 
  },
  { 
    name: 'Mantra Loupe', 
    url: 'https://pub-9bbd55405c2f4d19af472cb366881d09.r2.dev/LANDR-mantra-loupe-MASTER-Balanced-Medium.wav' 
  },
  { 
    name: 'As Sane As Any Of Us', 
    url: 'https://pub-9bbd55405c2f4d19af472cb366881d09.r2.dev/LANDR-as-sane-as-any-of-us-MASTER-Open-Medium.wav' 
  },
  { 
    name: 'Virtuous Vitreous', 
    url: 'https://pub-9bbd55405c2f4d19af472cb366881d09.r2.dev/LANDR-virtuous-vitreous-MASTER-Balanced-High.wav' 
  },
  { 
    name: "I Stole A Glance At My Brother's Sketchbook", 
    url: 'https://pub-9bbd55405c2f4d19af472cb366881d09.r2.dev/LANDR-i-stole-a-glance-at-my-brothers-sketchbook-MASTER-Warm-Medium.wav' 
  },
  { 
    name: "Dr. Remember's Miracle Elixir", 
    url: 'https://pub-9bbd55405c2f4d19af472cb366881d09.r2.dev/LANDR-dr-remembers-miracle-elixir-MASTER-Balanced-Medium.wav' 
  },
  { 
    name: 'Down And Out In Sidereal Time', 
    url: 'https://pub-9bbd55405c2f4d19af472cb366881d09.r2.dev/LANDR-down-and-out-in-sidereal-time-MASTER-Balanced-Medium.wav' 
  }
]);

let ctx, audioContext, analyser, dataArray, bufferLength;
let animationId;
let rotationAngle = 0;
let breathePhase = 0;
let audioElement = null;
let particles = [];

// Mouse/Touch interaction state
let mouseX = null;
let mouseY = null;
let isMouseDown = false;
let ripples = [];
let warpIntensity = 0;
let targetWarpIntensity = 0;

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
  },
  // NEW PALETTES BELOW
  {
    name: 'Midnight Purple',
    colors: ['#1a1a2e', '#16213e', '#0f3460', '#533483', '#e94560']
  },
  {
    name: 'Tropical Paradise',
    colors: ['#06d6a0', '#118ab2', '#073b4c', '#ef476f', '#ffd166']
  },
  {
    name: 'Candy Shop',
    colors: ['#ff006e', '#fb5607', '#ffbe0b', '#8338ec', '#3a86ff']
  },
  {
    name: 'Deep Space',
    colors: ['#0d1b2a', '#1b263b', '#415a77', '#778da9', '#e0e1dd']
  },
  {
    name: 'Retro Wave',
    colors: ['#f72585', '#7209b7', '#560bad', '#3a0ca3', '#4361ee', '#4cc9f0']
  },
  {
    name: 'Forest Glow',
    colors: ['#114b5f', '#1a936f', '#88d498', '#c6dabf', '#f3e9d2']
  },
  {
    name: 'Lava Flow',
    colors: ['#d62828', '#f77f00', '#fcbf49', '#eae2b7', '#003049']
  },
  {
    name: 'Cyberpunk',
    colors: ['#00f5ff', '#ff00ff', '#ffff00', '#00ff00', '#ff0099', '#9d00ff']
  },
  {
    name: 'Pastel Dream',
    colors: ['#f4acb7', '#ffcad4', '#b392ac', '#9d84b7', '#d4a5a5']
  },
  {
    name: 'Aurora Borealis',
    colors: ['#00ffc8', '#00d9ff', '#7000ff', '#ff00ff', '#00ff88']
  },
  {
    name: 'Desert Night',
    colors: ['#f4a261', '#e76f51', '#264653', '#2a9d8f', '#e9c46a']
  },
  {
    name: 'Neon Tokyo',
    colors: ['#ff0080', '#ff8c00', '#00ffff', '#7fff00', '#ff1493', '#00ff7f']
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

// Ripple effect class
class Ripple {
  constructor(x, y, intensity = 1) {
    this.x = x;
    this.y = y;
    this.radius = 0;
    this.maxRadius = 200 + intensity * 100;
    this.speed = 3 + intensity * 2;
    this.life = 1.0;
    this.intensity = intensity;
  }

  update() {
    this.radius += this.speed;
    this.life = 1 - (this.radius / this.maxRadius);
  }

  getDistortion(x, y) {
    const dx = x - this.x;
    const dy = y - this.y;
    const distance = Math.sqrt(dx * dx + dy * dy);
    
    if (distance > this.radius + 50 || distance < this.radius - 50) {
      return { dx: 0, dy: 0, scale: 1 };
    }
    
    // Create wave distortion at ripple edge
    const distFromEdge = Math.abs(distance - this.radius);
    const strength = (1 - distFromEdge / 50) * this.life * 30;
    
    const angle = Math.atan2(dy, dx);
    const perpAngle = angle + Math.PI / 2;
    
    return {
      dx: Math.cos(perpAngle) * strength,
      dy: Math.sin(perpAngle) * strength,
      scale: 1 + (this.life * 0.1 * Math.cos(distFromEdge * 0.5))
    };
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

// Pattern Definitions
const patterns = [
  { name: 'Diamond Lattice', draw: drawDiamondLattice },
  { name: 'Hex Flowers', draw: drawHexFlowers },
  { name: 'Concentric Waves', draw: drawConcentricWaves },
  { name: 'Spiral Galaxy', draw: drawSpiralGalaxy },
  { name: 'Flowing Rivers', draw: drawFlowingRivers },  // NEW
  { name: 'Dot Matrix', draw: drawDotMatrix }           // NEW
];

const currentPatternName = ref(patterns[0].name);
const currentPaletteName = ref(palettes[0].name);

// Music player handlers
function handleTrackChange(index) {
  currentTrackIndex.value = index;
  loadTrack(index);
}

function nextTrack() {
  const nextIndex = (currentTrackIndex.value + 1) % tracks.value.length;
  handleTrackChange(nextIndex);
}

function previousTrack() {
  const prevIndex = (currentTrackIndex.value - 1 + tracks.value.length) % tracks.value.length;
  handleTrackChange(prevIndex);
}

function loadTrack(index) {
  if (!useMusicPlayer.value) return;
  
  const track = tracks.value[index];
  fileName.value = track.name;
  
  // Stop current audio if playing
  if (audioElement) {
    audioElement.pause();
    audioElement = null;
  }
  
  // Create new audio element
  audioElement = new Audio(track.url);
  audioElement.crossOrigin = "anonymous";
  
  // Auto-play the track
  audioElement.play().then(() => {
    setupAudioContext(audioElement);
    audioLoaded.value = true;
    isPaused.value = false;
  }).catch(err => {
    console.error('Error playing track:', err);
    // If auto-play fails, just load it ready to play
    setupAudioContext(audioElement);
    audioLoaded.value = true;
    isPaused.value = true;
  });
  
  // Handle track end - auto-advance to next track
  audioElement.addEventListener('ended', () => {
    nextTrack();
  });
}

// function loadAudio(event) {
//   const file = event.target.files[0];
//   if (!file) return;

//   fileName.value = file.name;
//   audioElement = new Audio();
//   const reader = new FileReader();

//   reader.onload = (e) => {
//     audioElement.src = e.target.result;
//     audioElement.play();
//     setupAudioContext(audioElement);
//     audioLoaded.value = true;
//     isPaused.value = false;
//   };

//   reader.readAsDataURL(file);
// }

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

function applyWarpDistortion(x, y) {
  if (!mouseX || !mouseY) return { x, y, scale: 1 };
  
  let totalDx = 0;
  let totalDy = 0;
  let totalScale = 1;
  
  // Apply ripple distortions
  ripples.forEach(ripple => {
    const distortion = ripple.getDistortion(x, y);
    totalDx += distortion.dx;
    totalDy += distortion.dy;
    totalScale *= distortion.scale;
  });
  
  // Apply mouse warp (gravitational lens effect)
  if (warpIntensity > 0.01) {
    const dx = x - mouseX;
    const dy = y - mouseY;
    const distance = Math.sqrt(dx * dx + dy * dy);
    const maxDistance = 300;
    
    if (distance < maxDistance) {
      const strength = (1 - distance / maxDistance) * warpIntensity;
      const angle = Math.atan2(dy, dx);
      
      if (isMouseDown) {
        // Pull towards mouse when dragging
        totalDx -= Math.cos(angle) * strength * 40;
        totalDy -= Math.sin(angle) * strength * 40;
        totalScale *= 1 + strength * 0.3;
      } else {
        // Gentle warp around mouse
        const perpAngle = angle + Math.PI / 2;
        totalDx += Math.cos(perpAngle) * strength * 15;
        totalDy += Math.sin(perpAngle) * strength * 15;
      }
    }
  }
  
  return {
    x: x + totalDx,
    y: y + totalDy,
    scale: totalScale
  };
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
      const baseX = (i - 1) * size + size / 2;
      const baseY = (j - 1) * size + size / 2;
      const index = i + j * cols;
      const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.15) * 0.3 + 0.5;
      
      // Apply warp distortion
      const warped = applyWarpDistortion(baseX, baseY);
      const x = warped.x;
      const y = warped.y;
      
      const octagonSize = size * 0.35 * (0.7 + value * 0.5) * warped.scale;
      const diamondSize = size * 0.25 * (0.8 + value * 0.4) * warped.scale;

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
      const baseX = (i - 1) * hexRadius * 1.5;
      const baseY = (j - 1) * hexHeight + (i % 2) * hexHeight / 2;
      const index = i + j * cols;
      const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.12) * 0.3 + 0.5;
      
      // Apply warp distortion
      const warped = applyWarpDistortion(baseX, baseY);
      const x = warped.x;
      const y = warped.y;
      
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
        const size = hexRadius * p.scale * (0.7 + value * 0.5) * warped.scale;
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
      const baseX = (i - 1) * size + size / 2;
      const baseY = (j - 1) * size + size / 2;
      const index = i + j * cols;
      const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.1) * 0.3 + 0.5;
      
      // Apply warp distortion
      const warped = applyWarpDistortion(baseX, baseY);
      const x = warped.x;
      const y = warped.y;
      
      const rings = 8;
      const maxRadius = size * 0.45 * warped.scale;

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

function drawFlowingRivers() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const rivers = 8;
  const segments = 40;

  for (let r = 0; r < rivers; r++) {
    const startY = (r / rivers) * height;
    const dataOffset = Math.floor((r / rivers) * bufferLength);
    
    ctx.save();
    
    // Draw flowing bezier curve
    const points = [];
    for (let s = 0; s <= segments; s++) {
      const t = s / segments;
      const x = t * width;
      const dataIndex = (dataOffset + Math.floor(t * bufferLength / 4)) % bufferLength;
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + t * Math.PI * 4 + r) * 0.3 + 0.5;
      
      // Create wave motion
      const waveOffset = Math.sin(t * Math.PI * 3 + rotationAngle * 2 + r * 0.5) * 80;
      const audioOffset = (value - 0.5) * 120;
      const y = startY + waveOffset + audioOffset;
      
      points.push({ x, y, value });
    }

    // Draw river with varying thickness
    for (let i = 0; i < points.length - 1; i++) {
      const p1 = points[i];
      const p2 = points[i + 1];
      const avgValue = (p1.value + p2.value) / 2;
      
      // Apply warp to river points
      const warped1 = applyWarpDistortion(p1.x, p1.y);
      const warped2 = applyWarpDistortion(p2.x, p2.y);
      
      const thickness = 3 + avgValue * 25;
      
      // Glow layer
      if (avgValue > 0.6) {
        ctx.strokeStyle = getColor(r * 50 + i * 3, rivers * segments, avgValue * 0.3);
        ctx.lineWidth = thickness * 2.5;
        ctx.shadowBlur = 20;
        ctx.shadowColor = getColor(r * 50 + i * 3, rivers * segments, avgValue);
        ctx.lineCap = 'round';
        ctx.beginPath();
        ctx.moveTo(warped1.x, warped1.y);
        ctx.lineTo(warped2.x, warped2.y);
        ctx.stroke();
        ctx.shadowBlur = 0;
      }
      
      // Main river line
      ctx.strokeStyle = getColor(r * 50 + i * 3, rivers * segments, avgValue);
      ctx.lineWidth = thickness;
      ctx.lineCap = 'round';
      ctx.beginPath();
      ctx.moveTo(warped1.x, warped1.y);
      ctx.lineTo(warped2.x, warped2.y);
      ctx.stroke();
      
      // Spawn particles at high energy points
      if (audioLoaded.value && avgValue > 0.8 && Math.random() > 0.92) {
        createParticles(warped1.x, warped1.y, avgValue, r * segments + i, rivers * segments, 1);
      }
      
      // Add bright dots at peaks
      if (avgValue > 0.75) {
        ctx.fillStyle = getColor(r * 50 + i * 3 + 20, rivers * segments, 1);
        ctx.shadowBlur = 10;
        ctx.shadowColor = getColor(r * 50 + i * 3 + 20, rivers * segments, 1);
        ctx.beginPath();
        ctx.arc(warped1.x, warped1.y, 3 + avgValue * 4, 0, Math.PI * 2);
        ctx.fill();
        ctx.shadowBlur = 0;
      }
    }
    
    ctx.restore();
  }
}

function drawDotMatrix() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const spacing = 35;
  const cols = Math.ceil(width / spacing) + 1;
  const rows = Math.ceil(height / spacing) + 1;

  ctx.save();
  
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      const baseX = i * spacing;
      const baseY = j * spacing;
      const index = i + j * cols;
      
      // Use different frequency ranges across the grid
      const freqZone = Math.floor((i / cols) * 4);
      const dataIndex = Math.floor((freqZone / 4) * bufferLength + (j / rows) * (bufferLength / 4));
      const value = audioLoaded.value ? dataArray[dataIndex % bufferLength] / 255 : Math.sin(breathePhase + i * 0.2 + j * 0.15) * 0.3 + 0.5;
      
      // Apply warp distortion
      const warped = applyWarpDistortion(baseX, baseY);
      const x = warped.x;
      const y = warped.y;
      
      // Dot size based on audio
      const baseSize = 4;
      const size = baseSize + value * 12;
      
      // Spawn particles at very high energy dots
      if (audioLoaded.value && value > 0.85 && Math.random() > 0.97) {
        createParticles(x, y, value, index, cols * rows, 1);
      }
      
      // Glow effect for high energy
      if (value > 0.6) {
        const glowSize = size * 3;
        const gradient = ctx.createRadialGradient(x, y, 0, x, y, glowSize);
        gradient.addColorStop(0, getColor(index, cols * rows, value * 0.4));
        gradient.addColorStop(1, 'rgba(0,0,0,0)');
        ctx.fillStyle = gradient;
        ctx.beginPath();
        ctx.arc(x, y, glowSize, 0, Math.PI * 2);
        ctx.fill();
      }
      
      // Main dot
      ctx.fillStyle = getColor(index, cols * rows, value * 0.9);
      ctx.beginPath();
      ctx.arc(x, y, size, 0, Math.PI * 2);
      ctx.fill();
      
      // Add ring for very high energy
      if (value > 0.75) {
        ctx.strokeStyle = getColor(index + 50, cols * rows, 1);
        ctx.lineWidth = 1.5;
        ctx.shadowBlur = 8;
        ctx.shadowColor = getColor(index + 50, cols * rows, value);
        ctx.beginPath();
        ctx.arc(x, y, size + 4, 0, Math.PI * 2);
        ctx.stroke();
        ctx.shadowBlur = 0;
      }
      
      // Connect neighboring dots with lines when both are high energy
      if (i < cols - 1 && value > 0.65) {
        // const nextIndex = (i + 1) + j * cols;
        const nextDataIndex = Math.floor((freqZone / 4) * bufferLength + (j / rows) * (bufferLength / 4) + spacing);
        const nextValue = audioLoaded.value ? dataArray[nextDataIndex % bufferLength] / 255 : value;
        
        if (nextValue > 0.65) {
          const nextWarped = applyWarpDistortion((i + 1) * spacing, baseY);
          ctx.strokeStyle = getColor(index, cols * rows, Math.min(value, nextValue) * 0.3);
          ctx.lineWidth = 1;
          ctx.beginPath();
          ctx.moveTo(x, y);
          ctx.lineTo(nextWarped.x, nextWarped.y);
          ctx.stroke();
        }
      }
      
      // Vertical connections
      if (j < rows - 1 && value > 0.65) {
        // const nextIndex = i + (j + 1) * cols;
        const nextDataIndex = Math.floor((freqZone / 4) * bufferLength + ((j + 1) / rows) * (bufferLength / 4));
        const nextValue = audioLoaded.value ? dataArray[nextDataIndex % bufferLength] / 255 : value;
        
        if (nextValue > 0.65) {
          const nextWarped = applyWarpDistortion(baseX, (j + 1) * spacing);
          ctx.strokeStyle = getColor(index, cols * rows, Math.min(value, nextValue) * 0.3);
          ctx.lineWidth = 1;
          ctx.beginPath();
          ctx.moveTo(x, y);
          ctx.lineTo(nextWarped.x, nextWarped.y);
          ctx.stroke();
        }
      }
    }
  }
  
  ctx.restore();
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


  // Draw current pattern
  patterns[currentPatternIndex.value].draw();

  // Update and draw particles
  updateParticles();
  drawParticles();

  // Update and clean up ripples
  ripples = ripples.filter(r => !r.isDead());
  ripples.forEach(r => r.update());

  // Smooth warp intensity
  warpIntensity += (targetWarpIntensity - warpIntensity) * 0.1;

  // Draw soundwaves on top  // Draw ripple effects
  ripples.forEach(ripple => {
    ctx.strokeStyle = 'solid';
    ctx.lineWidth = 3;
    ctx.shadowBlur = 10;
    ctx.shadowColor = 'red';
    ctx.beginPath();
    ctx.arc(ripple.x, ripple.y, ripple.radius, 0, Math.PI * 2);
    ctx.stroke();
    ctx.shadowBlur = 0;
  });

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

function resize() {
  canvas.value.width = window.innerWidth;
  canvas.value.height = window.innerHeight;
}

// Mouse/Touch interaction handlers
function handleMouseMove(e) {
  const rect = canvas.value.getBoundingClientRect();
  mouseX = e.clientX - rect.left;
  mouseY = e.clientY - rect.top;
  targetWarpIntensity = 1;
}

function handleMouseDown(e) {
  isMouseDown = true;
  const rect = canvas.value.getBoundingClientRect();
  mouseX = e.clientX - rect.left;
  mouseY = e.clientY - rect.top;
}

function handleMouseUp() {
  isMouseDown = false;
}

function handleMouseLeave() {
  targetWarpIntensity = 0;
  isMouseDown = false;
}

function handleClick(e) {
  const rect = canvas.value.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  
  // Create ripple at click location
  ripples.push(new Ripple(x, y, 1.5));
  
  // Spawn some particles
  if (audioLoaded.value) {
    createParticles(x, y, 0.8, Math.floor(Math.random() * 100), 100, 5);
  }
}

function handleTouchStart(e) {
  e.preventDefault();
  const rect = canvas.value.getBoundingClientRect();
  const touch = e.touches[0];
  mouseX = touch.clientX - rect.left;
  mouseY = touch.clientY - rect.top;
  isMouseDown = true;
  targetWarpIntensity = 1;
}

function handleTouchMove(e) {
  e.preventDefault();
  const rect = canvas.value.getBoundingClientRect();
  const touch = e.touches[0];
  mouseX = touch.clientX - rect.left;
  mouseY = touch.clientY - rect.top;
  
  // Create ripples along drag path
  if (Math.random() > 0.8) {
    ripples.push(new Ripple(mouseX, mouseY, 0.8));
  }
}

function handleTouchEnd(e) {
  e.preventDefault();
  isMouseDown = false;
  targetWarpIntensity = 0;
  
  // Create final ripple on release
  if (mouseX && mouseY) {
    ripples.push(new Ripple(mouseX, mouseY, 1.5));
  }
}

// Keyboard controls
function handleKeyDown(e) {
  switch(e.key) {
    case 'ArrowRight':
      cyclePattern();
      break;
    case 'ArrowLeft':
      currentPatternIndex.value = (currentPatternIndex.value - 1 + patterns.length) % patterns.length;
      currentPatternName.value = patterns[currentPatternIndex.value].name;
      break;
    case 'ArrowUp':
      cyclePalette();
      break;
    case 'ArrowDown':
      currentPaletteIndex.value = (currentPaletteIndex.value - 1 + palettes.length) % palettes.length;
      currentPaletteName.value = palettes[currentPaletteIndex.value].name;
      break;
  }
}

onMounted(() => {
  ctx = canvas.value.getContext('2d');
  resize();
  window.addEventListener('resize', resize);
  window.addEventListener('keydown', handleKeyDown);  // Add this line
  
  // Mouse events
  canvas.value.addEventListener('mousemove', handleMouseMove);
  canvas.value.addEventListener('mousedown', handleMouseDown);
  canvas.value.addEventListener('mouseup', handleMouseUp);
  canvas.value.addEventListener('mouseleave', handleMouseLeave);
  canvas.value.addEventListener('click', handleClick);
  
  // Touch events
  canvas.value.addEventListener('touchstart', handleTouchStart);
  canvas.value.addEventListener('touchmove', handleTouchMove);
  canvas.value.addEventListener('touchend', handleTouchEnd);
  
  animate();
  
  // Auto-load first track if using music player
  if (useMusicPlayer.value) {
    loadTrack(0);
  }
});

onUnmounted(() => {
  window.removeEventListener('resize', resize);
  window.removeEventListener('keydown', handleKeyDown);
  
  // Remove mouse events
  canvas.value.removeEventListener('mousemove', handleMouseMove);
  canvas.value.removeEventListener('mousedown', handleMouseDown);
  canvas.value.removeEventListener('mouseup', handleMouseUp);
  canvas.value.removeEventListener('mouseleave', handleMouseLeave);
  canvas.value.removeEventListener('click', handleClick);
  
  // Remove touch events
  canvas.value.removeEventListener('touchstart', handleTouchStart);
  canvas.value.removeEventListener('touchmove', handleTouchMove);
  canvas.value.removeEventListener('touchend', handleTouchEnd);
  
  if (animationId) cancelAnimationFrame(animationId);
  if (audioContext) audioContext.close();
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

.visualizer-app {
  font-family: 'Inter', sans-serif;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  background: #0a0a0a;
  color: #fff;
  overflow: hidden;
  position: relative;
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
  padding: 8px 10px;
  border-radius: 50px;
  border: 5px solid black;
  background: rgba(20, 20, 20, 0.6);
  backdrop-filter: blur(20px) saturate(150%);
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
  border: 1px solid rgba(0, 0, 0, 0.8);
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
  position: absolute;
  overflow: hidden;
}

canvas {
  display: block;
  width: 100%;
  height: 100%;
  cursor: crosshair;
  touch-action: none;
}

.info {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(20, 20, 20, 0.6);
  backdrop-filter: blur(20px) saturate(150%);
  padding: 10px 20px;
  border-radius: 20px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  font-size: 13px;
  color: rgba(255, 255, 255, 0.7);
  width: 400px;
  display: flex;
  justify-content: center;
}

@keyframes breathe {
  0%, 100% { opacity: 0.6; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.02); }
}

.breathing {
  animation: breathe 3s ease-in-out infinite;
}
</style>
