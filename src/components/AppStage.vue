<template>
  <div :style="styles.container">
    <div :style="styles.visualizerWrapper">
      <svg viewBox="0 0 1600 1200" :style="styles.svg">
        <defs>
          <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" :style="`stop-color: ${currentPalette.primary}; stop-opacity: 0.8`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.secondary}; stop-opacity: 0.8`" />
          </linearGradient>
          
          <linearGradient id="grad2" x1="0%" y1="0%" x2="0%" y2="100%">
            <stop offset="0%" :style="`stop-color: ${currentPalette.accent1}; stop-opacity: 0.6`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.accent2}; stop-opacity: 0.6`" />
          </linearGradient>
          
          <linearGradient id="grad3" x1="0%" y1="0%" x2="100%" y2="0%">
            <stop offset="0%" :style="`stop-color: ${currentPalette.highlight1}; stop-opacity: 0.7`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.highlight2}; stop-opacity: 0.7`" />
          </linearGradient>
          
          <radialGradient id="glow1">
            <stop offset="0%" :style="`stop-color: ${currentPalette.primary}; stop-opacity: ${audioData.bass * 0.8}`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.primary}; stop-opacity: 0`" />
          </radialGradient>
          
          <radialGradient id="glow2">
            <stop offset="0%" :style="`stop-color: ${currentPalette.accent2}; stop-opacity: ${audioData.treble * 0.6}`" />
            <stop offset="100%" :style="`stop-color: ${currentPalette.accent2}; stop-opacity: 0`" />
          </radialGradient>
          
          <filter id="blur">
            <feGaussianBlur :stdDeviation="2 + audioData.overall * 4" />
          </filter>
          
          <clipPath id="hexClip">
            <polygon points="0,-115.47 100,-57.735 100,57.735 0,115.47 -100,57.735 -100,-57.735" />
          </clipPath>
        </defs>
        
        <!-- Render honeycomb cells -->
        <g v-for="(cell, idx) in honeycombCells" :key="`cell-${idx}`">
          <g :transform="`translate(${cell.x}, ${cell.y})`">
            <!-- Hexagon border -->
            <polygon
              points="0,-115.47 100,-57.735 100,57.735 0,115.47 -100,57.735 -100,-57.735"
              fill="none"
              :stroke="currentPalette.primary"
              :stroke-width="cell.isCenter ? 3 : 1"
              :opacity="cell.isCenter ? 0.6 : 0.2"
            />
            
            <!-- Clipped content -->
            <g clip-path="url(#hexClip)">
              <!-- Background glow for this cell -->
              <circle 
                cx="0" 
                cy="0" 
                :r="80 + audioData.bass * 100" 
                fill="url(#glow1)" 
                :opacity="cell.isCenter ? audioData.bass * 0.5 : audioData.bass * 0.2" 
              />
              <circle 
                cx="0" 
                cy="0" 
                :r="60 + audioData.treble * 80" 
                fill="url(#glow2)" 
                :opacity="cell.isCenter ? audioData.treble * 0.4 : audioData.treble * 0.15" 
              />
              
              <!-- Cell-specific visualization -->
              <g v-if="cell.isCenter">
                <!-- CENTER CELL - Main visualization -->
                <g :style="{ opacity: 0.2 + audioData.treble * 0.5 }">
                  <line
                    v-for="(line, i) in getCellGeometry(cell).lines"
                    :key="`line-outer-${i}`"
                    :x1="line.x1"
                    :y1="line.y1"
                    :x2="line.x2"
                    :y2="line.y2"
                    stroke="url(#grad1)"
                    :stroke-width="0.5 + audioData.mid * 1.5"
                  />
                </g>
                
                <g :style="{ opacity: 0.3 + audioData.mid * 0.6 }">
                  <line
                    v-for="(line, i) in getCellGeometry(cell).midLines"
                    :key="`line-mid-${i}`"
                    :x1="line.x1"
                    :y1="line.y1"
                    :x2="line.x2"
                    :y2="line.y2"
                    stroke="url(#grad2)"
                    :stroke-width="1 + audioData.bass * 2"
                  />
                </g>
                
                <g :style="{ opacity: 0.4 + audioData.bass * 0.5 }">
                  <line
                    v-for="(line, i) in getCellGeometry(cell).innerLines"
                    :key="`line-inner-${i}`"
                    :x1="line.x1"
                    :y1="line.y1"
                    :x2="line.x2"
                    :y2="line.y2"
                    stroke="url(#grad3)"
                    :stroke-width="1.5 + audioData.treble * 2.5"
                  />
                </g>
                
                <g :transform="`rotate(${getCellGeometry(cell).rotation}) scale(${getCellGeometry(cell).scale})`">
                  <polygon
                    :points="getCellGeometry(cell).outerShape"
                    fill="none"
                    stroke="url(#grad1)"
                    :stroke-width="2 + audioData.bass * 4"
                    :opacity="0.6 + audioData.overall * 0.4"
                  />
                  <polygon
                    :points="getCellGeometry(cell).middleShape"
                    fill="none"
                    stroke="url(#grad2)"
                    :stroke-width="1.5 + audioData.mid * 3"
                    :opacity="0.7 + audioData.mid * 0.3"
                  />
                  <polygon
                    :points="getCellGeometry(cell).innerShape"
                    fill="none"
                    stroke="url(#grad3)"
                    :stroke-width="1 + audioData.treble * 2.5"
                    :opacity="0.8 + audioData.treble * 0.2"
                  />
                  <circle
                    cx="0"
                    cy="0"
                    :r="3 + audioData.overall * 12"
                    :fill="`rgba(255, 255, 255, ${audioData.overall * 0.8})`"
                    filter="url(#blur)"
                  />
                </g>
                
                <g :style="{ opacity: audioData.bass * 0.7 }">
                  <line
                    v-for="(burst, i) in getCellGeometry(cell).energyBursts"
                    :key="`burst-${i}`"
                    :x1="burst.x1"
                    :y1="burst.y1"
                    :x2="burst.x2"
                    :y2="burst.y2"
                    stroke="url(#grad3)"
                    :stroke-width="1.5 + audioData.bass * 2"
                    stroke-linecap="round"
                  />
                </g>
              </g>
              
              <g v-else>
                <!-- SURROUNDING CELLS - Different pattern types -->
                <g v-if="cell.pattern === 'spiral'">
                  <path
                    v-for="(spiral, i) in getCellGeometry(cell).spirals"
                    :key="`spiral-${i}`"
                    :d="spiral.path"
                    fill="none"
                    stroke="url(#grad1)"
                    :stroke-width="0.5 + audioData.mid * 1"
                    :opacity="0.3 + audioData.overall * 0.4"
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
                    :stroke-width="0.5 + audioData.treble * 1"
                    :opacity="0.3 + audioData.mid * 0.5"
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
                    :stroke-width="0.5 + audioData.bass * 1"
                    :opacity="0.3 + audioData.treble * 0.4"
                  />
                </g>
              </g>
            </g>
          </g>
        </g>
      </svg>
    </div>
    
    <!-- Flash overlay for extreme bass hits -->
    <div 
      :style="{
        ...styles.flashOverlay,
        background: `radial-gradient(circle, ${currentPalette.primary}33 0%, rgba(10, 10, 10, 0) 70%)`,
        opacity: audioData.bass > 0.7 ? (audioData.bass - 0.7) * 2 : 0
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

