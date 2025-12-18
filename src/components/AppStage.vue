<template>
  <div :style="styles.container">
    <div :style="styles.visualizerWrapper">
      <svg viewBox="0 0 2400 1600" :style="styles.svg">
        <defs>
          <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" :style="`stop-color: ${currentPalette.primary}; stop-opacity: 0.9`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.secondary}; stop-opacity: 0.9`" />
          </linearGradient>
          
          <linearGradient id="grad2" x1="0%" y1="0%" x2="0%" y2="100%">
            <stop offset="0%" :style="`stop-color: ${currentPalette.accent1}; stop-opacity: 0.8`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.accent2}; stop-opacity: 0.8`" />
          </linearGradient>
          
          <linearGradient id="grad3" x1="0%" y1="0%" x2="100%" y2="0%">
            <stop offset="0%" :style="`stop-color: ${currentPalette.highlight1}; stop-opacity: 0.85`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.highlight2}; stop-opacity: 0.85`" />
          </linearGradient>
          
          <radialGradient id="glow1">
            <stop offset="0%" :style="`stop-color: ${currentPalette.primary}; stop-opacity: ${audioData.bass * 0.9}`" />
            <stop offset="50%" :style="`stop-color: ${currentPalette.primary}; stop-opacity: ${audioData.bass * 0.5}`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.primary}; stop-opacity: 0`" />
          </radialGradient>
          
          <radialGradient id="glow2">
            <stop offset="0%" :style="`stop-color: ${currentPalette.accent2}; stop-opacity: ${audioData.treble * 0.8}`" />
            <stop offset="50%" :style="`stop-color: ${currentPalette.accent2}; stop-opacity: ${audioData.treble * 0.4}`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.accent2}; stop-opacity: 0`" />
          </radialGradient>
          
          <radialGradient id="centerGlow">
            <stop offset="0%" :style="`stop-color: ${currentPalette.highlight1}; stop-opacity: ${audioData.overall * 0.9}`" />
            <stop offset="100%" style="stop-color: #ffffff; stop-opacity: 0" />
          </radialGradient>
          
          <filter id="blur">
            <feGaussianBlur :stdDeviation="3 + audioData.overall * 8" />
          </filter>
          
          <filter id="glow">
            <feGaussianBlur stdDeviation="5" result="coloredBlur"/>
            <feMerge>
              <feMergeNode in="coloredBlur"/>
              <feMergeNode in="SourceGraphic"/>
            </feMerge>
          </filter>
          
          <clipPath id="hexClipSmall">
            <polygon points="0,-115.47 100,-57.735 100,57.735 0,115.47 -100,57.735 -100,-57.735" />
          </clipPath>
          
          <clipPath id="hexClipLarge">
            <polygon points="0,-577.35 500,-288.675 500,288.675 0,577.35 -500,288.675 -500,-288.675" />
          </clipPath>
        </defs>
        
        <!-- SURROUNDING CELLS - dense honeycomb pattern -->
        <g v-for="(cell, idx) in surroundingCells" :key="`cell-${idx}`">
          <g :transform="`translate(${cell.x}, ${cell.y})`">
            <!-- Hexagon border -->
            <polygon
              points="0,-115.47 100,-57.735 100,57.735 0,115.47 -100,57.735 -100,-57.735"
              fill="none"
              :stroke="currentPalette.primary"
              :stroke-width="1 + audioData.overall * 1.5"
              :opacity="0.2 + audioData.overall * 0.3"
            />
            
            <g clip-path="url(#hexClipSmall)">
              <!-- Subtle background glow -->
              <circle 
                cx="0" 
                cy="0" 
                :r="60 + audioData.bass * 40" 
                fill="url(#glow1)" 
                :opacity="audioData.bass * 0.15" 
              />
              <circle 
                cx="0" 
                cy="0" 
                :r="50 + audioData.treble * 35" 
                fill="url(#glow2)" 
                :opacity="audioData.treble * 0.12" 
              />
              
              <!-- Pattern rendering -->
              <g v-if="cell.pattern === 'spiral'">
                <path
                  v-for="(spiral, i) in getCellGeometry(cell).spirals"
                  :key="`spiral-${i}`"
                  :d="spiral.path"
                  fill="none"
                  stroke="url(#grad1)"
                  :stroke-width="0.8 + audioData.mid * 1.2"
                  :opacity="0.25 + audioData.overall * 0.4"
                />
              </g>
              
              <g v-else-if="cell.pattern === 'concentric'">
                <circle
                  v-for="(ring, i) in getCellGeometry(cell).concentricRings"
                  :key="`ring-${i}`"
                  cx="0"
                  cy="0"
                  :r="ring.r"
                  fill="none"
                  stroke="url(#grad2)"
                  :stroke-width="ring.strokeWidth"
                  :opacity="ring.opacity"
                />
              </g>
              
              <g v-else-if="cell.pattern === 'radial'">
                <line
                  v-for="(ray, i) in getCellGeometry(cell).rays"
                  :key="`ray-${i}`"
                  :x1="ray.x1"
                  :y1="ray.y1"
                  :x2="ray.x2"
                  :y2="ray.y2"
                  stroke="url(#grad3)"
                  :stroke-width="0.7 + audioData.treble * 1"
                  :opacity="0.25 + audioData.mid * 0.4"
                />
              </g>
              
              <g v-else-if="cell.pattern === 'geometric'">
                <polygon
                  v-for="(poly, i) in getCellGeometry(cell).polygons"
                  :key="`poly-${i}`"
                  :points="poly.points"
                  fill="none"
                  :stroke="poly.stroke"
                  :stroke-width="poly.strokeWidth"
                  :opacity="poly.opacity"
                />
              </g>
              
              <g v-else-if="cell.pattern === 'particles'">
                <circle
                  v-for="(particle, i) in getCellGeometry(cell).particles"
                  :key="`particle-${i}`"
                  :cx="particle.x"
                  :cy="particle.y"
                  :r="particle.r"
                  :fill="currentPalette.highlight1"
                  :opacity="particle.opacity"
                />
              </g>
              
              <g v-else-if="cell.pattern === 'wave'">
                <path
                  v-for="(wave, i) in getCellGeometry(cell).waves"
                  :key="`wave-${i}`"
                  :d="wave.path"
                  fill="none"
                  stroke="url(#grad2)"
                  :stroke-width="0.7 + audioData.bass * 1"
                  :opacity="0.25 + audioData.treble * 0.35"
                />
              </g>
            </g>
          </g>
        </g>
        
        <!-- CENTER CELL - 5x larger, dominant -->
        <g transform="translate(1200, 800)">
          <!-- Massive background glow pulses -->
          <circle 
            cx="0" 
            cy="0" 
            :r="400 + audioData.bass * 300" 
            fill="url(#glow1)" 
            :opacity="0.4 + audioData.bass * 0.5" 
          />
          <circle 
            cx="0" 
            cy="0" 
            :r="350 + audioData.treble * 250" 
            fill="url(#glow2)" 
            :opacity="0.3 + audioData.treble * 0.6" 
          />
          <circle 
            cx="0" 
            cy="0" 
            :r="250 + audioData.mid * 200" 
            fill="url(#centerGlow)" 
            :opacity="0.2 + audioData.mid * 0.5" 
            filter="url(#blur)"
          />
          
          <!-- Hexagon border - thick and prominent -->
          <polygon
            points="0,-577.35 500,-288.675 500,288.675 0,577.35 -500,288.675 -500,-288.675"
            fill="none"
            :stroke="currentPalette.primary"
            :stroke-width="4 + audioData.overall * 8"
            :opacity="0.7 + audioData.overall * 0.3"
            filter="url(#glow)"
          />
          
          <g clip-path="url(#hexClipLarge)">
            <!-- Explosive outer radial lines -->
            <g :style="{ opacity: 0.3 + audioData.treble * 0.7 }">
              <line
                v-for="(line, i) in centerGeometry.outerLines"
                :key="`line-outer-${i}`"
                :x1="line.x1"
                :y1="line.y1"
                :x2="line.x2"
                :y2="line.y2"
                stroke="url(#grad1)"
                :stroke-width="2 + audioData.mid * 6"
                stroke-linecap="round"
              />
            </g>
            
            <!-- Pulsing mid-layer lines -->
            <g :style="{ opacity: 0.4 + audioData.mid * 0.6 }">
              <line
                v-for="(line, i) in centerGeometry.midLines"
                :key="`line-mid-${i}`"
                :x1="line.x1"
                :y1="line.y1"
                :x2="line.x2"
                :y2="line.y2"
                stroke="url(#grad2)"
                :stroke-width="3 + audioData.bass * 8"
                stroke-linecap="round"
              />
            </g>
            
            <!-- Inner radial burst -->
            <g :style="{ opacity: 0.5 + audioData.bass * 0.5 }">
              <line
                v-for="(line, i) in centerGeometry.innerLines"
                :key="`line-inner-${i}`"
                :x1="line.x1"
                :y1="line.y1"
                :x2="line.x2"
                :y2="line.y2"
                stroke="url(#grad3)"
                :stroke-width="4 + audioData.treble * 10"
                stroke-linecap="round"
              />
            </g>
            
            <!-- Orbital ring elements -->
            <g v-for="(ring, i) in centerGeometry.orbitRings" :key="`orbit-${i}`">
              <circle
                :cx="ring.x"
                :cy="ring.y"
                :r="ring.size"
                fill="none"
                :stroke="ring.stroke"
                :stroke-width="2 + audioData.overall * 4"
                :opacity="0.4 + audioData.mid * 0.6"
                filter="url(#glow)"
              />
            </g>
            
            <!-- Main morphing center shape -->
            <g :transform="`rotate(${centerGeometry.rotation}) scale(${centerGeometry.scale})`">
              <!-- Outermost explosive layer -->
              <polygon
                :points="centerGeometry.outerShape"
                fill="none"
                stroke="url(#grad1)"
                :stroke-width="5 + audioData.bass * 15"
                :opacity="0.5 + audioData.overall * 0.5"
                filter="url(#glow)"
              />
              
              <!-- Middle morphing layer -->
              <polygon
                :points="centerGeometry.middleShape"
                fill="none"
                stroke="url(#grad2)"
                :stroke-width="4 + audioData.mid * 12"
                :opacity="0.6 + audioData.mid * 0.4"
                filter="url(#glow)"
              />
              
              <!-- Inner sharp layer -->
              <polygon
                :points="centerGeometry.innerShape"
                fill="none"
                stroke="url(#grad3)"
                :stroke-width="3 + audioData.treble * 10"
                :opacity="0.7 + audioData.treble * 0.3"
                filter="url(#glow)"
              />
              
              <!-- Core layer -->
              <polygon
                :points="centerGeometry.coreShape"
                fill="none"
                :stroke="`rgba(255, 255, 255, ${0.8 + audioData.overall * 0.2})`"
                :stroke-width="2 + audioData.overall * 8"
                filter="url(#glow)"
              />
              
              <!-- Brilliant center point -->
              <circle
                cx="0"
                cy="0"
                :r="15 + audioData.overall * 50"
                :fill="`rgba(255, 255, 255, ${0.6 + audioData.overall * 0.4})`"
                filter="url(#blur)"
              />
            </g>
            
            <!-- Massive energy bursts on bass hits -->
            <g :style="{ opacity: audioData.bass * 0.9 }">
              <line
                v-for="(burst, i) in centerGeometry.energyBursts"
                :key="`burst-${i}`"
                :x1="burst.x1"
                :y1="burst.y1"
                :x2="burst.x2"
                :y2="burst.y2"
                stroke="url(#grad3)"
                :stroke-width="5 + audioData.bass * 12"
                stroke-linecap="round"
                :opacity="0.6 + audioData.bass * 0.4"
                filter="url(#glow)"
              />
            </g>
            
            <!-- Treble sparkles -->
            <g :style="{ opacity: 0.4 + audioData.treble * 0.6 }">
              <circle
                v-for="(sparkle, i) in centerGeometry.sparkles"
                :key="`sparkle-${i}`"
                :cx="sparkle.x"
                :cy="sparkle.y"
                :r="sparkle.r"
                :fill="currentPalette.highlight2"
                :opacity="sparkle.opacity"
                filter="url(#blur)"
              />
            </g>
            
            <!-- Swirling particles -->
            <g :style="{ opacity: 0.5 + audioData.mid * 0.5 }">
              <circle
                v-for="(particle, i) in centerGeometry.particles"
                :key="`particle-${i}`"
                :cx="particle.x"
                :cy="particle.y"
                :r="particle.r"
                :fill="currentPalette.primary"
                :opacity="particle.opacity"
              />
            </g>
          </g>
        </g>
      </svg>
    </div>
    
    <!-- Intense flash overlay for extreme bass hits -->
    <div 
      :style="{
        ...styles.flashOverlay,
        background: `radial-gradient(circle, ${currentPalette.primary}66 0%, rgba(10, 10, 10, 0) 60%)`,
        opacity: audioData.bass > 0.6 ? Math.pow((audioData.bass - 0.6) / 0.4, 2) : 0
      }"
    />
    
    <!-- Treble shimmer overlay -->
    <div 
      :style="{
        ...styles.flashOverlay,
        background: `radial-gradient(circle, ${currentPalette.highlight2}33 0%, rgba(10, 10, 10, 0) 70%)`,
        opacity: audioData.treble > 0.7 ? (audioData.treble - 0.7) * 2 : 0
      }"
    />
    
    <div v-if="isDemoMode" :style="styles.demoLabel">
      Demo Mode - Upload audio to visualize your music
    </div>
    
    <div :style="styles.controls">
      <input
        type="file"
        accept="audio/*"
        @change="handleFileUpload"
        :style="styles.fileInput"
        id="audio-upload"
      />
      <label for="audio-upload" :style="styles.uploadLabel">
        Choose Audio File
      </label>
      
      <button v-if="audioElement?.src" @click="togglePlayPause" :style="styles.playButton">
        {{ isPlaying ? 'Pause' : 'Play' }}
      </button>
      
      <button @click="cyclePalette" :style="styles.paletteButton">
        {{ currentPalette.name }}
      </button>
    </div>
    
    <audio ref="audioElement" style="display: none" />
  </div>
