<template>
  <div class="visualizer-app">
    
    <!-- Play Modal -->
    <div v-if="showPlayModal" class="modal-overlay">
      <div class="modal-content">
        <!-- <div class="modal-logo">ONE WAX WING</div>
         -->
          <svg id="oww-typog-modal" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1435.21 160.32">
          <g id="back-text">
            <path class="oww-1" d="M12.45,142.6V39.8h15.63v-16.67h94.3v16.67h16.5v102.81h-16.5v16.67H28.08v-16.67h-15.63ZM55.34,123.85h39.59V59.59h-39.59v64.25Z" />
            <path class="oww-1" d="M196.17,23.13v16.67h16.15v15.98h17.19v16.67h6.43V23.13h42.2v136.15h-42.2v-16.67h-17.19v-15.98h-17.19v-16.67h-6.43v49.32h-43.41V23.13h44.46Z" />
            <path class="oww-1" d="M395.18,23.13v35.95h-58v13.89h58v36.47h-58v13.89h58v35.95h-102.98V23.13h102.98Z" />
            <path class="oww-1" d="M538.28,23.13v57.83h6.43v-14.93h26.74v14.93h7.64V23.13h42.2v136.15h-42.2v-16.67h-17.19v-15.46h-6.43v15.46h-17.19v16.67h-43.41V23.13h43.41Z" />
            <path class="oww-1" d="M739.37,23.13v136.15h-44.46v-36.47h-15.11v36.47h-44.46V23.13h104.02ZM680.33,89.64h14.07v-34.38h-14.07v34.38Z" />
            <path
              class="oww-1"
              d="M769.93,111.52v-12.85h12.33v-14.93h-12.33v-12.85h-16.67V23.13h44.46v16.67h12.85v12.33h13.89v-12.33h13.02v-16.67h44.46v47.76h-16.67v12.85h-12.33v14.93h12.33v12.85h16.67v47.76h-44.46v-16.67h-13.02v-12.33h-13.89v12.33h-12.85v16.67h-44.46v-47.76h16.67Z"
            />
            <path class="oww-1" d="M1025.03,23.13v57.83h6.42v-14.93h26.74v14.93h7.64V23.13h42.2v136.15h-42.2v-16.67h-17.19v-15.46h-6.42v15.46h-17.19v16.67h-43.41V23.13h43.41Z" />
            <path class="oww-1" d="M1165,23.13v136.15h-42.89V23.13h42.89Z" />
            <path class="oww-1" d="M1223.35,23.13v16.67h16.15v15.98h17.19v16.67h6.43V23.13h42.2v136.15h-42.2v-16.67h-17.19v-15.98h-17.19v-16.67h-6.42v49.32h-43.41V23.13h44.46Z" />
            <path class="oww-1" d="M1319.38,143.64V40.84h16.67v-16.67h99.16v35.95h-70.85v63.73h34.91v-24.66h35.95v61.13h-99.16v-16.67h-16.67Z" />
          </g>
          <g id="fore-text">
            <path d="M0,119.48V16.67h15.63V0h94.3v16.67h16.5v102.81h-16.5v16.67H15.63v-16.67H0ZM42.89,100.72h39.59V36.47h-39.59v64.25Z" />
            <path d="M183.73,0v16.67h16.15v15.98h17.19v16.67h6.43V0h42.2v136.15h-42.2v-16.67h-17.19v-15.98h-17.19v-16.67h-6.43v49.32h-43.41V0h44.46Z" />
            <path d="M382.74,0v35.95h-58v13.89h58v36.47h-58v13.89h58v35.95h-102.98V0h102.98Z" />
            <path d="M525.83,0v57.83h6.43v-14.93h26.74v14.93h7.64V0h42.2v136.15h-42.2v-16.67h-17.19v-15.46h-6.43v15.46h-17.19v16.67h-43.41V0h43.41Z" />
            <path d="M726.92,0v136.15h-44.46v-36.47h-15.11v36.47h-44.46V0h104.02ZM667.88,66.51h14.07v-34.38h-14.07v34.38Z" />
            <path
              d="M757.49,88.39v-12.85h12.33v-14.93h-12.33v-12.85h-16.67V0h44.46v16.67h12.85v12.33h13.89v-12.33h13.02V0h44.46v47.76h-16.67v12.85h-12.33v14.93h12.33v12.85h16.67v47.76h-44.46v-16.67h-13.02v-12.33h-13.89v12.33h-12.85v16.67h-44.46v-47.76h16.67Z"
            />
            <path d="M1012.59,0v57.83h6.42v-14.93h26.74v14.93h7.64V0h42.2v136.15h-42.2v-16.67h-17.19v-15.46h-6.42v15.46h-17.19v16.67h-43.41V0h43.41Z" />
            <path d="M1152.55,0v136.15h-42.89V0h42.89Z" />
            <path d="M1210.9,0v16.67h16.15v15.98h17.19v16.67h6.43V0h42.2v136.15h-42.2v-16.67h-17.19v-15.98h-17.19v-16.67h-6.42v49.32h-43.41V0h44.46Z" />
            <path d="M1306.93,120.52V17.71h16.67V1.04h99.16v35.95h-70.85v63.73h34.91v-24.66h35.95v61.13h-99.16v-16.67h-16.67Z" />
          </g>
        </svg>
        <div class="modal-subtitle">Debut Album - "Let Slip" - Available Everywhere</div>
        <button class="play-modal-btn" @click="startPlayback">
          ▶ Play "Let Slip"
        </button>
      </div>
    </div>

    <h1>
      <svg id="oww-typog" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1435.21 160.32">
        <g id="back-text">
          <path class="oww-1" d="M12.45,142.6V39.8h15.63v-16.67h94.3v16.67h16.5v102.81h-16.5v16.67H28.08v-16.67h-15.63ZM55.34,123.85h39.59V59.59h-39.59v64.25Z" />
          <path class="oww-1" d="M196.17,23.13v16.67h16.15v15.98h17.19v16.67h6.43V23.13h42.2v136.15h-42.2v-16.67h-17.19v-15.98h-17.19v-16.67h-6.43v49.32h-43.41V23.13h44.46Z" />
          <path class="oww-1" d="M395.18,23.13v35.95h-58v13.89h58v36.47h-58v13.89h58v35.95h-102.98V23.13h102.98Z" />
          <path class="oww-1" d="M538.28,23.13v57.83h6.43v-14.93h26.74v14.93h7.64V23.13h42.2v136.15h-42.2v-16.67h-17.19v-15.46h-6.43v15.46h-17.19v16.67h-43.41V23.13h43.41Z" />
          <path class="oww-1" d="M739.37,23.13v136.15h-44.46v-36.47h-15.11v36.47h-44.46V23.13h104.02ZM680.33,89.64h14.07v-34.38h-14.07v34.38Z" />
          <path
            class="oww-1"
            d="M769.93,111.52v-12.85h12.33v-14.93h-12.33v-12.85h-16.67V23.13h44.46v16.67h12.85v12.33h13.89v-12.33h13.02v-16.67h44.46v47.76h-16.67v12.85h-12.33v14.93h12.33v12.85h16.67v47.76h-44.46v-16.67h-13.02v-12.33h-13.89v12.33h-12.85v16.67h-44.46v-47.76h16.67Z"
          />
          <path class="oww-1" d="M1025.03,23.13v57.83h6.42v-14.93h26.74v14.93h7.64V23.13h42.2v136.15h-42.2v-16.67h-17.19v-15.46h-6.42v15.46h-17.19v16.67h-43.41V23.13h43.41Z" />
          <path class="oww-1" d="M1165,23.13v136.15h-42.89V23.13h42.89Z" />
          <path class="oww-1" d="M1223.35,23.13v16.67h16.15v15.98h17.19v16.67h6.43V23.13h42.2v136.15h-42.2v-16.67h-17.19v-15.98h-17.19v-16.67h-6.42v49.32h-43.41V23.13h44.46Z" />
          <path class="oww-1" d="M1319.38,143.64V40.84h16.67v-16.67h99.16v35.95h-70.85v63.73h34.91v-24.66h35.95v61.13h-99.16v-16.67h-16.67Z" />
        </g>
        <g id="fore-text">
          <path d="M0,119.48V16.67h15.63V0h94.3v16.67h16.5v102.81h-16.5v16.67H15.63v-16.67H0ZM42.89,100.72h39.59V36.47h-39.59v64.25Z" />
          <path d="M183.73,0v16.67h16.15v15.98h17.19v16.67h6.43V0h42.2v136.15h-42.2v-16.67h-17.19v-15.98h-17.19v-16.67h-6.43v49.32h-43.41V0h44.46Z" />
          <path d="M382.74,0v35.95h-58v13.89h58v36.47h-58v13.89h58v35.95h-102.98V0h102.98Z" />
          <path d="M525.83,0v57.83h6.43v-14.93h26.74v14.93h7.64V0h42.2v136.15h-42.2v-16.67h-17.19v-15.46h-6.43v15.46h-17.19v16.67h-43.41V0h43.41Z" />
          <path d="M726.92,0v136.15h-44.46v-36.47h-15.11v36.47h-44.46V0h104.02ZM667.88,66.51h14.07v-34.38h-14.07v34.38Z" />
          <path
            d="M757.49,88.39v-12.85h12.33v-14.93h-12.33v-12.85h-16.67V0h44.46v16.67h12.85v12.33h13.89v-12.33h13.02V0h44.46v47.76h-16.67v12.85h-12.33v14.93h12.33v12.85h16.67v47.76h-44.46v-16.67h-13.02v-12.33h-13.89v12.33h-12.85v16.67h-44.46v-47.76h16.67Z"
          />
          <path d="M1012.59,0v57.83h6.42v-14.93h26.74v14.93h7.64V0h42.2v136.15h-42.2v-16.67h-17.19v-15.46h-6.42v15.46h-17.19v16.67h-43.41V0h43.41Z" />
          <path d="M1152.55,0v136.15h-42.89V0h42.89Z" />
          <path d="M1210.9,0v16.67h16.15v15.98h17.19v16.67h6.43V0h42.2v136.15h-42.2v-16.67h-17.19v-15.98h-17.19v-16.67h-6.42v49.32h-43.41V0h44.46Z" />
          <path d="M1306.93,120.52V17.71h16.67V1.04h99.16v35.95h-70.85v63.73h34.91v-24.66h35.95v61.13h-99.16v-16.67h-16.67Z" />
        </g>
      </svg>
    </h1>

    <div class="triangle" />
    <div class="triangle tri2" />
    
    <div class="controls">
      <button class="arrow-btn" @click="cyclePreviousPattern" title="Previous pattern">←</button>
      <button 
        class="pattern-btn" 
        :class="{ locked: patternLocked }"
        @click="togglePatternLock"
      >
        <span v-if="patternLocked" class="lock-icon">🔒</span>
        Pattern: {{ currentPatternName }}
      </button>
      <button class="arrow-btn" @click="cyclePattern" title="Next pattern">→</button>
      <button class="palette-btn" @click="cyclePalette">Palette: {{ currentPaletteName }}</button>
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
      <span class="hint"> Use ← → to change Pattern | ↑ ↓ to change Colors | SPACE to lock Pattern</span>
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
import { ref, onMounted, onUnmounted, computed } from 'vue';
import MusicPlayer from './MusicPlayer.vue';
import Seraphim from './Seraphim.vue';