const honeycombCells = computed(() => {
  const cells = [];
  const hexWidth = 200;
  const hexHeight = 173.2; // sqrt(3) * 100
  const patterns = ['spiral', 'concentric', 'radial', 'geometric', 'particles', 'wave'];
  
  // Center cell
  cells.push({
    x: 800,
    y: 600,
    isCenter: true,
    pattern: null
  });
  
  // First ring (6 cells)
  for (let i = 0; i < 6; i++) {
    const angle = (i * 60) * Math.PI / 180;
    cells.push({
      x: 800 + Math.cos(angle) * hexWidth,
      y: 600 + Math.sin(angle) * hexHeight,
      isCenter: false,
      pattern: patterns[i % patterns.length]
    });
  }
  
  // Second ring (12 cells)
  const ring2Positions = [
    [2, 0], [1, 1], [0, 2], [-1, 1], [-2, 0], [-1, -1],
    [0, -2], [1, -1], [2, -2], [2, 2], [-2, 2], [-2, -2]
  ];
  
  ring2Positions.forEach(([q, r], i) => {
    const x = 800 + hexWidth * (3/2 * q);
    const y = 600 + hexHeight * (Math.sqrt(3)/2 * q + Math.sqrt(3) * r);
    cells.push({
      x,
      y,
      isCenter: false,
      pattern: patterns[i % patterns.length]
    });
  });
  
  return cells;
});