</template>

<script setup>
import { ref, reactive, computed, onMounted, onUnmounted } from 'vue';

const colorPalettes = [
  {
    name: 'Purple Dream',
    primary: '#667eea',
    secondary: '#764ba2',
    accent1: '#f093fb',
    accent2: '#f5576c',
    highlight1: '#4facfe',
    highlight2: '#00f2fe'
  },
  {
    name: 'Sunset Blaze',
    primary: '#ff6b6b',
    secondary: '#ee5a6f',
    accent1: '#feca57',
    accent2: '#ff9ff3',
    highlight1: '#ff6348',
    highlight2: '#ffa502'
  },
  {
    name: 'Ocean Deep',
    primary: '#00d2ff',
    secondary: '#3a7bd5',
    accent1: '#00f260',
    accent2: '#0575e6',
    highlight1: '#2af598',
    highlight2: '#009efd'
  },
  {
    name: 'Emerald Forest',
    primary: '#11998e',
    secondary: '#38ef7d',
    accent1: '#56ab2f',
    accent2: '#a8e063',
    highlight1: '#06beb6',
    highlight2: '#48b1bf'
  },
  {
    name: 'Electric Neon',
    primary: '#f953c6',
    secondary: '#b91d73',
    accent1: '#00f5ff',
    accent2: '#fc00ff',
    highlight1: '#ff00cc',
    highlight2: '#333399'
  },
  {
    name: 'Golden Hour',
    primary: '#f7971e',
    secondary: '#ffd200',
    accent1: '#ff6b95',
    accent2: '#ffaa00',
    highlight1: '#ffdd00',
    highlight2: '#fbb034'
  },
  {
    name: 'Arctic Aurora',
    primary: '#a8edea',
    secondary: '#fed6e3',
    accent1: '#c471f5',
    accent2: '#fa709a',
    highlight1: '#30cfd0',
    highlight2: '#330867'
  },
  {
    name: 'Crimson Fire',
    primary: '#eb3349',
    secondary: '#f45c43',
    accent1: '#fc4a1a',
    accent2: '#f7b733',
    highlight1: '#ff0844',
    highlight2: '#ffb199'
  },
  {
    name: 'Cyber Synthwave',
    primary: '#ff00ff',
    secondary: '#7700ff',
    accent1: '#00ffff',
    accent2: '#ff006e',
    highlight1: '#8338ec',
    highlight2: '#3a86ff'
  },
  {
    name: 'Mint Fresh',
    primary: '#00b4db',
    secondary: '#0083b0',
    accent1: '#7bed9f',
    accent2: '#2ed573',
    highlight1: '#70a1ff',
    highlight2: '#5f27cd'
  },
  {
    name: 'Rose Gold',
    primary: '#eb9fac',
    secondary: '#d4a5a5',
    accent1: '#ffafbd',
    accent2: '#ffc3a0',
    highlight1: '#ff9a9e',
    highlight2: '#fad0c4'
  },
  {
    name: 'Cosmic Void',
    primary: '#667eea',
    secondary: '#764ba2',
    accent1: '#fa709a',
    accent2: '#fee140',
    highlight1: '#30cfd0',
    highlight2: '#a8edea'
  }
];