const canvas = ref(null);
const audioLoaded = ref(false);
const fileName = ref('');
const currentPatternIndex = ref(0);
const currentPaletteIndex = ref(0);
const isPaused = ref(false);
const patternLocked = ref(false);
const typographyColorIndex = ref(0);
const showPlayModal = ref(true);

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
const patternRotateInterval = ref(15000); // 15 seconds per pattern
const paletteRotateInterval = ref(8000); // 8 seconds per palette
let patternTimer = null;
let paletteTimer = null;

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
    name: 'Rainblow',
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
  {
    name: 'Midnight Purple',
    colors: ['#1a1a2e', '#16213e', '#0f3460', '#533483', '#e94560']
  },
  {
    name: 'Tropicali',
    colors: ['#06d6a0', '#118ab2', '#073b4c', '#ef476f', '#ffd166']
  },
  {
    name: 'Candy Shop',
    colors: ['#ff006e', '#fb5607', '#ffbe0b', '#8338ec', '#3a86ff']
  },
  {
    name: 'Deep Space',
    colors: ['#0d1b2a', '#1b263b', '#415a77', '#778da9', '#e0e1dd', '#FF0000']
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
    name: 'Borealis',
    colors: ['#00ffc8', '#00d9ff', '#7000ff', '#ff00ff', '#00ff88']
  },
  {
    name: 'Desert Night',
    colors: ['#f4a261', '#e76f51', '#264653', '#2a9d8f', '#e9c46a']
  },
  {
    name: 'Neon Tokyo',
    colors: ['#ff0080', '#ff8c00', '#00ffff', '#7fff00', '#ff1493', '#00ff7f']
  },
  {
    name: 'Infrared',
    colors: ['#ff0000', '#cc0000', '#990000', '#ff3333', '#ff6666']
  },
  {
    name: 'Ultraviolet',
    colors: ['#9d00ff', '#7000ff', '#5000cc', '#b366ff', '#cc99ff']
  },
  {
    name: 'Toxic Slime',
    colors: ['#39ff14', '#7fff00', '#adff2f', '#00ff00', '#32cd32']
  },
  {
    name: 'Ice Cave',
    colors: ['#00ced1', '#00bfff', '#1e90ff', '#87ceeb', '#b0e0e6']
  },
  {
    name: 'Synthwave Nights',
    colors: ['#ff00ff', '#ff006e', '#8338ec', '#3a0ca3', '#4cc9f0']
  },
  {
    name: 'Nebula',
    colors: ['#9d4edd', '#7209b7', '#5a189a', '#240046', '#ff006e']
  },
  {
    name: 'Radioactive',
    colors: ['#ccff00', '#ffff00', '#ff9900', '#ff0000', '#00ff00']
  },
  {
  name: 'Blue Flame',
  colors: ['#0066ff', '#0099ff', '#ff6600', '#ff8c00', '#ffa500', '#00bfff']
  },
  {
  name: 'Tropical Storm',
  colors: ['#ffff00', '#ffd700', '#00ff00', '#00ff7f', '#0099ff', '#00bfff', '#7fff00']
  },
  {
  name: 'Taxi Cab',
  colors: ['#ffff00', '#ffd700', '#000000', '#ffffff', '#ffcc00']
  }
];

function togglePatternLock() {
  patternLocked.value = !patternLocked.value;
}

function getRandomPatternIndex() {
  return Math.floor(Math.random() * patterns.length);
}

const currentTypographyColor = computed(() => {
  const palette = palettes[currentPaletteIndex.value].colors;
  return palette[typographyColorIndex.value % palette.length];
});

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

