<template>
  <div :style="styles.container">
    <div :style="styles.visualizerWrapper">
      <svg viewBox="0 0 800 800" :style="styles.svg">
        <defs>
          <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color: #667eea; stop-opacity: 0.8" />
            <stop offset="100%" style="stop-color: #764ba2; stop-opacity: 0.8" />
          </linearGradient>
          
          <linearGradient id="grad2" x1="0%" y1="0%" x2="0%" y2="100%">
            <stop offset="0%" style="stop-color: #f093fb; stop-opacity: 0.6" />
            <stop offset="100%" style="stop-color: #f5576c; stop-opacity: 0.6" />
          </linearGradient>
        </defs>
        
        <g :style="{ opacity: 0.3 + audioData.treble * 0.4 }">
          <line
            v-for="(line, i) in geometry.lines"
            :key="`line-${i}`"
            :x1="line.x1"
            :y1="line.y1"
            :x2="line.x2"
            :y2="line.y2"
            stroke="url(#grad1)"
            :stroke-width="1 + audioData.mid * 2"
          />
        </g>
        
        <g :transform="`translate(400, 400) rotate(${geometry.hexRotation}) scale(${geometry.hexScale})`">
          <polygon
            points="0,-100 86.6,-50 86.6,50 0,100 -86.6,50 -86.6,-50"
            fill="none"
            stroke="url(#grad1)"
            :stroke-width="2 + audioData.bass * 4"
          />
          <polygon
            points="0,-70 60.6,-35 60.6,35 0,70 -60.6,35 -60.6,-35"
            fill="none"
            stroke="url(#grad2)"
            :stroke-width="1.5 + audioData.mid * 3"
          />
        </g>
        
        <g
          v-for="(ring, i) in geometry.rings"
          :key="`ring-${i}`"
          :transform="`translate(${ring.x}, ${ring.y}) rotate(${ring.angle})`"
          :style="{ opacity: 0.4 + audioData.overall * 0.6 }"
        >
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
            stroke="url(#grad1)"
            :stroke-width="0.5 + audioData.bass * 1.5"
          />
        </g>
        
        <g :style="{ opacity: 0.2 + audioData.mid * 0.5 }">
          <line
            v-for="(ring, i) in geometry.rings"
            :key="`arc-${i}`"
            :x1="ring.x"
            :y1="ring.y"
            :x2="geometry.rings[(i + 1) % geometry.rings.length].x"
            :y2="geometry.rings[(i + 1) % geometry.rings.length].y"
            stroke="url(#grad1)"
            :stroke-width="0.5 + audioData.overall * 1.5"
          />
        </g>
      </svg>
    </div>
    
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
    </div>
    
    <audio ref="audioElement" style="display: none" />
  </div>
</template>

<script setup>
import { ref, reactive, computed, onMounted, onUnmounted } from 'vue';

const audioData = reactive({
  bass: 0,
  mid: 0,
  treble: 0,
  overall: 0
});

const isPlaying = ref(false);
const isDemoMode = ref(true);

const audioContext = ref(null);
const analyser = ref(null);
const dataArray = ref(null);
const source = ref(null);
const audioElement = ref(null);
const animationFrame = ref(null);
const demoTime = ref(0);

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

const geometry = computed(() => {
  const hexRotation = audioData.overall * 360;
  const hexScale = 0.8 + audioData.bass * 0.4;
  
  const ringCount = 6;
  const rings = [];
  
  for (let i = 0; i < ringCount; i++) {
    const angle = (i / ringCount) * 360 + audioData.mid * 60;
    const radius = 200 + audioData.treble * 80;
    const x = 400 + Math.cos((angle * Math.PI) / 180) * radius;
    const y = 400 + Math.sin((angle * Math.PI) / 180) * radius;
    const size = 40 + audioData.bass * 30;
    
    rings.push({ x, y, size, angle: angle + audioData.overall * 180 });
  }
  
  const lineCount = 12;
  const lines = [];
  
  for (let i = 0; i < lineCount; i++) {
    const angle = (i / lineCount) * 360 + audioData.treble * 120;
    const r1 = 100 + audioData.mid * 50;
    const r2 = 180 + audioData.bass * 60;
    
    const x1 = 400 + Math.cos((angle * Math.PI) / 180) * r1;
    const y1 = 400 + Math.sin((angle * Math.PI) / 180) * r1;
    const x2 = 400 + Math.cos((angle * Math.PI) / 180) * r2;
    const y2 = 400 + Math.sin((angle * Math.PI) / 180) * r2;
    
    lines.push({ x1, y1, x2, y2 });
  }
  
  return { hexRotation, hexScale, rings, lines };
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
    overflow: 'hidden'
  },
  visualizerWrapper: {
    width: '90vmin',
    height: '90vmin',
    maxWidth: '800px',
    maxHeight: '800px',
    display: 'flex',
    alignItems: 'center',
    justifyContent: 'center'
  },
  svg: {
    width: '100%',
    height: '100%',
    filter: 'drop-shadow(0 0 20px rgba(102, 126, 234, 0.3))'
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