const audioData = reactive({
  bass: 0,
  mid: 0,
  treble: 0,
  overall: 0
});

const isPlaying = ref(false);
const isDemoMode = ref(true);
const currentPaletteIndex = ref(0);

const audioContext = ref(null);
const analyser = ref(null);
const dataArray = ref(null);
const source = ref(null);
const audioElement = ref(null);
const animationFrame = ref(null);
const demoTime = ref(0);
const paletteTimer = ref(null);

const currentPalette = computed(() => colorPalettes[currentPaletteIndex.value]);

const surroundingCells = computed(() => {
  const cells = [];
  const hexWidth = 200;
  const hexHeight = 173.2; // sqrt(3) * 100
  const patterns = ['spiral', 'concentric', 'radial', 'geometric', 'particles', 'wave'];
  
  // Helper to calculate hex position using axial coordinates
  const hexToPixel = (q, r) => {
    const x = 1200 + hexWidth * (3/2 * q);
    const y = 800 + hexHeight * (Math.sqrt(3)/2 * q + Math.sqrt(3) * r);
    return { x, y };
  };
  
  // Generate a massive grid of hexagons
  // Cover from q: -8 to 8, r: -6 to 6
  for (let q = -8; q <= 8; q++) {
    for (let r = -6; r <= 6; r++) {
      // Skip if sum is out of range (creates hexagonal boundary)
      if (Math.abs(q + r) > 7) continue;
      
      // Skip the center cell (that's our large one)
      if (q === 0 && r === 0) continue;
      
      const pos = hexToPixel(q, r);
      
      // Only add cells that are within the visible viewport (with some padding)
      if (pos.x >= -200 && pos.x <= 2600 && pos.y >= -200 && pos.y <= 1800) {
        cells.push({
          x: pos.x,
          y: pos.y,
          pattern: patterns[Math.abs(q + r * 2) % patterns.length]
        });
      }
    }
  }
  
  return cells;
});