function startPlayback() {
  showPlayModal.value = false;
  if (useMusicPlayer.value) {
    loadTrack(0);
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
  { name: 'Spkrwall', draw: drawPlasmaStorm },
  { name: 'Aurora Waves', draw: drawAuroraWaves },
  { name: 'Diamond Lattice', draw: drawDiamondLattice },
  { name: 'Hex Meadow', draw: drawHexFlowers },
  { name: 'Wavepools', draw: drawConcentricWaves },
  { name: 'Undertide', draw: drawFlowingRivers },
  { name: 'Dot Matrix', draw: drawDotMatrix },
  { name: 'Osmosis', draw: drawLiquidCrystals },
  { name: 'Petri', draw: drawEnergyShards },
  { name: 'Boombox', draw: drawBoombox },
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

function drawAuroraWaves() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const waveCount = audioLoaded.value && !isPaused.value ? 15 : 8; // Fewer waves when no audio
  const segments = 80;

  ctx.save();
  
  for (let w = 0; w < waveCount; w++) {
    const waveOffset = (w / waveCount) * height;
    const dataOffset = Math.floor((w / waveCount) * bufferLength);
    
    // Create wave path points
    const points = [];
    for (let s = 0; s <= segments; s++) {
      const t = s / segments;
      const x = t * width;
      const dataIndex = (dataOffset + Math.floor(t * bufferLength / 3)) % bufferLength;
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + t * Math.PI * 2 + w * 0.3) * 0.3 + 0.5;
      
      // Create multiple sine waves layered together for aurora effect
      const wave1 = Math.sin(t * Math.PI * 3 + rotationAngle * 1.5 + w * 0.4) * 60;
      const wave2 = Math.sin(t * Math.PI * 5 + rotationAngle * 0.8 + w * 0.6) * 30;
      const wave3 = Math.sin(t * Math.PI * 7 + rotationAngle * 0.5 + w * 0.2) * 15;
      const audioWave = (value - 0.5) * 140;
      
      const y = waveOffset + wave1 + wave2 + wave3 + audioWave;
      
      points.push({ x, y, value });
    }
    
    // Only draw filled layers when audio is playing
    if (audioLoaded.value && !isPaused.value) {
      const layers = 7;
      for (let layer = layers - 1; layer >= 0; layer--) {
        const layerOffset = (layer - layers / 2) * 6;
        const layerAlpha = (layers - layer) / layers * 0.5;
        
        // Create gradient for aurora colors
        const gradient = ctx.createLinearGradient(0, 0, width, 0);
        const colorShift = w * 50 + layer * 30;
        
        for (let g = 0; g <= 5; g++) {
          const stop = g / 5;
          const colorIdx = Math.floor((stop * 1000 + colorShift + rotationAngle * 100) % 1000);
          gradient.addColorStop(stop, getColor(colorIdx, 1000, 0.05 * layerAlpha));
        }
        
        ctx.fillStyle = gradient;
        
        // Draw filled wave shape
        ctx.beginPath();
        points.forEach((point, idx) => {
          const x = point.x;
          const y = point.y + layerOffset;
          
          if (idx === 0) {
            ctx.moveTo(x, y);
          } else {
            const prevPoint = points[idx - 1];
            const cpx = (prevPoint.x + x) / 2;
            const cpy = (prevPoint.y + y) / 2 + layerOffset;
            ctx.quadraticCurveTo(prevPoint.x, prevPoint.y + layerOffset, cpx, cpy);
          }
        });
        
        ctx.lineTo(width, height);
        ctx.lineTo(0, height);
        ctx.closePath();
        ctx.fill();
        
        // Draw subtle glowing edge on some layers
        if (layer === 0 || layer === 3) {
          ctx.strokeStyle = getColor(w * 50, waveCount * 50, 0.3);
          ctx.lineWidth = 1;
          ctx.shadowBlur = 10;
          ctx.shadowColor = getColor(w * 50, waveCount * 50, 0.4);
          
          ctx.beginPath();
          points.forEach((point, idx) => {
            if (idx === 0) {
              ctx.moveTo(point.x, point.y);
            } else {
              const prevPoint = points[idx - 1];
              const cpx = (prevPoint.x + point.x) / 2;
              const cpy = (prevPoint.y + point.y) / 2;
              ctx.quadraticCurveTo(prevPoint.x, prevPoint.y, cpx, cpy);
            }
          });
          ctx.stroke();
          ctx.shadowBlur = 0;
        }
      }
    } else {
      // Minimal mode - just draw a single subtle wave line
      ctx.strokeStyle = getColor(w * 50, waveCount * 50, 0.2); // Very subtle
      ctx.lineWidth = 1;
      ctx.shadowBlur = 0;
      
      ctx.beginPath();
      points.forEach((point, idx) => {
        if (idx === 0) {
          ctx.moveTo(point.x, point.y);
        } else {
          const prevPoint = points[idx - 1];
          const cpx = (prevPoint.x + point.x) / 2;
          const cpy = (prevPoint.y + point.y) / 2;
          ctx.quadraticCurveTo(prevPoint.x, prevPoint.y, cpx, cpy);
        }
      });
      ctx.stroke();
    }
    
    if (audioLoaded.value && !isPaused.value) {
      // Full effects when audio is playing
      
      // Shimmering light particles along wave peaks
      points.forEach((point, idx) => {
        if (point.value > 0.65 && idx % 3 === 0 && Math.random() > 0.75) {
          createParticles(point.x, point.y, point.value, w * segments + idx, waveCount * segments, 2);
        }
        
        // Bright stars/dots at medium-high energy points
        if (point.value > 0.6 && idx % 2 === 0) {
          const starSize = 1.5 + point.value * 3;
          
          // Star glow
          const starGradient = ctx.createRadialGradient(point.x, point.y, 0, point.x, point.y, starSize * 4);
          starGradient.addColorStop(0, getColor(w * 50 + idx * 5, waveCount * segments, point.value * 0.9));
          starGradient.addColorStop(0.3, getColor(w * 50 + idx * 5, waveCount * segments, point.value * 0.5));
          starGradient.addColorStop(1, 'rgba(0,0,0,0)');
          ctx.fillStyle = starGradient;
          ctx.beginPath();
          ctx.arc(point.x, point.y, starSize * 4, 0, Math.PI * 2);
          ctx.fill();
          
          // Bright center dot
          ctx.fillStyle = getColor(w * 50 + idx * 5 + 100, waveCount * segments, 1);
          ctx.shadowBlur = 12;
          ctx.shadowColor = getColor(w * 50 + idx * 5 + 100, waveCount * segments, 0.8);
          ctx.beginPath();
          ctx.arc(point.x, point.y, starSize, 0, Math.PI * 2);
          ctx.fill();
          ctx.shadowBlur = 0;
          
          // Four-pointed star rays
          if (point.value > 0.75) {
            for (let ray = 0; ray < 4; ray++) {
              const angle = (ray / 4) * Math.PI * 2 + rotationAngle * 2;
              const rayLength = starSize * 3;
              
              ctx.strokeStyle = getColor(w * 50 + idx * 5 + 100, waveCount * segments, 0.9);
              ctx.lineWidth = 2;
              ctx.shadowBlur = 10;
              ctx.shadowColor = getColor(w * 50 + idx * 5 + 100, waveCount * segments, 0.7);
              ctx.beginPath();
              ctx.moveTo(point.x, point.y);
              ctx.lineTo(
                point.x + Math.cos(angle) * rayLength,
                point.y + Math.sin(angle) * rayLength
              );
              ctx.stroke();
              ctx.shadowBlur = 0;
            }
          }
        }
        
        // Small ambient dots everywhere for atmosphere
        if (idx % 5 === 0 && Math.random() > 0.6) {
          const dotSize = 0.5 + Math.random() * 1.5;
          ctx.fillStyle = getColor(w * 50 + idx * 3, waveCount * segments, 0.6);
          ctx.shadowBlur = 6;
          ctx.shadowColor = getColor(w * 50 + idx * 3, waveCount * segments, 0.4);
          ctx.beginPath();
          ctx.arc(point.x, point.y, dotSize, 0, Math.PI * 2);
          ctx.fill();
          ctx.shadowBlur = 0;
        }
      });
      
      // Flowing light streaks/connections between points
      for (let i = 0; i < points.length - 1; i++) {
        const p1 = points[i];
        const p2 = points[i + 1];
        
        // Horizontal connections along wave
        if (p1.value > 0.55 && p2.value > 0.55 && Math.random() > 0.5) {
          const avgValue = (p1.value + p2.value) / 2;
          
          // Glowing connecting line
          ctx.strokeStyle = getColor(w * 50 + i * 10, waveCount * segments, avgValue * 0.5);
          ctx.lineWidth = 1 + avgValue * 2;
          ctx.shadowBlur = 8;
          ctx.shadowColor = getColor(w * 50 + i * 10, waveCount * segments, avgValue * 0.4);
          ctx.beginPath();
          ctx.moveTo(p1.x, p1.y);
          ctx.lineTo(p2.x, p2.y);
          ctx.stroke();
          ctx.shadowBlur = 0;
        }
        
        // Vertical light beams
        if (p1.value > 0.65 && Math.random() > 0.6) {
          const beamHeight = 80 + p1.value * 120;
          
          // Vertical light beam
          const beamGradient = ctx.createLinearGradient(p1.x, p1.y, p1.x, p1.y + beamHeight);
          beamGradient.addColorStop(0, getColor(w * 50 + i * 10, waveCount * segments, p1.value * 0.7));
          beamGradient.addColorStop(0.5, getColor(w * 50 + i * 10, waveCount * segments, p1.value * 0.3));
          beamGradient.addColorStop(1, 'rgba(0,0,0,0)');
          
          ctx.strokeStyle = beamGradient;
          ctx.lineWidth = 2 + p1.value * 3;
          ctx.shadowBlur = 12;
          ctx.shadowColor = getColor(w * 50 + i * 10, waveCount * segments, p1.value * 0.5);
          ctx.beginPath();
          ctx.moveTo(p1.x, p1.y);
          ctx.lineTo(p1.x, p1.y + beamHeight);
          ctx.stroke();
          ctx.shadowBlur = 0;
        }
      }
      
      // Cross-wave connections
      if (w > 0 && Math.random() > 0.7) {
        const connectionPoints = 5;
        for (let c = 0; c < connectionPoints; c++) {
          const pointIdx = Math.floor(Math.random() * points.length);
          const point = points[pointIdx];
          
          if (point.value > 0.7) {
            const targetY = point.y - (height / waveCount);
            
            ctx.strokeStyle = getColor(w * 50 + pointIdx * 5, waveCount * segments, point.value * 0.4);
            ctx.lineWidth = 1;
            ctx.shadowBlur = 8;
            ctx.shadowColor = getColor(w * 50 + pointIdx * 5, waveCount * segments, point.value * 0.3);
            ctx.setLineDash([4, 6]);
            ctx.beginPath();
            ctx.moveTo(point.x, point.y);
            ctx.lineTo(point.x + (Math.random() - 0.5) * 40, targetY);
            ctx.stroke();
            ctx.setLineDash([]);
            ctx.shadowBlur = 0;
          }
        }
      }
    } else {
      // Minimal mode - just occasional subtle dots on wave lines
      points.forEach((point, idx) => {
        if (idx % 8 === 0 && Math.random() > 0.65) {
          const dotSize = 1;
          ctx.fillStyle = getColor(w * 50 + idx * 3, waveCount * segments, 0.4);
          ctx.shadowBlur = 4;
          ctx.shadowColor = getColor(w * 50 + idx * 3, waveCount * segments, 0.2);
          ctx.beginPath();
          ctx.arc(point.x, point.y, dotSize, 0, Math.PI * 2);
          ctx.fill();
          ctx.shadowBlur = 0;
        }
      });
    }
  }
  
  // Atmospheric fog/mist effect at bottom - only when audio playing
  if (audioLoaded.value && !isPaused.value) {
    const mistGradient = ctx.createLinearGradient(0, height - 200, 0, height);
    mistGradient.addColorStop(0, 'rgba(0,0,0,0)');
    mistGradient.addColorStop(0.5, getColor(500, 1000, 0.05));
    mistGradient.addColorStop(1, getColor(700, 1000, 0.08));
    ctx.fillStyle = mistGradient;
    ctx.fillRect(0, height - 200, width, 200);
  }
  
  ctx.restore();
}

    
function drawPlasmaStorm() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const orbCount = 20; // Fixed count
  
  ctx.save();
  
  // Generate energy orbs - FIXED grid positions
  const orbs = [];
  let i = 0;
  while (i < orbCount) {
    const dataIndex = Math.floor((i / orbCount) * bufferLength);
    const value = audioLoaded.value && !isPaused.value ? dataArray[dataIndex] / 255 : 0.5;
    
    // Smooth the value to reduce rapid changes
    const smoothValue = value * 0.3 + 0.5; // Range: 0.5 to 0.8
    
    // Determine if this speaker should be "vibrating" (active)
    // Use a slow changing pattern so speakers turn on/off gradually
    const vibratePhase = Math.sin(breathePhase * 0.2 + i * 0.8);
    const isVibrating = audioLoaded.value && !isPaused.value && vibratePhase > -0.3; // 60% active
    
    // RANDOM distribution - only ~1/3 of speakers create rings
    // Use index-based seed for consistent assignment
    const ringsSeed = Math.sin(i * 12.9898 + 78.233) * 43758.5453;
    const createsRings = (ringsSeed - Math.floor(ringsSeed)) < 0.75; // 1/3 probability
    
    orbs.push({
      x: (0.15 + (i % 5) * 0.175) * width, // Grid layout - 5 columns
      y: (0.2 + Math.floor(i / 5) * 0.2) * height, // 4 rows
      radius: 50 + smoothValue * 80,
      value: smoothValue,
      colorIndex: i,
      isVibrating: isVibrating,
      vibratePhase: vibratePhase,
      createsRings: createsRings // NEW - random assignment
    });
    i++;
  }
  
  // MINIMAL MODE when no music
  if (!audioLoaded.value || isPaused.value) {
    orbs.forEach((orb) => {
      const x = orb.x;
      const y = orb.y;
      const radius = orb.radius;
      
      // Very subtle outer glow
      const haloGradient = ctx.createRadialGradient(x, y, radius * 0.5, x, y, radius * 1.2);
      haloGradient.addColorStop(0, getColor(orb.colorIndex * 50, orbCount * 50, 0.15));
      haloGradient.addColorStop(1, 'rgba(0,0,0,0)');
      ctx.fillStyle = haloGradient;
      ctx.beginPath();
      ctx.arc(x, y, radius * 1.2, 0, Math.PI * 2);
      ctx.fill();
      
      // Simple orb body
      const gradient = ctx.createRadialGradient(x, y, 0, x, y, radius);
      gradient.addColorStop(0, getColor(orb.colorIndex * 50, orbCount * 50, 0.25));
      gradient.addColorStop(0.7, getColor(orb.colorIndex * 50 + 30, orbCount * 50, 0.15));
      gradient.addColorStop(1, 'rgba(0,0,0,0)');
      ctx.fillStyle = gradient;
      ctx.beginPath();
      ctx.arc(x, y, radius, 0, Math.PI * 2);
      ctx.fill();
      
      // Just one subtle ring
      ctx.strokeStyle = getColor(orb.colorIndex * 50, orbCount * 50, 0.2);
      ctx.lineWidth = 1;
      ctx.beginPath();
      ctx.arc(x, y, radius * 0.6, 0, Math.PI * 2);
      ctx.stroke();
      
      // Small dim core
      ctx.fillStyle = getColor(orb.colorIndex * 50 + 100, orbCount * 50, 0.2);
      ctx.beginPath();
      ctx.arc(x, y, radius * 0.12, 0, Math.PI * 2);
      ctx.fill();
    });
    
    ctx.restore();
    return;
  }
  
  // ACTIVE MODE with music playing
  
  // Draw connections between nearby orbs
  orbs.forEach((orb1, idx1) => {
    orbs.forEach((orb2, idx2) => {
      if (idx1 >= idx2) return;
      
      const dx = orb2.x - orb1.x;
      const dy = orb2.y - orb1.y;
      const distance = Math.sqrt(dx * dx + dy * dy);
      
      if (distance < 350) {
        const avgValue = (orb1.value + orb2.value) / 2;
        const strength = 1 - (distance / 350);
        
        const gradient = ctx.createLinearGradient(orb1.x, orb1.y, orb2.x, orb2.y);
        gradient.addColorStop(0, getColor(orb1.colorIndex * 50, orbCount * 50, avgValue * strength * 0.4));
        gradient.addColorStop(0.5, getColor((orb1.colorIndex + orb2.colorIndex) * 25, orbCount * 50, avgValue * strength * 0.6));
        gradient.addColorStop(1, getColor(orb2.colorIndex * 50, orbCount * 50, avgValue * strength * 0.4));
        
        ctx.strokeStyle = gradient;
        ctx.lineWidth = 1 + avgValue * 3;
        ctx.shadowBlur = 10;
        ctx.shadowColor = getColor(orb1.colorIndex * 50, orbCount * 50, avgValue * strength * 0.3);
        
        ctx.beginPath();
        ctx.moveTo(orb1.x, orb1.y);
        ctx.lineTo(orb2.x, orb2.y);
        ctx.stroke();
        ctx.shadowBlur = 0;
      }
    });
  });
  
  // Draw expanding ripple rings from designated speakers ONLY
  orbs.forEach((orb) => {
    // Only create rings if this speaker is designated AND vibrating AND high enough value
    if (orb.createsRings && orb.isVibrating && orb.value > 0.65) {
      // Create ripple effect - multiple expanding rings
      const rippleCount = 3;
      for (let r = 0; r < rippleCount; r++) {
        const ripplePhase = (breathePhase * 2 + r * 0.7) % (Math.PI * 2);
        const rippleProgress = ripplePhase / (Math.PI * 2); // 0 to 1
        const rippleRadius = orb.radius * 1.5 + rippleProgress * orb.radius * 1.5;
        const rippleAlpha = (1 - rippleProgress) * orb.value * 0.6;
        
        if (rippleAlpha > 0.05) {
          ctx.strokeStyle = getColor(orb.colorIndex * 50 + r * 80, orbCount * 50, rippleAlpha);
          ctx.lineWidth = 2 + (1 - rippleProgress) * 4;
          ctx.shadowBlur = 15;
          ctx.shadowColor = getColor(orb.colorIndex * 50 + r * 80, orbCount * 50, rippleAlpha * 0.5);
          ctx.beginPath();
          ctx.arc(orb.x, orb.y, rippleRadius, 0, Math.PI * 2);
          ctx.stroke();
          ctx.shadowBlur = 0;
        }
      }
    }
  });
  
  // Draw orbs with colorful reactive grid lines
  orbs.forEach((orb) => {
    let x = orb.x;
    let y = orb.y;
    const radius = orb.radius;
    
    // VIBRATION EFFECT - speakers shake when active
    if (orb.isVibrating && orb.value > 0.65) {
      const vibrateAmount = (orb.value - 0.65) * 8; // Stronger vibration at high values
      x += Math.sin(breathePhase * 8 + orb.colorIndex) * vibrateAmount;
      y += Math.cos(breathePhase * 8 + orb.colorIndex * 1.3) * vibrateAmount;
    }
    
    // Dimmed appearance when not vibrating
    const activeMultiplier = orb.isVibrating ? 1.0 : 0.4;
    
    // Outer glow halo
    const haloGradient = ctx.createRadialGradient(x, y, radius * 0.5, x, y, radius * 1.5);
    haloGradient.addColorStop(0, getColor(orb.colorIndex * 50, orbCount * 50, orb.value * 0.4 * activeMultiplier));
    haloGradient.addColorStop(0.6, getColor(orb.colorIndex * 50 + 30, orbCount * 50, orb.value * 0.2 * activeMultiplier));
    haloGradient.addColorStop(1, 'rgba(0,0,0,0)');
    ctx.fillStyle = haloGradient;
    ctx.beginPath();
    ctx.arc(x, y, radius * 1.5, 0, Math.PI * 2);
    ctx.fill();
    
    // Multi-layer main orb - slow rotation
    const layers = 3;
    for (let layer = layers - 1; layer >= 0; layer--) {
      const layerRadius = radius * (1 - layer * 0.2);
      const layerAlpha = (layers - layer) / layers * 0.6;
      
      const spinAngle = rotationAngle * 0.3 * (layer + 1);
      const x1 = x + Math.cos(spinAngle) * layerRadius;
      const y1 = y + Math.sin(spinAngle) * layerRadius;
      const x2 = x - Math.cos(spinAngle) * layerRadius;
      const y2 = y - Math.sin(spinAngle) * layerRadius;
      
      const gradient = ctx.createLinearGradient(x1, y1, x2, y2);
      const colorShift = layer * 50;
      gradient.addColorStop(0, getColor(orb.colorIndex * 50 + colorShift, orbCount * 50, orb.value * layerAlpha * 0.5 * activeMultiplier));
      gradient.addColorStop(0.5, getColor(orb.colorIndex * 50 + colorShift + 100, orbCount * 50, orb.value * layerAlpha * 0.7 * activeMultiplier));
      gradient.addColorStop(1, getColor(orb.colorIndex * 50 + colorShift, orbCount * 50, orb.value * layerAlpha * 0.5 * activeMultiplier));
      
      ctx.fillStyle = gradient;
      ctx.beginPath();
      ctx.arc(x, y, layerRadius, 0, Math.PI * 2);
      ctx.fill();
    }
    
    // COLORFUL REACTIVE GRID RINGS - each ring gets different color based on audio
    const ringCount = 5;
    for (let r = 0; r < ringCount; r++) {
      const ringRadius = radius * (0.25 + r * 0.15);
      
      // Each ring reacts to different frequency band
      const freqIndex = Math.floor((r / ringCount) * bufferLength);
      const ringValue = dataArray[freqIndex] / 255;
      
      // Ring gets its own color that shifts with audio
      const ringColorShift = r * 150 + ringValue * 200;
      const ringBrightness = (0.4 + ringValue * 0.5) * activeMultiplier;
      const ringThickness = (1 + ringValue * 2) * (orb.isVibrating ? 1.0 : 0.7);
      
      ctx.strokeStyle = getColor(orb.colorIndex * 50 + ringColorShift, orbCount * 50, ringBrightness);
      ctx.lineWidth = ringThickness;
      ctx.shadowBlur = (6 + ringValue * 10) * activeMultiplier;
      ctx.shadowColor = getColor(orb.colorIndex * 50 + ringColorShift, orbCount * 50, ringValue * 0.5 * activeMultiplier);
      ctx.beginPath();
      ctx.arc(x, y, ringRadius, 0, Math.PI * 2);
      ctx.stroke();
      ctx.shadowBlur = 0;
    }
    
    // ENERGY SPIKES - only show on vibrating speakers
    if (orb.isVibrating) {
      const spikeCount = 6;
      for (let s = 0; s < spikeCount; s++) {
        const angle = (s / spikeCount) * Math.PI * 2 + rotationAngle * 0.4;
        const baseRadius = radius * 0.75;
        const tipRadius = radius * 1.15;
        
        const baseX = x + Math.cos(angle) * baseRadius;
        const baseY = y + Math.sin(angle) * baseRadius;
        const tipX = x + Math.cos(angle) * tipRadius;
        const tipY = y + Math.sin(angle) * tipRadius;
        
        // Each spike gets different color
        const spikeColorShift = s * 100 + orb.value * 150;
        const spikeGradient = ctx.createLinearGradient(baseX, baseY, tipX, tipY);
        spikeGradient.addColorStop(0, getColor(orb.colorIndex * 50 + spikeColorShift, orbCount * 50, orb.value * 0.7));
        spikeGradient.addColorStop(1, 'rgba(0,0,0,0)');
        
        ctx.strokeStyle = spikeGradient;
        ctx.lineWidth = 1.5 + orb.value * 1.5;
        ctx.shadowBlur = 8;
        ctx.shadowColor = getColor(orb.colorIndex * 50 + spikeColorShift, orbCount * 50, orb.value * 0.4);
        ctx.beginPath();
        ctx.moveTo(baseX, baseY);
        ctx.lineTo(tipX, tipY);
        ctx.stroke();
        ctx.shadowBlur = 0;
      }
    }
    
    // Bright reactive core
    const coreSize = radius * 0.18;
    const coreColorShift = orb.value * 200;
    ctx.fillStyle = getColor(orb.colorIndex * 50 + 100 + coreColorShift, orbCount * 50, orb.value * 0.9 * activeMultiplier);
    ctx.shadowBlur = (12 + orb.value * 8) * activeMultiplier;
    ctx.shadowColor = getColor(orb.colorIndex * 50 + 100 + coreColorShift, orbCount * 50, orb.value * 0.6 * activeMultiplier);
    ctx.beginPath();
    ctx.arc(x, y, coreSize, 0, Math.PI * 2);
    ctx.fill();
    ctx.shadowBlur = 0;
  });
  
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

