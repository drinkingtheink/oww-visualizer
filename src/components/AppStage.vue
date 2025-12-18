<template>
  <div :style="styles.container">
    <div :style="styles.visualizerWrapper">
      <svg viewBox="0 0 800 800" :style="styles.svg">
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
        </defs>
        
        <!-- Background glow effects -->
        <circle cx="400" cy="400" :r="150 + audioData.bass * 200" fill="url(#glow1)" :opacity="audioData.bass * 0.5" />
        <circle cx="400" cy="400" :r="100 + audioData.treble * 150" fill="url(#glow2)" :opacity="audioData.treble * 0.4" />
        
        <!-- Triple layer fractal lines -->
        <g :style="{ opacity: 0.2 + audioData.treble * 0.5 }">
          <line
            v-for="(line, i) in geometry.lines"
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
            v-for="(line, i) in geometry.midLines"
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
            v-for="(line, i) in geometry.innerLines"
            :key="`line-inner-${i}`"
            :x1="line.x1"
            :y1="line.y1"
            :x2="line.x2"
            :y2="line.y2"
            stroke="url(#grad3)"
            :stroke-width="1.5 + audioData.treble * 2.5"
          />
        </g>
        
        <!-- Dynamic center shape - morphs between hexagon and star -->
        <g :transform="`translate(400, 400) rotate(${geometry.hexRotation}) scale(${geometry.hexScale})`">
          <!-- Outer pulsing hexagon -->
          <polygon
            :points="geometry.outerShape"
            fill="none"
            stroke="url(#grad1)"
            :stroke-width="3 + audioData.bass * 6"
            :opacity="0.6 + audioData.overall * 0.4"
          />
          
          <!-- Middle morphing shape -->
          <polygon
            :points="geometry.middleShape"
            fill="none"
            stroke="url(#grad2)"
            :stroke-width="2 + audioData.mid * 5"
            :opacity="0.7 + audioData.mid * 0.3"
          />
          
          <!-- Inner star burst -->
          <polygon
            :points="geometry.innerShape"
            fill="none"
            stroke="url(#grad3)"
            :stroke-width="1.5 + audioData.treble * 4"
            :opacity="0.8 + audioData.treble * 0.2"
          />
          
          <!-- Center glow point -->
          <circle
            cx="0"
            cy="0"
            :r="5 + audioData.overall * 20"
            :fill="`rgba(255, 255, 255, ${audioData.overall * 0.8})`"
            filter="url(#blur)"
          />
        </g>
        
        <!-- Triple layer outer rings -->
        <g
          v-for="(ring, i) in geometry.rings"
          :key="`ring-outer-${i}`"
          :transform="`translate(${ring.x}, ${ring.y}) rotate(${ring.angle})`"
          :style="{ opacity: 0.3 + audioData.overall * 0.6 }"
        >
          <circle
            cx="0"
            cy="0"
            :r="ring.size * 1.2"
            fill="none"
            stroke="url(#grad1)"
            :stroke-width="0.5 + audioData.bass * 1.5"
          />
          <circle
            cx="0"
            cy="0"
            :r="ring.size"
            fill="none"
            stroke="url(#grad2)"
            :stroke-width="1 + audioData.treble * 2"
          />
          <circle
            cx="0"
            cy="0"
            :r="ring.size * 0.6"
            fill="none"
            stroke="url(#grad3)"
            :stroke-width="0.5 + audioData.mid * 1.5"
          />
        </g>
        
        <!-- Additional mid-layer rings -->
        <g
          v-for="(ring, i) in geometry.midRings"
          :key="`ring-mid-${i}`"
          :transform="`translate(${ring.x}, ${ring.y}) rotate(${ring.angle})`"
          :style="{ opacity: 0.4 + audioData.mid * 0.5 }"
        >
          <circle
            cx="0"
            cy="0"
            :r="ring.size"
            fill="none"
            stroke="url(#grad2)"
            :stroke-width="1.5 + audioData.mid * 2.5"
          />
          <polygon
            :points="generateSmallHex(ring.size * 0.7)"
            fill="none"
            stroke="url(#grad3)"
            :stroke-width="0.5 + audioData.bass * 1"
          />
        </g>
        
        <!-- Connecting arcs - multiple layers -->
        <g :style="{ opacity: 0.2 + audioData.mid * 0.4 }">
          <line
            v-for="(ring, i) in geometry.rings"
            :key="`arc-outer-${i}`"
            :x1="ring.x"
            :y1="ring.y"
            :x2="geometry.rings[(i + 1) % geometry.rings.length].x"
            :y2="geometry.rings[(i + 1) % geometry.rings.length].y"
            stroke="url(#grad1)"
            :stroke-width="0.5 + audioData.overall * 1.5"
          />
        </g>
        
        <g :style="{ opacity: 0.3 + audioData.bass * 0.4 }">
          <line
            v-for="(ring, i) in geometry.midRings"
            :key="`arc-mid-${i}`"
            :x1="ring.x"
            :y1="ring.y"
            :x2="geometry.midRings[(i + 1) % geometry.midRings.length].x"
            :y2="geometry.midRings[(i + 1) % geometry.midRings.length].y"
            stroke="url(#grad2)"
            :stroke-width="0.5 + audioData.bass * 2"
          />
        </g>
        
        <!-- Energy burst lines on bass hits -->
        <g :style="{ opacity: audioData.bass * 0.7 }">
          <line
            v-for="(burst, i) in geometry.energyBursts"
            :key="`burst-${i}`"
            :x1="burst.x1"
            :y1="burst.y1"
            :x2="burst.x2"
            :y2="burst.y2"
            stroke="url(#grad3)"
            :stroke-width="2 + audioData.bass * 3"
            stroke-linecap="round"
          />
        </g>
        
        <!-- Outer orbital particles -->
        <g :style="{ opacity: 0.6 + audioData.treble * 0.4 }">
          <circle
            v-for="(particle, i) in geometry.particles"
            :key="`particle-${i}`"
            :cx="particle.x"
            :cy="particle.y"
            :r="2 + audioData.overall * 4"
            :fill="currentPalette.primary"
            :style="`opacity: ${0.6 + audioData.treble * 0.4}`"
            filter="url(#blur)"
          />
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
        {{ currentPaletteIndex + 1 }}/12
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