const centerGeometry = computed(() => {
  const t = demoTime.value;
  const rotation = audioData.overall * 540;
  const scale = 0.3 + audioData.bass * 1.2;
  
  const morphFactor = audioData.mid * 1.5;
  const spikeFactor = audioData.treble * 2;
  
  const generateMorphShape = (baseSize, spikes, points = 16) => {
    const pts = [];
    for (let i = 0; i < points; i++) {
      const angle = (i / points) * Math.PI * 2 - Math.PI / 2;
      const isSpike = i % 2 === 0;
      const radius = isSpike 
        ? baseSize * (1 + spikes * 0.8)
        : baseSize * (1 - morphFactor * 0.5);
      const x = Math.cos(angle) * radius;
      const y = Math.sin(angle) * radius;
      pts.push(`${x},${y}`);
    }
    return pts.join(' ');
  };
  
  const outerShape = generateMorphShape(350 + audioData.bass * 120, spikeFactor);
  const middleShape = generateMorphShape(250 + audioData.mid * 90, spikeFactor * 0.8);
  const innerShape = generateMorphShape(170 + audioData.treble * 70, spikeFactor * 1.3);
  const coreShape = generateMorphShape(100 + audioData.overall * 50, spikeFactor * 0.6);
  
  const outerLines = [];
  for (let i = 0; i < 32; i++) {
    const angle = (i / 32) * 360 + audioData.treble * 240;
    const r1 = 380 + audioData.bass * 100;
    const r2 = 500 + audioData.overall * 120;
    const x1 = Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = Math.sin((angle * Math.PI) / 180) * r2;
    outerLines.push({ x1, y1, x2, y2 });
  }
  
  const midLines = [];
  for (let i = 0; i < 24; i++) {
    const angle = (i / 24) * 360 + audioData.mid * 200;
    const r1 = 200 + audioData.mid * 80;
    const r2 = 320 + audioData.bass * 100;
    const x1 = Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = Math.sin((angle * Math.PI) / 180) * r2;
    midLines.push({ x1, y1, x2, y2 });
  }
  
  const innerLines = [];
  for (let i = 0; i < 16; i++) {
    const angle = (i / 16) * 360 + audioData.treble * 300;
    const r1 = 100 + audioData.treble * 50;
    const r2 = 180 + audioData.mid * 70;
    const x1 = Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = Math.sin((angle * Math.PI) / 180) * r2;
    innerLines.push({ x1, y1, x2, y2 });
  }
  
  const energyBursts = [];
  for (let i = 0; i < 12; i++) {
    const angle = (i / 12) * 360 + audioData.bass * 720;
    const r1 = 80;
    const r2 = 80 + Math.pow(audioData.bass, 2) * 350;
    const x1 = Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = Math.sin((angle * Math.PI) / 180) * r2;
    energyBursts.push({ x1, y1, x2, y2 });
  }
  
  const orbitRings = [];
  for (let i = 0; i < 18; i++) {
    const angle = (i / 18) * 360 + t * 80 + audioData.mid * 150;
    const radius = 280 + audioData.treble * 100 + Math.sin(t * 2 + i) * 40;
    const x = Math.cos((angle * Math.PI) / 180) * radius;
    const y = Math.sin((angle * Math.PI) / 180) * radius;
    const size = 20 + audioData.bass * 30;
    orbitRings.push({
      x, y, size,
      stroke: i % 3 === 0 ? 'url(#grad1)' : i % 3 === 1 ? 'url(#grad2)' : 'url(#grad3)'
    });
  }
  
  const sparkles = [];
  for (let i = 0; i < 40; i++) {
    const angle = (i / 40) * 360 + t * 150;
    const radius = 250 + Math.sin(t * 4 + i * 0.5) * 120 + audioData.treble * 80;
    const x = Math.cos((angle * Math.PI) / 180) * radius;
    const y = Math.sin((angle * Math.PI) / 180) * radius;
    sparkles.push({
      x, y,
      r: 2 + audioData.treble * 6,
      opacity: 0.3 + Math.sin(t * 5 + i) * 0.3 + audioData.treble * 0.4
    });
  }
  
  const particles = [];
  for (let i = 0; i < 60; i++) {
    const angle = (i / 60) * 360 + t * 100 + audioData.overall * 200;
    const radius = 150 + (i % 3) * 60 + Math.sin(t * 3 + i) * 40;
    const x = Math.cos((angle * Math.PI) / 180) * radius;
    const y = Math.sin((angle * Math.PI) / 180) * radius;
    particles.push({
      x, y,
      r: 1.5 + audioData.mid * 4,
      opacity: 0.2 + audioData.mid * 0.5
    });
  }
  
  return {
    rotation,
    scale,
    outerShape,
    middleShape,
    innerShape,
    coreShape,
    outerLines,
    midLines,
    innerLines,
    energyBursts,
    orbitRings,
    sparkles,
    particles
  };
});