function drawLiquidCrystals() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const cellSize = 120;
  const cols = Math.ceil(width / cellSize) + 1;
  const rows = Math.ceil(height / cellSize) + 1;

  ctx.save();
  
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      const baseX = i * cellSize;
      const baseY = j * cellSize;
      const index = i + j * cols;
      const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.11) * 0.3 + 0.5;
      
      // Apply warp distortion
      const warped = applyWarpDistortion(baseX, baseY);
      const x = warped.x;
      const y = warped.y;
      
      // Create organic, fluid cell shapes
      const sides = 5 + Math.floor(value * 3); // 5-7 sides based on audio
      const baseRadius = cellSize * 0.4 * warped.scale;
      
      // Spawn particles at high energy cells
      if (audioLoaded.value && value > 0.8 && Math.random() > 0.9) {
        createParticles(x, y, value, index, cols * rows, 2);
      }
      
      // Draw fluid membrane (outer glow)
      if (value > 0.5) {
        const glowRadius = baseRadius * 1.5;
        const gradient = ctx.createRadialGradient(x, y, baseRadius * 0.5, x, y, glowRadius);
        gradient.addColorStop(0, 'rgba(0,0,0,0)');
        gradient.addColorStop(0.7, getColor(index, cols * rows, (value - 0.5) * 0.4));
        gradient.addColorStop(1, 'rgba(0,0,0,0)');
        ctx.fillStyle = gradient;
        ctx.beginPath();
        ctx.arc(x, y, glowRadius, 0, Math.PI * 2);
        ctx.fill();
      }
      
      // Draw main cell with morphing shape
      ctx.beginPath();
      for (let k = 0; k <= sides; k++) {
        const angle = (k / sides) * Math.PI * 2 + rotationAngle * 0.5;
        
        // Create organic variation in radius
        const radiusVariation = Math.sin(angle * 3 + breathePhase + index * 0.3) * 0.2 + 0.9;
        const audioMorph = Math.sin(angle * 2 + value * Math.PI * 2) * value * 0.3;
        const radius = baseRadius * (radiusVariation + audioMorph);
        
        const px = x + Math.cos(angle) * radius;
        const py = y + Math.sin(angle) * radius;
        
        if (k === 0) {
          ctx.moveTo(px, py);
        } else {
          // Use bezier curves for smooth, organic edges
          const prevAngle = ((k - 1) / sides) * Math.PI * 2 + rotationAngle * 0.5;
          const prevRadius = baseRadius * (Math.sin(prevAngle * 3 + breathePhase + index * 0.3) * 0.2 + 0.9);
          const cpx = x + Math.cos(prevAngle + Math.PI / sides) * prevRadius * 1.1;
          const cpy = y + Math.sin(prevAngle + Math.PI / sides) * prevRadius * 1.1;
          ctx.quadraticCurveTo(cpx, cpy, px, py);
        }
      }
      ctx.closePath();
      
      // Fill with gradient
      const cellGradient = ctx.createRadialGradient(x, y, 0, x, y, baseRadius);
      cellGradient.addColorStop(0, getColor(index, cols * rows, value * 0.9));
      cellGradient.addColorStop(0.6, getColor(index + 30, cols * rows, value * 0.7));
      cellGradient.addColorStop(1, getColor(index + 60, cols * rows, value * 0.3));
      ctx.fillStyle = cellGradient;
      ctx.fill();
      
      // Outer membrane stroke
      ctx.strokeStyle = getColor(index + 100, cols * rows, 1);
      ctx.lineWidth = value > 0.7 ? 3 : 2;
      if (value > 0.6) {
        ctx.shadowBlur = 15;
        ctx.shadowColor = getColor(index + 100, cols * rows, value);
      }
      ctx.stroke();
      ctx.shadowBlur = 0;
      
      // Draw inner nucleus
      const nucleusSize = baseRadius * 0.3 * (0.8 + value * 0.4);
      const nucleusSides = 4;
      
      ctx.beginPath();
      for (let k = 0; k < nucleusSides; k++) {
        const angle = (k / nucleusSides) * Math.PI * 2 + rotationAngle;
        const px = x + Math.cos(angle) * nucleusSize;
        const py = y + Math.sin(angle) * nucleusSize;
        if (k === 0) ctx.moveTo(px, py);
        else ctx.lineTo(px, py);
      }
      ctx.closePath();
      
      if (value > 0.65) {
        ctx.shadowBlur = 12;
        ctx.shadowColor = getColor(index + 150, cols * rows, value);
      }
      ctx.fillStyle = getColor(index + 150, cols * rows, value);
      ctx.fill();
      ctx.strokeStyle = getColor(index + 150, cols * rows, 1);
      ctx.lineWidth = 1.5;
      ctx.stroke();
      ctx.shadowBlur = 0;
      
      // Draw energy pulses (small dots) around high-energy cells
      if (value > 0.75) {
        const pulseCount = 4 + Math.floor(value * 4);
        for (let p = 0; p < pulseCount; p++) {
          const pulseAngle = (p / pulseCount) * Math.PI * 2 + breathePhase;
          const pulseDistance = baseRadius * 1.2 + Math.sin(breathePhase * 2 + p) * 10;
          const pulseX = x + Math.cos(pulseAngle) * pulseDistance;
          const pulseY = y + Math.sin(pulseAngle) * pulseDistance;
          const pulseSize = 2 + value * 3;
          
          ctx.fillStyle = getColor(index + p * 20, cols * rows, value);
          ctx.beginPath();
          ctx.arc(pulseX, pulseY, pulseSize, 0, Math.PI * 2);
          ctx.fill();
        }
      }
    }
  }
  
  // SECOND PASS: Draw all connecting lines on top
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      const baseX = i * cellSize;
      const baseY = j * cellSize;
      const index = i + j * cols;
      const dataIndex = Math.floor((index / (cols * rows)) * bufferLength);
      const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + index * 0.11) * 0.3 + 0.5;
      
      const warped = applyWarpDistortion(baseX, baseY);
      const x = warped.x;
      const y = warped.y;
      
      // HORIZONTAL connections - lower threshold, more connections
      if (i < cols - 1 && value > 0.5) { // Lowered from 0.7
        const nextIndex = (i + 1) + j * cols;
        const nextDataIndex = Math.floor((nextIndex / (cols * rows)) * bufferLength);
        const nextValue = audioLoaded.value ? dataArray[nextDataIndex] / 255 : value;
        
        if (nextValue > 0.5) { // Lowered from 0.7
          const nextWarped = applyWarpDistortion((i + 1) * cellSize, baseY);
          const midX = (x + nextWarped.x) / 2;
          const midY = (y + nextWarped.y) / 2;
          
          // Draw organic connecting tendril
          const lineIntensity = Math.min(value, nextValue);
          ctx.strokeStyle = getColor(index, cols * rows, lineIntensity * 0.6); // Increased from 0.4
          ctx.lineWidth = 2 + lineIntensity * 4; // Increased from 3
          ctx.shadowBlur = 8; // Added glow
          ctx.shadowColor = getColor(index, cols * rows, lineIntensity * 0.4);
          ctx.beginPath();
          ctx.moveTo(x, y);
          ctx.quadraticCurveTo(
            midX + Math.sin(breathePhase + index * 0.2) * 20,
            midY + Math.cos(breathePhase + index * 0.2) * 20,
            nextWarped.x, nextWarped.y
          );
          ctx.stroke();
          ctx.shadowBlur = 0;
        }
      }
      
      // VERTICAL connections - lower threshold, more connections
      if (j < rows - 1 && value > 0.5) { // Lowered from 0.7
        const nextIndex = i + (j + 1) * cols;
        const nextDataIndex = Math.floor((nextIndex / (cols * rows)) * bufferLength);
        const nextValue = audioLoaded.value ? dataArray[nextDataIndex] / 255 : value;
        
        if (nextValue > 0.5) { // Lowered from 0.7
          const nextWarped = applyWarpDistortion(baseX, (j + 1) * cellSize);
          const midX = (x + nextWarped.x) / 2;
          const midY = (y + nextWarped.y) / 2;
          
          const lineIntensity = Math.min(value, nextValue);
          ctx.strokeStyle = getColor(index, cols * rows, lineIntensity * 0.6); // Increased from 0.4
          ctx.lineWidth = 2 + lineIntensity * 4; // Increased from 3
          ctx.shadowBlur = 8; // Added glow
          ctx.shadowColor = getColor(index, cols * rows, lineIntensity * 0.4);
          ctx.beginPath();
          ctx.moveTo(x, y);
          ctx.quadraticCurveTo(
            midX + Math.sin(breathePhase + index * 0.2 + Math.PI / 2) * 20,
            midY + Math.cos(breathePhase + index * 0.2 + Math.PI / 2) * 20,
            nextWarped.x, nextWarped.y
          );
          ctx.stroke();
          ctx.shadowBlur = 0;
        }
      }
      
      // DIAGONAL connections (NEW!) - top-right and bottom-right
      if (i < cols - 1 && j < rows - 1 && value > 0.55) {
        // Bottom-right diagonal
        const diagIndex = (i + 1) + (j + 1) * cols;
        const diagDataIndex = Math.floor((diagIndex / (cols * rows)) * bufferLength);
        const diagValue = audioLoaded.value ? dataArray[diagDataIndex] / 255 : value;
        
        if (diagValue > 0.55) {
          const diagWarped = applyWarpDistortion((i + 1) * cellSize, (j + 1) * cellSize);
          const lineIntensity = Math.min(value, diagValue);
          
          ctx.strokeStyle = getColor(index + 25, cols * rows, lineIntensity * 0.5);
          ctx.lineWidth = 1.5 + lineIntensity * 3;
          ctx.shadowBlur = 6;
          ctx.shadowColor = getColor(index + 25, cols * rows, lineIntensity * 0.3);
          ctx.beginPath();
          ctx.moveTo(x, y);
          ctx.lineTo(diagWarped.x, diagWarped.y);
          ctx.stroke();
          ctx.shadowBlur = 0;
        }
      }
      
      if (i < cols - 1 && j > 0 && value > 0.55) {
        // Top-right diagonal
        const diagIndex = (i + 1) + (j - 1) * cols;
        const diagDataIndex = Math.floor((diagIndex / (cols * rows)) * bufferLength);
        const diagValue = audioLoaded.value ? dataArray[diagDataIndex] / 255 : value;
        
        if (diagValue > 0.55) {
          const diagWarped = applyWarpDistortion((i + 1) * cellSize, (j - 1) * cellSize);
          const lineIntensity = Math.min(value, diagValue);
          
          ctx.strokeStyle = getColor(index + 50, cols * rows, lineIntensity * 0.5);
          ctx.lineWidth = 1.5 + lineIntensity * 3;
          ctx.shadowBlur = 6;
          ctx.shadowColor = getColor(index + 50, cols * rows, lineIntensity * 0.3);
          ctx.beginPath();
          ctx.moveTo(x, y);
          ctx.lineTo(diagWarped.x, diagWarped.y);
          ctx.stroke();
          ctx.shadowBlur = 0;
        }
      }
    }
  }
  
  ctx.restore();
}