const currentPalette = computed(() => colorPalettes[currentPaletteIndex.value]);

const cyclePalette = () => {
  currentPaletteIndex.value = (currentPaletteIndex.value + 1) % colorPalettes.length;
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

const generateSmallHex = (size) => {
  const points = [];
  for (let i = 0; i < 6; i++) {
    const angle = (i / 6) * Math.PI * 2 - Math.PI / 2;
    const x = Math.cos(angle) * size;
    const y = Math.sin(angle) * size;
    points.push(`${x},${y}`);
  }
  return points.join(' ');
};

const geometry = computed(() => {
  const hexRotation = audioData.overall * 360;
  const hexScale = 0.6 + audioData.bass * 0.8;
  
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
  
  const outerShape = generateMorphShape(120 + audioData.bass * 40, spikeFactor);
  const middleShape = generateMorphShape(80 + audioData.mid * 30, spikeFactor * 0.7);
  const innerShape = generateMorphShape(50 + audioData.treble * 20, spikeFactor * 1.2);
  
  const ringCount = 6;
  const rings = [];
  for (let i = 0; i < ringCount; i++) {
    const angle = (i / ringCount) * 360 + audioData.mid * 90;
    const radius = 240 + audioData.treble * 100;
    const x = 400 + Math.cos((angle * Math.PI) / 180) * radius;
    const y = 400 + Math.sin((angle * Math.PI) / 180) * radius;
    const size = 50 + audioData.bass * 40;
    rings.push({ x, y, size, angle: angle + audioData.overall * 240 });
  }
  
  const midRings = [];
  for (let i = 0; i < 12; i++) {
    const angle = (i / 12) * 360 + audioData.bass * 120;
    const radius = 160 + audioData.mid * 60;
    const x = 400 + Math.cos((angle * Math.PI) / 180) * radius;
    const y = 400 + Math.sin((angle * Math.PI) / 180) * radius;
    const size = 25 + audioData.treble * 20;
    midRings.push({ x, y, size, angle: angle - audioData.overall * 180 });
  }
  
  const lineCount = 24;
  const lines = [];
  for (let i = 0; i < lineCount; i++) {
    const angle = (i / lineCount) * 360 + audioData.treble * 180;
    const r1 = 280 + audioData.bass * 60;
    const r2 = 360 + audioData.overall * 80;
    const x1 = 400 + Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = 400 + Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = 400 + Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = 400 + Math.sin((angle * Math.PI) / 180) * r2;
    lines.push({ x1, y1, x2, y2 });
  }
  
  const midLines = [];
  for (let i = 0; i < 18; i++) {
    const angle = (i / 18) * 360 + audioData.mid * 150;
    const r1 = 150 + audioData.mid * 40;
    const r2 = 220 + audioData.bass * 50;
    const x1 = 400 + Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = 400 + Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = 400 + Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = 400 + Math.sin((angle * Math.PI) / 180) * r2;
    midLines.push({ x1, y1, x2, y2 });
  }
  
  const innerLines = [];
  for (let i = 0; i < 12; i++) {
    const angle = (i / 12) * 360 + audioData.treble * 200;
    const r1 = 80 + audioData.treble * 30;
    const r2 = 130 + audioData.mid * 40;
    const x1 = 400 + Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = 400 + Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = 400 + Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = 400 + Math.sin((angle * Math.PI) / 180) * r2;
    innerLines.push({ x1, y1, x2, y2 });
  }
  
  const energyBursts = [];
  for (let i = 0; i < 8; i++) {
    const angle = (i / 8) * 360 + audioData.bass * 360;
    const r1 = 50;
    const r2 = 50 + audioData.bass * 150;
    const x1 = 400 + Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = 400 + Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = 400 + Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = 400 + Math.sin((angle * Math.PI) / 180) * r2;
    energyBursts.push({ x1, y1, x2, y2 });
  }
  
  const particles = [];
  for (let i = 0; i < 16; i++) {
    const angle = (i / 16) * 360 + audioData.overall * 720;
    const radius = 320 + Math.sin(audioData.treble * Math.PI * 2 + i) * 40;
    const x = 400 + Math.cos((angle * Math.PI) / 180) * radius;
    const y = 400 + Math.sin((angle * Math.PI) / 180) * radius;
    particles.push({ x, y });
  }
  
  return {
    hexRotation,
    hexScale,
    outerShape,
    middleShape,
    innerShape,
    rings,
    midRings,
    lines,
    midLines,
    innerLines,
    energyBursts,
    particles
  };
});

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
    width: '95vmin',
    height: '95vmin',
    maxWidth: '900px',
    maxHeight: '900px',
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
});

onUnmounted(() => {
  if (animationFrame.value) {
    cancelAnimationFrame(animationFrame.value);
  }
});
</script>