const getCellGeometry = (cell) => {
  const t = demoTime.value;
  
  switch (cell.pattern) {
    case 'spiral':
      return {
        spirals: Array.from({ length: 3 }, (_, i) => {
          let path = 'M 0,0';
          const turns = 2 + audioData.overall * 2;
          const maxRadius = 80 + audioData.bass * 25;
          for (let j = 0; j <= 40; j++) {
            const angle = (j / 40) * turns * Math.PI * 2 + t * 2 + i * Math.PI * 2 / 3;
            const r = (j / 40) * maxRadius;
            const x = Math.cos(angle) * r;
            const y = Math.sin(angle) * r;
            path += ` L ${x},${y}`;
          }
          return { path };
        })
      };
      
    case 'concentric':
      return {
        concentricRings: Array.from({ length: 8 }, (_, i) => ({
          r: 15 + i * 10 + audioData.mid * 6,
          strokeWidth: 0.7 + audioData.bass * 0.8,
          opacity: 0.15 + (audioData.overall * 0.4) * (1 - i / 8)
        }))
      };
      
    case 'radial':
      return {
        rays: Array.from({ length: 12 }, (_, i) => {
          const angle = (i / 12) * 360 + t * 40;
          const r1 = 20;
          const r2 = 70 + audioData.treble * 25;
          return {
            x1: Math.cos((angle * Math.PI) / 180) * r1,
            y1: Math.sin((angle * Math.PI) / 180) * r1,
            x2: Math.cos((angle * Math.PI) / 180) * r2,
            y2: Math.sin((angle * Math.PI) / 180) * r2
          };
        })
      };
      
    case 'geometric':
      return {
        polygons: Array.from({ length: 4 }, (_, i) => {
          const size = 30 + i * 15 + audioData.mid * 8;
          const rotation = t * 25 * (i % 2 === 0 ? 1 : -1);
          const sides = 6;
          const points = [];
          for (let j = 0; j < sides; j++) {
            const angle = (j / sides) * Math.PI * 2 + (rotation * Math.PI / 180);
            const x = Math.cos(angle) * size;
            const y = Math.sin(angle) * size;
            points.push(`${x},${y}`);
          }
          return {
            points: points.join(' '),
            stroke: i % 2 === 0 ? 'url(#grad1)' : 'url(#grad2)',
            strokeWidth: 0.7 + audioData.bass * 0.7,
            opacity: 0.2 + audioData.overall * 0.35
          };
        })
      };
      
    case 'particles':
      return {
        particles: Array.from({ length: 20 }, (_, i) => {
          const angle = (i / 20) * 360 + t * 80;
          const r = 40 + Math.sin(t * 2.5 + i) * 20 + audioData.overall * 15;
          return {
            x: Math.cos((angle * Math.PI) / 180) * r,
            y: Math.sin((angle * Math.PI) / 180) * r,
            r: 1.5 + audioData.treble * 2,
            opacity: 0.2 + audioData.mid * 0.5
          };
        })
      };
      
    case 'wave':
      return {
        waves: Array.from({ length: 5 }, (_, i) => {
          let path = `M -90,${i * 20 - 40}`;
          for (let j = 0; j <= 25; j++) {
            const x = -90 + (j / 25) * 180;
            const y = i * 20 - 40 + Math.sin(j / 3.5 + t * 2 + i) * (10 + audioData.bass * 10);
            path += ` L ${x},${y}`;
          }
          return { path };
        })
      };
      
    default:
      return {};
  }
};