const getCellGeometry = (cell) => {
  if (cell.isCenter) {
    return generateCenterGeometry();
  } else {
    return generateSurroundingGeometry(cell);
  }
};

const generateCenterGeometry = () => {
  const rotation = audioData.overall * 360;
  const scale = 0.4 + audioData.bass * 0.4;
  
  const morphFactor = audioData.mid;
  const spikeFactor = audioData.treble;
  
  const generateMorphShape = (baseSize, spikes) => {
    const points = [];
    for (let i = 0; i < 12; i++) {
      const angle = (i / 12) * Math.PI * 2 - Math.PI / 2;
      const isSpike = i % 2 === 0;
      const radius = isSpike 
        ? baseSize * (1 + spikes * 0.5)
        : baseSize * (1 - morphFactor * 0.3);
      const x = Math.cos(angle) * radius;
      const y = Math.sin(angle) * radius;
      points.push(`${x},${y}`);
    }
    return points.join(' ');
  };
  
  const outerShape = generateMorphShape(80 + audioData.bass * 25, spikeFactor);
  const middleShape = generateMorphShape(55 + audioData.mid * 20, spikeFactor * 0.7);
  const innerShape = generateMorphShape(35 + audioData.treble * 15, spikeFactor * 1.2);
  
  const lineCount = 16;
  const lines = [];
  for (let i = 0; i < lineCount; i++) {
    const angle = (i / lineCount) * 360 + audioData.treble * 120;
    const r1 = 70 + audioData.bass * 20;
    const r2 = 100 + audioData.overall * 15;
    const x1 = Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = Math.sin((angle * Math.PI) / 180) * r2;
    lines.push({ x1, y1, x2, y2 });
  }
  
  const midLines = [];
  for (let i = 0; i < 12; i++) {
    const angle = (i / 12) * 360 + audioData.mid * 100;
    const r1 = 40 + audioData.mid * 15;
    const r2 = 60 + audioData.bass * 20;
    const x1 = Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = Math.sin((angle * Math.PI) / 180) * r2;
    midLines.push({ x1, y1, x2, y2 });
  }
  
  const innerLines = [];
  for (let i = 0; i < 8; i++) {
    const angle = (i / 8) * 360 + audioData.treble * 150;
    const r1 = 20 + audioData.treble * 10;
    const r2 = 35 + audioData.mid * 15;
    const x1 = Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = Math.sin((angle * Math.PI) / 180) * r2;
    innerLines.push({ x1, y1, x2, y2 });
  }
  
  const energyBursts = [];
  for (let i = 0; i < 6; i++) {
    const angle = (i / 6) * 360 + audioData.bass * 360;
    const r1 = 30;
    const r2 = 30 + audioData.bass * 60;
    const x1 = Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = Math.sin((angle * Math.PI) / 180) * r2;
    energyBursts.push({ x1, y1, x2, y2 });
  }
  
  return {
    rotation,
    scale,
    outerShape,
    middleShape,
    innerShape,
    lines,
    midLines,
    innerLines,
    energyBursts
  };
};