function drawEnergyShards() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const shardCount = audioLoaded.value && !isPaused.value ? 60 : 40; // More shards when active
  const centerX = width / 2;
  const centerY = height / 2;
  
  ctx.save();
  ctx.translate(centerX, centerY);
  ctx.rotate(rotationAngle * 0.3);
  ctx.translate(-centerX, -centerY);
  
  for (let i = 0; i < shardCount; i++) {
    const dataIndex = Math.floor((i / shardCount) * bufferLength);
    const value = audioLoaded.value ? dataArray[dataIndex] / 255 : Math.sin(breathePhase + i * 0.2) * 0.3 + 0.5;
    
    // Position shards in expanding rings
    const ringIndex = Math.floor(i / 10); // 10 shards per ring instead of 8
    const ringPosition = i % 10;
    const ringCount = 10;
    const baseRadius = 80 + ringIndex * 100; // Closer rings
    
    // Much more explosive audio response
    const explosionForce = audioLoaded.value && !isPaused.value ? value * 150 : value * 40; // 150 instead of 80
    const radius = baseRadius + explosionForce;
    
    const angle = (ringPosition / ringCount) * Math.PI * 2 + rotationAngle * (ringIndex % 2 === 0 ? 1.5 : -1.5); // Faster rotation
    const baseX = centerX + Math.cos(angle) * radius;
    const baseY = centerY + Math.sin(angle) * radius;
    
    // Apply warp
    const warped = applyWarpDistortion(baseX, baseY);
    const x = warped.x;
    const y = warped.y;
    
    // Larger shards with more variation
    const shardLength = audioLoaded.value && !isPaused.value ? 50 + value * 120 : 40 + value * 60;
    const shardWidth = audioLoaded.value && !isPaused.value ? 18 + value * 35 : 15 + value * 20;
    
    // More particles
    if (audioLoaded.value && value > 0.75 && Math.random() > 0.8) { // More frequent
      createParticles(x, y, value, i, shardCount, 8); // More particles per spawn
    }
    
    ctx.save();
    ctx.translate(x, y);
    
    // Rotate shard to point outward from center + more audio spin
    const pointAngle = Math.atan2(y - centerY, x - centerX) + value * Math.PI * 0.8; // More spin
    ctx.rotate(pointAngle);
    
    // Longer, more intense energy trails
    if (value > 0.5) { // Lower threshold
      const trailLength = shardLength * 2; // Longer trails
      const gradient = ctx.createLinearGradient(-trailLength, 0, 0, 0);
      gradient.addColorStop(0, 'rgba(0,0,0,0)');
      gradient.addColorStop(0.3, getColor(i, shardCount, (value - 0.5) * 0.8)); // More opacity
      gradient.addColorStop(1, getColor(i, shardCount, (value - 0.5) * 0.6));
      
      ctx.fillStyle = gradient;
      ctx.beginPath();
      ctx.moveTo(-trailLength, -shardWidth * 0.4);
      ctx.lineTo(0, -shardWidth * 0.6);
      ctx.lineTo(0, shardWidth * 0.6);
      ctx.lineTo(-trailLength, shardWidth * 0.4);
      ctx.closePath();
      ctx.fill();
    }
    
    // Main shard body - crystalline shape
    const shardPath = [
      [shardLength * 0.5, 0],  // Sharp tip
      [shardLength * 0.1, shardWidth * 0.3],
      [-shardLength * 0.5, shardWidth * 0.4],
      [-shardLength * 0.3, 0],
      [-shardLength * 0.5, -shardWidth * 0.4],
      [shardLength * 0.1, -shardWidth * 0.3]
    ];
    
    // Stronger outer glow
    if (value > 0.45) { // Lower threshold
      ctx.shadowBlur = 35; // Stronger
      ctx.shadowColor = getColor(i, shardCount, value);
    }
    
    // Brighter crystal gradient
    const shardGradient = ctx.createLinearGradient(-shardLength * 0.5, 0, shardLength * 0.5, 0);
    shardGradient.addColorStop(0, getColor(i, shardCount, value * 0.6)); // Brighter
    shardGradient.addColorStop(0.5, getColor(i, shardCount, value));
    shardGradient.addColorStop(1, getColor(i + 30, shardCount, value * 1.2)); // Much brighter tip
    
    ctx.fillStyle = shardGradient;
    ctx.beginPath();
    shardPath.forEach(([px, py], idx) => {
      if (idx === 0) ctx.moveTo(px, py);
      else ctx.lineTo(px, py);
    });
    ctx.closePath();
    ctx.fill();
    
    // Brighter crystal edges
    ctx.strokeStyle = getColor(i + 50, shardCount, 1);
    ctx.lineWidth = value > 0.6 ? 4 : 3; // Thicker
    ctx.stroke();
    ctx.shadowBlur = 0;
    
    // Inner facet lines
    ctx.strokeStyle = getColor(i + 100, shardCount, value * 0.8);
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.moveTo(shardLength * 0.5, 0);
    ctx.lineTo(-shardLength * 0.3, 0);
    ctx.stroke();
    
    ctx.beginPath();
    ctx.moveTo(shardLength * 0.1, shardWidth * 0.3);
    ctx.lineTo(-shardLength * 0.3, 0);
    ctx.lineTo(shardLength * 0.1, -shardWidth * 0.3);
    ctx.stroke();
    
    // Brighter core/tip highlight
    if (value > 0.55) { // Lower threshold
      const tipGradient = ctx.createRadialGradient(shardLength * 0.4, 0, 0, shardLength * 0.4, 0, shardWidth * 0.6);
      tipGradient.addColorStop(0, getColor(i + 150, shardCount, 1));
      tipGradient.addColorStop(0.5, getColor(i + 150, shardCount, 0.6));
      tipGradient.addColorStop(1, 'rgba(0,0,0,0)');
      
      ctx.fillStyle = tipGradient;
      ctx.beginPath();
      ctx.arc(shardLength * 0.4, 0, shardWidth * 0.6, 0, Math.PI * 2);
      ctx.fill();
    }
    
    // More energy sparks at tip
    if (value > 0.6) { // Lower threshold
      const sparkCount = 4 + Math.floor(value * 6); // More sparks
      for (let s = 0; s < sparkCount; s++) {
        const sparkAngle = (s / sparkCount) * Math.PI * 2 + breathePhase * 4;
        const sparkDist = shardWidth * 0.7 + Math.sin(breathePhase * 5 + s) * 8;
        const sparkX = shardLength * 0.5 + Math.cos(sparkAngle) * sparkDist;
        const sparkY = Math.sin(sparkAngle) * sparkDist;
        const sparkSize = 3 + value * 6; // Bigger sparks
        
        ctx.fillStyle = getColor(i + s * 25, shardCount, 1);
        ctx.shadowBlur = 15; // More glow
        ctx.shadowColor = getColor(i + s * 25, shardCount, 1);
        ctx.beginPath();
        ctx.arc(sparkX, sparkY, sparkSize, 0, Math.PI * 2);
        ctx.fill();
        ctx.shadowBlur = 0;
      }
    }
    
    ctx.restore();
    
    // More connection beams between nearby high-energy shards
    if (value > 0.65 && i < shardCount - 1 && Math.random() > 0.5) { // More frequent
      const nextDataIndex = Math.floor(((i + 1) / shardCount) * bufferLength);
      const nextValue = audioLoaded.value ? dataArray[nextDataIndex] / 255 : value;
      
      if (nextValue > 0.65) {
        const nextRingIndex = Math.floor((i + 1) / 10);
        const nextRingPosition = (i + 1) % 10;
        const nextRadius = 80 + nextRingIndex * 100 + (audioLoaded.value && !isPaused.value ? nextValue * 150 : nextValue * 40);
        const nextAngle = (nextRingPosition / ringCount) * Math.PI * 2 + rotationAngle * (nextRingIndex % 2 === 0 ? 1.5 : -1.5);
        const nextX = centerX + Math.cos(nextAngle) * nextRadius;
        const nextY = centerY + Math.sin(nextAngle) * nextRadius;
        const nextWarped = applyWarpDistortion(nextX, nextY);
        
        // Brighter lightning connection
        const beamGradient = ctx.createLinearGradient(x, y, nextWarped.x, nextWarped.y);
        beamGradient.addColorStop(0, getColor(i, shardCount, value * 0.8));
        beamGradient.addColorStop(0.5, getColor(i + 75, shardCount, Math.min(value, nextValue) * 1.2));
        beamGradient.addColorStop(1, getColor(i + 1, shardCount, nextValue * 0.8));
        
        ctx.strokeStyle = beamGradient;
        ctx.lineWidth = 2 + Math.min(value, nextValue) * 6; // Thicker
        ctx.shadowBlur = 20; // More glow
        ctx.shadowColor = getColor(i, shardCount, value);
        
        // More jagged lightning
        ctx.beginPath();
        ctx.moveTo(x, y);
        const segments = 5; // More segments
        for (let seg = 1; seg <= segments; seg++) {
          const t = seg / segments;
          const midX = x + (nextWarped.x - x) * t;
          const midY = y + (nextWarped.y - y) * t;
          const jitter = (Math.random() - 0.5) * 50 * value; // More jitter
          ctx.lineTo(midX + jitter, midY + jitter);
        }
        ctx.lineTo(nextWarped.x, nextWarped.y);
        ctx.stroke();
        ctx.shadowBlur = 0;
      }
    }
  }
  
  // Much larger, more explosive central core
  const coreSize = 40 + Math.sin(breathePhase) * 15; // Bigger base
  const coreValue = audioLoaded.value ? dataArray[0] / 255 : 0.6;
  const explosiveCore = coreSize + (audioLoaded.value && !isPaused.value ? coreValue * 80 : coreValue * 20); // Much bigger with audio
  
  // Brighter pulsing gradient core
  const coreGradient = ctx.createRadialGradient(centerX, centerY, 0, centerX, centerY, explosiveCore);
  coreGradient.addColorStop(0, getColor(0, 1, 1));
  coreGradient.addColorStop(0.3, getColor(50, 100, coreValue));
  coreGradient.addColorStop(0.6, getColor(100, 100, coreValue * 0.6));
  coreGradient.addColorStop(1, 'rgba(0,0,0,0)');
  
  ctx.fillStyle = coreGradient;
  ctx.beginPath();
  ctx.arc(centerX, centerY, explosiveCore, 0, Math.PI * 2);
  ctx.fill();
  
  // Brighter core solid circle with stronger shadow
  ctx.shadowBlur = 60; // Much stronger
  ctx.shadowColor = getColor(0, 1, coreValue);
  ctx.fillStyle = getColor(0, 1, coreValue);
  ctx.beginPath();
  ctx.arc(centerX, centerY, coreSize, 0, Math.PI * 2);
  ctx.fill();
  ctx.shadowBlur = 0;
  
  // More core energy rings
  for (let r = 1; r <= 6; r++) { // 6 rings instead of 4
    ctx.strokeStyle = getColor(r * 40, 240, 0.6 + coreValue * 0.4); // Brighter
    ctx.lineWidth = 3; // Thicker
    ctx.shadowBlur = 10;
    ctx.shadowColor = getColor(r * 40, 240, coreValue * 0.4);
    ctx.beginPath();
    ctx.arc(centerX, centerY, coreSize + r * 15, 0, Math.PI * 2);
    ctx.stroke();
    ctx.shadowBlur = 0;
  }
  
  ctx.restore();
}