const cyclePalette = () => {
  currentPaletteIndex.value = (currentPaletteIndex.value + 1) % colorPalettes.length;
};

const startPaletteCycle = () => {
  const cycleInterval = 5000 + Math.random() * 3000;
  paletteTimer.value = setTimeout(() => {
    cyclePalette();
    startPaletteCycle();
  }, cycleInterval);
};

const initAudio = async () => {
  if (!audioContext.value) {
    audioContext.value = new (window.AudioContext || window.webkitAudioContext)();
    analyser.value = audioContext.value.createAnalyser();
    analyser.value.fftSize = 512;
    
    const bufferLength = analyser.value.frequencyBinCount;
    dataArray.value = new Uint8Array(bufferLength);
  }
};

const analyzeAudio = () => {
  if (!analyser.value || !dataArray.value) return;
  
  analyser.value.getByteFrequencyData(dataArray.value);
  
  const bass = dataArray.value.slice(0, 15).reduce((a, b) => a + b, 0) / 15;
  const mid = dataArray.value.slice(15, 80).reduce((a, b) => a + b, 0) / 65;
  const treble = dataArray.value.slice(80, 150).reduce((a, b) => a + b, 0) / 70;
  const overall = dataArray.value.reduce((a, b) => a + b, 0) / dataArray.value.length;
  
  const smoothing = 0.7;
  audioData.bass = audioData.bass * smoothing + (bass / 255) * (1 - smoothing);
  audioData.mid = audioData.mid * smoothing + (mid / 255) * (1 - smoothing);
  audioData.treble = audioData.treble * smoothing + (treble / 255) * (1 - smoothing);
  audioData.overall = audioData.overall * smoothing + (overall / 255) * (1 - smoothing);
  
  animationFrame.value = requestAnimationFrame(analyzeAudio);
};