const generateSurroundingGeometry = (cell) => {
  const t = demoTime.value;
  
  switch (cell.pattern) {
    case 'spiral':
      return {
        spirals: Array.from({ length: 3 }, (_, i) => {
          let path = 'M 0,0';
          const turns = 2 + audioData.overall * 2;
          const maxRadius = 80 + audioData.bass * 20;
          for (let j = 0; j <= 50; j++) {
            const angle = (j / 50) * turns * Math.PI * 2 + t * 2 + i * Math.PI * 2 / 3;
            const r = (j / 50) * maxRadius;
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
          r: 15 + i * 10 + audioData.mid * 5,
          strokeWidth: 0.5 + audioData.bass * 0.5,
          opacity: 0.2 + (audioData.overall * 0.4) * (1 - i / 8)
        }))
      };
      
    case 'radial':
      return {
        rays: Array.from({ length: 12 }, (_, i) => {
          const angle = (i / 12) * 360 + t * 50;
          const r1 = 20;
          const r2 = 60 + audioData.treble * 30;
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
          const size = 30 + i * 15 + audioData.mid * 10;
          const rotation = t * 30 * (i % 2 === 0 ? 1 : -1);
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
            strokeWidth: 0.5 + audioData.bass * 0.5,
            opacity: 0.3 + audioData.overall * 0.3
          };
        })
      };
      
    case 'particles':
      return {
        particles: Array.from({ length: 20 }, (_, i) => {
          const angle = (i / 20) * 360 + t * 100;
          const r = 30 + Math.sin(t * 3 + i) * 20 + audioData.overall * 20;
          return {
            x: Math.cos((angle * Math.PI) / 180) * r,
            y: Math.sin((angle * Math.PI) / 180) * r,
            r: 1 + audioData.treble * 2,
            opacity: 0.3 + audioData.mid * 0.5
          };
        })
      };
      
    case 'wave':
      return {
        waves: Array.from({ length: 5 }, (_, i) => {
          let path = `M -80,${i * 15 - 30}`;
          for (let j = 0; j <= 20; j++) {
            const x = -80 + (j / 20) * 160;
            const y = i * 15 - 30 + Math.sin(j / 3 + t * 2 + i) * (10 + audioData.bass * 10);
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
  const cycleInterval = 5000 + Math.random() * 3000; // 5-8 seconds
  paletteTimer.value = setTimeout(() => {
    cyclePalette();
    startPaletteCycle();
  }, cycleInterval);
};

const initAudio = async () => {
  if (!audioContext.value) {
    audioContext.value = new (window.AudioContext || window.webkitAudioContext)();
    analyser.value = audioContext.value.createAnalyser();
    analyser.value.fftSize = 256;
    
    const bufferLength = analyser.value.frequencyBinCount;
    dataArray.value = new Uint8Array(bufferLength);
  }
};

const analyzeAudio = () => {
  if (!analyser.value || !dataArray.value) return;
  
  analyser.value.getByteFrequencyData(dataArray.value);
  
  const bass = dataArray.value.slice(0, 10).reduce((a, b) => a + b, 0) / 10;
  const mid = dataArray.value.slice(10, 50).reduce((a, b) => a + b, 0) / 40;
  const treble = dataArray.value.slice(50, 100).reduce((a, b) => a + b, 0) / 50;
  const overall = dataArray.value.reduce((a, b) => a + b, 0) / dataArray.value.length;
  
  audioData.bass = bass / 255;
  audioData.mid = mid / 255;
  audioData.treble = treble / 255;
  audioData.overall = overall / 255;
  
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
  
  audioData.bass = (Math.sin(t * 1.5) + 1) / 2;
  audioData.mid = (Math.sin(t * 2.3) + 1) / 2;
  audioData.treble = (Math.sin(t * 3.7) + 1) / 2;
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
    filter: 'drop-shadow(0 0 30px rgba(102, 126, 234, 0.4))'
  },
  flashOverlay: {
    position: 'fixed',
    top: 0,
    left: 0,
    right: 0,
    bottom: 0,
    pointerEvents: 'none',
    transition: 'opacity 0.1s ease-out'
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