function drawBoombox() {
  const width = canvas.value.width;
  const height = canvas.value.height;
  const peakCount = 3;
  
  ctx.save();
  
  // Generate elevation peaks - stable positions
  const peaks = [];
  let i = 0;
  while (i < peakCount) {
    const dataIndex = Math.floor((i / peakCount) * bufferLength);
    const value = audioLoaded.value ? dataArray[dataIndex] / 255 : 0.6;
    
    // Moderately damped audio response
    const smoothedValue = value * 0.2 + 0.6; // Range: 0.6 to 0.8
    
    peaks.push({
      x: width * (0.25 + i * 0.25),
      y: height * 0.5 + Math.sin(breathePhase * 0.1 + i * 2) * 50,
      height: 60 + smoothedValue * 50, // Range: 60-110
      radius: 250,
      value: smoothedValue
    });
    i++;
  }
  
  // Calculate elevation
  function getElevation(x, y) {
    let elevation = 0;
    
    peaks.forEach((peak) => {
      const dx = x - peak.x;
      const dy = y - peak.y;
      const distance = Math.sqrt(dx * dx + dy * dy);
      
      const influence = peak.height * Math.exp(-(distance * distance) / (2 * peak.radius * peak.radius));
      elevation += influence;
    });
    
    return elevation;
  }
  
  // Only draw elevation zones if audio is playing
  if (audioLoaded.value && !isPaused.value) {
    const zoneCount = 6;
    let zoneIdx = 0;
    while (zoneIdx < zoneCount) {
      const minElevation = zoneIdx * 15;
      const maxElevation = (zoneIdx + 1) * 15;
      const elevationRatio = (minElevation + maxElevation) / 2 / 100;
      const colorIndex = Math.floor(elevationRatio * 1000);
      
      // Create semi-transparent filled regions
      ctx.fillStyle = getColor(colorIndex, 1000, 0.15); // Subtle transparency
      
      // Sample and fill the elevation zone
      ctx.beginPath();
      let firstPoint = true;
      let x = 0;
      while (x < width) {
        let y = 0;
        while (y < height) {
          const elev = getElevation(x, y);
          
          if (elev >= minElevation && elev < maxElevation) {
            if (firstPoint) {
              ctx.moveTo(x, y);
              firstPoint = false;
            }
            // Draw small filled circles to create the zone
            ctx.arc(x, y, 8, 0, Math.PI * 2);
          }
          y += 16;
        }
        x += 16;
      }
      ctx.fill();
      
      zoneIdx++;
    }
  }
  
  // Draw contour lines with varying styles
  const contourInterval = 10;
  const maxContours = 12;
  
  peaks.forEach((peak, peakIdx) => {
    const peakX = peak.x;
    const peakY = peak.y;
    
    let contourLevel = 1;
    while (contourLevel < maxContours) {
      const targetElevation = contourLevel * contourInterval;
      if (targetElevation > peak.height) break;
      
      const ratio = targetElevation / peak.height;
      if (ratio <= 0 || ratio > 1) {
        contourLevel++;
        continue;
      }
      
      const contourRadius = peak.radius * Math.sqrt(-2 * Math.log(ratio));
      const segments = 50;
      const elevationRatio = targetElevation / 110;
      const colorIndex = Math.floor(elevationRatio * 1000);
      
      // Varying line styles - but simpler when no audio
      const isMajorLine = contourLevel % 3 === 0;
      const isAccentLine = contourLevel % 5 === 0;
      
      if (audioLoaded.value && !isPaused.value) {
        // Full styling when audio is playing
        if (isAccentLine) {
          ctx.lineWidth = 3;
          ctx.setLineDash([10, 5]);
          ctx.strokeStyle = getColor(colorIndex, 1000, 0.8);
          ctx.shadowBlur = 12;
          ctx.shadowColor = getColor(colorIndex, 1000, 0.4);
        } else if (isMajorLine) {
          ctx.lineWidth = 2;
          ctx.setLineDash([]);
          ctx.strokeStyle = getColor(colorIndex, 1000, 0.6);
          ctx.shadowBlur = 6;
          ctx.shadowColor = getColor(colorIndex, 1000, 0.3);
        } else {
          ctx.lineWidth = 1;
          ctx.setLineDash([4, 4]);
          ctx.strokeStyle = getColor(colorIndex, 1000, 0.4);
          ctx.shadowBlur = 0;
        }
      } else {
        // Simple styling when no audio
        if (isMajorLine) {
          ctx.lineWidth = 2;
          ctx.setLineDash([]);
          ctx.strokeStyle = getColor(colorIndex, 1000, 0.5);
          ctx.shadowBlur = 0;
        } else {
          ctx.lineWidth = 1;
          ctx.setLineDash([]);
          ctx.strokeStyle = getColor(colorIndex, 1000, 0.3);
          ctx.shadowBlur = 0;
        }
      }
      
      // Draw the contour
      ctx.beginPath();
      let segIdx = 0;
      while (segIdx <= segments) {
        const angle = (segIdx / segments) * Math.PI * 2;
        const variation = Math.sin(angle * 2 + breathePhase * 0.05) * 4;
        const actualRadius = contourRadius + variation;
        
        const x = peakX + Math.cos(angle) * actualRadius;
        const y = peakY + Math.sin(angle) * actualRadius;
        
        if (segIdx === 0) {
          ctx.moveTo(x, y);
        } else {
          ctx.lineTo(x, y);
        }
        segIdx++;
      }
      ctx.closePath();
      ctx.stroke();
      ctx.shadowBlur = 0;
      ctx.setLineDash([]);
      
      // Only add decorative tick marks when audio is playing
      if (audioLoaded.value && !isPaused.value && isMajorLine) {
        const tickCount = 8;
        let tickIdx = 0;
        while (tickIdx < tickCount) {
          const angle = (tickIdx / tickCount) * Math.PI * 2 + breathePhase * 0.05;
          const x = peakX + Math.cos(angle) * contourRadius;
          const y = peakY + Math.sin(angle) * contourRadius;
          
          // Draw outward tick
          const tickLength = 8;
          const endX = x + Math.cos(angle) * tickLength;
          const endY = y + Math.sin(angle) * tickLength;
          
          ctx.strokeStyle = getColor(colorIndex + 100, 1000, 0.7);
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.moveTo(x, y);
          ctx.lineTo(endX, endY);
          ctx.stroke();
          
          tickIdx++;
        }
      }
      
      contourLevel++;
    }
    
    // Peak marker - simpler when no audio
    const markerSize = 8 + peak.value * 6;
    const colorIndex = Math.floor((peak.height / 110) * 1000);
    
    if (audioLoaded.value && !isPaused.value) {
      // Full glow effects when audio is playing
      // Outer glow circle
      ctx.fillStyle = getColor(colorIndex, 1000, 0.2);
      ctx.beginPath();
      ctx.arc(peakX, peakY, markerSize * 2.5, 0, Math.PI * 2);
      ctx.fill();
      
      // Middle glow
      ctx.fillStyle = getColor(colorIndex, 1000, 0.4);
      ctx.beginPath();
      ctx.arc(peakX, peakY, markerSize * 1.5, 0, Math.PI * 2);
      ctx.fill();
      
      // Peak marker with shadow
      ctx.fillStyle = getColor(colorIndex, 1000, 0.9);
      ctx.strokeStyle = getColor(colorIndex + 150, 1000, 1);
      ctx.shadowBlur = 15;
      ctx.shadowColor = getColor(colorIndex, 1000, 0.6);
    } else {
      // Simple marker when no audio
      ctx.fillStyle = getColor(colorIndex, 1000, 0.7);
      ctx.strokeStyle = getColor(colorIndex + 150, 1000, 0.8);
      ctx.shadowBlur = 0;
    }
    
    // Triangle peak symbol
    ctx.lineWidth = 2.5;
    ctx.beginPath();
    ctx.moveTo(peakX, peakY - markerSize);
    ctx.lineTo(peakX + markerSize * 0.8, peakY + markerSize * 0.6);
    ctx.lineTo(peakX - markerSize * 0.8, peakY + markerSize * 0.6);
    ctx.closePath();
    ctx.fill();
    ctx.stroke();
    ctx.shadowBlur = 0;
    
    // Elevation label with background
    const labelText = Math.floor(peak.height) + 'm';
    ctx.font = 'bold 13px Inter, sans-serif';
    const textMetrics = ctx.measureText(labelText);
    const textWidth = textMetrics.width;
    
    // Label background
    ctx.fillStyle = 'rgba(0, 0, 0, 0.7)';
    ctx.fillRect(
      peakX - textWidth / 2 - 4,
      peakY - markerSize - 25,
      textWidth + 8,
      16
    );
    
    // Label text
    ctx.fillStyle = getColor(colorIndex, 1000, audioLoaded.value && !isPaused.value ? 1 : 0.8);
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    ctx.shadowBlur = audioLoaded.value && !isPaused.value ? 8 : 0;
    ctx.shadowColor = 'rgba(0, 0, 0, 0.9)';
    ctx.fillText(labelText, peakX, peakY - markerSize - 17);
    ctx.shadowBlur = 0;
    
    // Only spawn particles when audio is playing
    if (audioLoaded.value && !isPaused.value && peak.value > 0.75 && Math.random() > 0.97) {
      createParticles(peakX, peakY, peak.value, peakIdx, peakCount, 1);
    }
  });
  
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
  if (patternLocked.value) return;
  
  currentPatternIndex.value = (currentPatternIndex.value + 1) % patterns.length;
  currentPatternName.value = patterns[currentPatternIndex.value].name;

  // Get random pattern that's different from current
  let newIndex;
  do {
    newIndex = getRandomPatternIndex();
  } while (newIndex === currentPatternIndex.value && patterns.length > 1);
  
  if (patternTimer) {
    clearInterval(patternTimer);
    patternTimer = setInterval(() => {
      cyclePattern();
    }, patternRotateInterval.value);
  }
}