const handleFileUpload = async (e) => {
  const file = e.target.files[0];
  if (!file) return;
  
  isDemoMode.value = false;
  await initAudio();
  
  if (source.value) {
    source.value.disconnect();
  }
  
  const url = URL.createObjectURL(file);
  audioElement.value.src = url;
  
  if (!source.value) {
    source.value = audioContext.value.createMediaElementSource(audioElement.value);
    source.value.connect(analyser.value);
    analyser.value.connect(audioContext.value.destination);
  }
  
  audioElement.value.play();
  isPlaying.value = true;
  analyzeAudio();
};

const togglePlayPause = () => {
  if (isPlaying.value) {
    audioElement.value.pause();
  } else {
    audioElement.value.play();
  }
  isPlaying.value = !isPlaying.value;
};

const animateDemo = () => {
  if (!isDemoMode.value) return;
  
  demoTime.value += 0.016;
  const t = demoTime.value;
  
  audioData.bass = Math.pow((Math.sin(t * 1.5) + 1) / 2, 1.5);
  audioData.mid = Math.pow((Math.sin(t * 2.3) + 1) / 2, 1.2);
  audioData.treble = Math.pow((Math.sin(t * 3.7) + 1) / 2, 1.3);
  audioData.overall = (Math.sin(t * 1.1) + 1) / 2;
  
  animationFrame.value = requestAnimationFrame(animateDemo);
};

const styles = {
  container: {
    width: '100vw',
    height: '100vh',
    display: 'flex',
    flexDirection: 'column',
    alignItems: 'center',
    justifyContent: 'center',
    background: '#0a0a0a',
    fontFamily: '-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif',
    overflow: 'hidden',
    position: 'relative'
  },
  visualizerWrapper: {
    width: '100%',
    height: '100%',
    display: 'flex',
    alignItems: 'center',
    justifyContent: 'center'
  },
  svg: {
    width: '100%',
    height: '100%',
    filter: 'drop-shadow(0 0 40px rgba(102, 126, 234, 0.5))'
  },
  flashOverlay: {
    position: 'fixed',
    top: 0,
    left: 0,
    right: 0,
    bottom: 0,
    pointerEvents: 'none',
    transition: 'opacity 0.05s ease-out'
  },
  demoLabel: {
    position: 'fixed',
    top: '40px',
    color: 'rgba(255, 255, 255, 0.5)',
    fontSize: '14px',
    fontWeight: '400'
  },
  controls: {
    position: 'fixed',
    bottom: '40px',
    display: 'flex',
    gap: '16px',
    zIndex: 10
  },
  fileInput: {
    display: 'none'
  },
  uploadLabel: {
    padding: '12px 24px',
    background: 'rgba(102, 126, 234, 0.2)',
    border: '1px solid rgba(102, 126, 234, 0.4)',
    borderRadius: '8px',
    color: '#fff',
    cursor: 'pointer',
    fontSize: '14px',
    fontWeight: '500',
    transition: 'all 0.2s ease'
  },
  playButton: {
    padding: '12px 24px',
    background: 'rgba(245, 87, 108, 0.2)',
    border: '1px solid rgba(245, 87, 108, 0.4)',
    borderRadius: '8px',
    color: '#fff',
    cursor: 'pointer',
    fontSize: '14px',
    fontWeight: '500',
    transition: 'all 0.2s ease'
  },
  paletteButton: {
    padding: '12px 24px',
    background: 'rgba(255, 255, 255, 0.1)',
    border: '1px solid rgba(255, 255, 255, 0.2)',
    borderRadius: '8px',
    color: '#fff',
    cursor: 'pointer',
    fontSize: '14px',
    fontWeight: '500',
    transition: 'all 0.2s ease'
  }
};

onMounted(() => {
  animateDemo();
  startPaletteCycle();
});

onUnmounted(() => {
  if (animationFrame.value) {
    cancelAnimationFrame(animationFrame.value);
  }
  if (paletteTimer.value) {
    clearTimeout(paletteTimer.value);
  }
});
</script>