function cyclePreviousPattern() {
  if (patternLocked.value) return;
  
  // Get random pattern different from current
  let newIndex;
  do {
    newIndex = getRandomPatternIndex();
  } while (newIndex === currentPatternIndex.value && patterns.length > 1);
  
  currentPatternIndex.value = newIndex;
  currentPatternName.value = patterns[newIndex].name;
  
  // Reset timer
  if (patternTimer) {
    clearInterval(patternTimer);
    patternTimer = setInterval(() => {
      cyclePattern();
    }, patternRotateInterval.value);
  }
}

function cyclePalette() {
  currentPaletteIndex.value = (currentPaletteIndex.value + 1) % palettes.length;
  currentPaletteName.value = palettes[currentPaletteIndex.value].name;
  
  // Cycle to next color in palette
  typographyColorIndex.value = (typographyColorIndex.value + 1) % palettes[currentPaletteIndex.value].colors.length;
  
  if (paletteTimer) {
    clearInterval(paletteTimer);
    paletteTimer = setInterval(() => {
      cyclePalette();
    }, paletteRotateInterval.value);
  }
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
    case 'ArrowRight': {
      cyclePattern(); // Now random
      break;
    }
    case 'ArrowLeft': {
      if(patternLocked.value) return;
      
      // Get random pattern different from current
      let newIndex;
      do {
        newIndex = getRandomPatternIndex();
      } while (newIndex === currentPatternIndex.value && patterns.length > 1);
      
      currentPatternIndex.value = newIndex;
      currentPatternName.value = patterns[newIndex].name;
      break;
    }
    case 'ArrowUp': {
      cyclePalette();
      break;
    }
    case 'ArrowDown': {
      currentPaletteIndex.value = (currentPaletteIndex.value - 1 + palettes.length) % palettes.length;
      currentPaletteName.value = palettes[currentPaletteIndex.value].name;
      break;
    }
    case ' ': {
      e.preventDefault();
      togglePatternLock();
      break;
    }
  }
}

function startAutoRotation() {
  // Pattern rotation
  patternTimer = setInterval(() => {
    cyclePattern();
  }, patternRotateInterval.value);
  
  // Palette rotation (different interval for variety)
  paletteTimer = setInterval(() => {
    cyclePalette();
  }, paletteRotateInterval.value);
}

function stopAutoRotation() {
  if (patternTimer) {
    clearInterval(patternTimer);
    patternTimer = null;
  }
  if (paletteTimer) {
    clearInterval(paletteTimer);
    paletteTimer = null;
  }
}

onMounted(() => {
  ctx = canvas.value.getContext('2d');
  resize();
  window.addEventListener('resize', resize);
  window.addEventListener('keydown', handleKeyDown);  // Add this line
  

  // Set random initial pattern
  currentPatternIndex.value = getRandomPatternIndex();
  currentPatternName.value = patterns[currentPatternIndex.value].name;
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

  startAutoRotation();
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

  stopAutoRotation();
  
  if (animationId) cancelAnimationFrame(animationId);
  if (audioContext) audioContext.close();
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

#oww-typog {
  position: absolute;
  z-index: 10;
  width: 350px;
  top: 30px;
  left: 20px;
}

.triangle {
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 0 0 200px 400px; /* height, then width */
  border-color: transparent transparent #000000 transparent;
  left: 0; top: 0;
  z-index: 2;
  transform: rotate(180deg);
  position: absolute;
}

.triangle.tri2 {
  border-color: transparent transparent white transparent;
  z-index: 1;
  left: -13px;
  top: 4px;
  border-width: 0 0 210px 410px;
  opacity: 0.4;
}

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
  gap: 2px;
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

.palette-btn { 
  min-width: 150px;
}

.pattern-btn {
  min-width: 190px;
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
  background: rgba(20, 20, 20, 0.3);
  backdrop-filter: blur(20px) saturate(150%);
  padding: 10px 20px;
  border-radius: 20px;
  border: 5px solid black;
  font-size: 13px;
  color: rgba(255, 255, 255, 0.7);
  width: 550px;
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

.pattern-btn.locked {
  background: rgba(255, 255, 255, 0.15);
  border-color: rgba(255, 255, 255, 0.4);
}

.pattern-btn.locked:hover {
  background: rgba(255, 255, 255, 0.25);
}

.lock-icon {
  filter: brightness(0) invert(1); /* Makes emoji pure white */
  margin-right: 4px;
}

.arrow-btn {
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(0, 0, 0, 0.8);
  color: white;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 18px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0;
}

.arrow-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}

.arrow-btn:active {
  transform: scale(0.95);
}

.oww-1 {
  fill: #FFF;
}

#fore-text {
  fill: v-bind(currentTypographyColor);
  transition: fill 2s;
}


.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(6px) brightness(1.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  animation: fadeIn 0.4s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.modal-content {
  text-align: center;
  animation: slideUp 0.5s ease;
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.modal-logo {
  font-family: 'Inter', sans-serif;
  font-size: 72px;
  font-weight: 900;
  letter-spacing: -2px;
  color: white;
  margin-bottom: 8px;
  line-height: 1;
}

.modal-subtitle {
  font-family: 'Inter', sans-serif;
  font-size: 18px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.5);
  margin-bottom: 40px;
  letter-spacing: 2px;
  text-transform: uppercase;
}

.play-modal-btn {
  background: linear-gradient(135deg, #00ff88 0%, #00ffff 50%, #ff00ff 100%);
  background-size: 200% 200%;
  border: none;
  color: black;
  padding: 18px 56px;
  border-radius: 50px;
  font-family: 'Inter', sans-serif;
  font-size: 18px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  letter-spacing: 1px;
  text-transform: uppercase;
  box-shadow: 0 8px 32px rgba(0, 255, 136, 0.4);
  animation: gradientShift 3s ease infinite;
  position: relative;
  overflow: hidden;
}

@keyframes gradientShift {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.play-modal-btn::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  transform: translate(-50%, -50%);
  transition: width 0.6s, height 0.6s;
}

.play-modal-btn:hover {
  transform: scale(1.08);
  box-shadow: 0 12px 48px rgba(0, 255, 136, 0.6);
}

.play-modal-btn:hover::before {
  width: 300px;
  height: 300px;
}

.play-modal-btn:active {
  transform: scale(0.95);
}

#oww-typog,
#oww-typog-modal {
  stroke: black;
  stroke-width: 10px;
}
</style>
