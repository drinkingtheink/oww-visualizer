<template>
  <div class="music-player" :class="{ collapsed: isCollapsed }">
    <!-- Collapsed State -->
    <div v-if="isCollapsed" class="collapsed-view">
      <div class="collapsed-top">
        <div class="track-info">
          <div class="track-number">{{ currentTrackIndex + 1 }}</div>
          <div class="track-name">{{ tracks[currentTrackIndex].name }}</div>
        </div>
        <div class="mini-controls">
        <button @click="previousTrack" class="control-btn" title="Previous">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="currentColor">
            <path d="M6 6h2v12H6zm3.5 6l8.5 6V6z"/>
          </svg>
        </button>
        <button @click="togglePlayPause" class="control-btn play-pause" title="Play/Pause">
          <svg v-if="!isPlaying" width="12" height="12" viewBox="0 0 24 24" fill="currentColor">
            <path d="M8 5v14l11-7z"/>
          </svg>
          <svg v-else width="12" height="12" viewBox="0 0 24 24" fill="currentColor">
            <path d="M6 4h4v16H6V4zm8 0h4v16h-4V4z"/>
          </svg>
        </button>
        <button @click="nextTrack" class="control-btn" title="Next">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="currentColor">
            <path d="M6 18l8.5-6L6 6v12zM16 6v12h2V6h-2z"/>
          </svg>
        </button>
        </div>
        <button @click="toggleCollapse" class="expand-btn" title="Expand">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor">
            <path d="M10 17l5-5-5-5v10z"/>
          </svg>
        </button>
      </div>
      <!-- Progress Bar -->
      <div
        class="progress-bar mini-progress"
        @click="handleProgressClick"
        @touchstart.prevent="handleProgressTouch"
        @touchmove.prevent="handleProgressTouch"
      >
        <div class="progress-fill" :style="{ width: progressPercent + '%' }"></div>
      </div>
    </div>

    <!-- Expanded State -->
    <div v-else class="expanded-view">
      <div class="player-header">
        <h3>Let Slip by One Wax Wing</h3>
        <button @click="toggleCollapse" class="collapse-btn" title="Collapse">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor">
            <path d="M14 17l-5-5 5-5v10z"/>
          </svg>
        </button>
      </div>

      <div class="track-list">
        <div
          v-for="(track, index) in tracks"
          :key="index"
          class="track-item"
          :class="{ active: currentTrackIndex === index }"
          @click="selectTrack(index)"
        >
          <div class="track-number">{{ index + 1 }}</div>
          <div class="track-name">{{ track.name }}</div>
          <div v-if="currentTrackIndex === index && isPlaying" class="playing-indicator">
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
      </div>

      <!-- Progress Bar with Time -->
      <div class="progress-container">
        <span class="time-display">{{ formatTime(currentTime) }}</span>
        <div
          class="progress-bar"
          @click="handleProgressClick"
          @touchstart.prevent="handleProgressTouch"
          @touchmove.prevent="handleProgressTouch"
        >
          <div class="progress-fill" :style="{ width: progressPercent + '%' }"></div>
        </div>
        <span class="time-display">{{ formatTime(duration) }}</span>
      </div>

      <div class="controls">
        <button @click="previousTrack" class="control-btn" title="Previous">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
            <path d="M6 6h2v12H6zm3.5 6l8.5 6V6z"/>
          </svg>
        </button>
        <button @click="togglePlayPause" class="control-btn play-pause" title="Play/Pause">
          <svg v-if="!isPlaying" width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
            <path d="M8 5v14l11-7z"/>
          </svg>
          <svg v-else width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
            <path d="M6 4h4v16H6V4zm8 0h4v16h-4V4z"/>
          </svg>
        </button>
        <button @click="nextTrack" class="control-btn" title="Next">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
            <path d="M6 18l8.5-6L6 6v12zM16 6v12h2V6h-2z"/>
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, defineProps, defineEmits } from 'vue';

const props = defineProps({
  tracks: {
    type: Array,
    required: true
  },
  currentTrackIndex: {
    type: Number,
    default: 0
  },
  isPlaying: {
    type: Boolean,
    default: false
  },
  currentTime: {
    type: Number,
    default: 0
  },
  duration: {
    type: Number,
    default: 0
  }
});

const emit = defineEmits(['track-change', 'play-pause', 'next', 'previous', 'seek']);

const isCollapsed = ref(false);

function toggleCollapse() {
  isCollapsed.value = !isCollapsed.value;
}

function selectTrack(index) {
  emit('track-change', index);
}

function togglePlayPause() {
  emit('play-pause');
}

function nextTrack() {
  emit('next');
}

function previousTrack() {
  emit('previous');
}

// Progress bar functionality
const progressPercent = computed(() => {
  if (props.duration <= 0) return 0;
  return (props.currentTime / props.duration) * 100;
});

function formatTime(seconds) {
  if (!seconds || isNaN(seconds)) return '0:00';
  const mins = Math.floor(seconds / 60);
  const secs = Math.floor(seconds % 60);
  return `${mins}:${secs.toString().padStart(2, '0')}`;
}

function handleProgressClick(event) {
  const bar = event.currentTarget;
  const rect = bar.getBoundingClientRect();
  const clickX = event.clientX - rect.left;
  const percent = clickX / rect.width;
  const newTime = percent * props.duration;
  emit('seek', newTime);
}

function handleProgressTouch(event) {
  const bar = event.currentTarget;
  const rect = bar.getBoundingClientRect();
  const touch = event.touches[0];
  const touchX = touch.clientX - rect.left;
  const percent = Math.max(0, Math.min(1, touchX / rect.width));
  const newTime = percent * props.duration;
  emit('seek', newTime);
}
</script>

<style scoped>
.music-player {
  position: fixed;
  top: 22px;
  right: 20px;
  background: rgba(20, 20, 20, 0.6);
  backdrop-filter: blur(20px) saturate(150%);
  border: 5px solid black;
  border-radius: 20px;
  z-index: 200;
  transition: all 0.3s ease;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
}

/* Collapsed State */
.music-player.collapsed {
  width: auto;
  min-width: 200px;
}

.collapsed-view {
  display: flex;
  flex-direction: column;
}

.collapsed-view .track-info {
  display: flex;
  align-items: center;
  gap: 8px;
  flex: 1;
  min-width: 0;
}

.collapsed-view .track-number {
  font-size: 11px;
  font-weight: 700;
  color: rgba(255, 255, 255, 0.5);
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
  padding: 2px 6px;
  flex-shrink: 0;
}

.collapsed-view .track-name {
  font-size: 11px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.9);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.mini-controls {
  display: flex;
  gap: 4px;
  flex-shrink: 0;
}

.expand-btn,
.collapse-btn {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.6);
  cursor: pointer;
  border-radius: 6px;
  padding: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  flex-shrink: 0;
  font-family: inherit;
}

.expand-btn:hover,
.collapse-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.9);
  transform: scale(1.05);
}

/* Expanded State */
.expanded-view {
  width: 280px;
  padding: 16px;
  position: relative;
}

.player-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.player-header h3 {
  font-size: 13px;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.9);
  margin: 0;
  letter-spacing: 0.5px;
  text-transform: uppercase;
}

.track-list {
  max-height: 320px;
  overflow-y: auto;
  margin-bottom: 12px;
  padding-right: 4px;
}

.track-list::-webkit-scrollbar {
  width: 4px;
}

.track-list::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 2px;
}

.track-list::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 2px;
}

.track-list::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 255, 255, 0.3);
}

.track-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  margin-bottom: 4px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.track-item:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(255, 255, 255, 0.15);
  transform: translateX(2px);
}

.track-item.active {
  background: linear-gradient(135deg, rgba(102, 126, 234, 0.2) 0%, rgba(118, 75, 162, 0.2) 100%);
  border-color: rgba(102, 126, 234, 0.3);
}

.track-item .track-number {
  font-size: 11px;
  font-weight: 700;
  color: rgba(255, 255, 255, 0.4);
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
  padding: 3px 7px;
  min-width: 24px;
  text-align: center;
}

.track-item.active .track-number {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.track-item .track-name {
  font-size: 12px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.8);
  flex: 1;
  line-height: 1.4;
}

.track-item.active .track-name {
  color: rgba(255, 255, 255, 1);
  font-weight: 600;
}

.playing-indicator {
  display: flex;
  gap: 2px;
  align-items: flex-end;
  height: 12px;
}

.playing-indicator span {
  width: 2px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 1px;
  animation: wave 0.6s ease-in-out infinite;
}

.playing-indicator span:nth-child(1) {
  animation-delay: 0s;
}

.playing-indicator span:nth-child(2) {
  animation-delay: 0.2s;
}

.playing-indicator span:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes wave {
  0%, 100% {
    height: 4px;
  }
  50% {
    height: 12px;
  }
}

/* Progress Bar Styles */
.progress-container {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 0;
  margin-bottom: 4px;
}

.time-display {
  font-size: 10px;
  color: rgba(255, 255, 255, 0.5);
  font-variant-numeric: tabular-nums;
  min-width: 32px;
}

.time-display:first-child {
  text-align: right;
}

.time-display:last-child {
  text-align: left;
}

.progress-bar {
  flex: 1;
  height: 6px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 3px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: height 0.15s ease;
}

.progress-bar:hover {
  height: 8px;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
  border-radius: 3px;
  transition: width 0.1s linear;
}

/* Mini progress bar for collapsed view */
.mini-progress {
  height: 3px;
  margin: 0 12px 8px 12px;
  border-radius: 2px;
}

.mini-progress:hover {
  height: 5px;
}

.mini-progress .progress-fill {
  border-radius: 2px;
}

.collapsed-top {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 12px 4px 12px;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 8px;
  padding-top: 12px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  height: 60px;
}

.control-btn {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.7);
  cursor: pointer;
  border-radius: 8px;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.control-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 1);
  transform: scale(1.05);
}

.control-btn.play-pause {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-color: transparent;
  color: white;
  padding: 12px;
}

.control-btn.play-pause:hover {
  transform: scale(1.1);
  box-shadow: 0 4px 16px rgba(102, 126, 234, 0.4);
}

.mini-controls .control-btn {
  padding: 6px;
  border-radius: 6px;
}

.mini-controls .control-btn.play-pause {
  padding: 7px;
}

/* Mobile Responsive Styles */
@media (max-width: 768px) {
  .music-player {
    right: 10px;
    top: 10px;
    max-width: calc(100vw - 20px); /* Prevent overflow */
  }

  .music-player.collapsed {
    min-width: auto;
    max-width: calc(100vw - 20px);
  }

  .collapsed-view {
    padding: 6px 10px;
    gap: 6px;
    max-width: 100%;
  }

  .collapsed-view .track-name {
    font-size: 10px;
    max-width: 100px; /* Prevent very long track names from overflowing */
  }

  .expanded-view {
    width: min(280px, calc(100vw - 40px)); /* Responsive width */
    max-width: calc(100vw - 40px);
  }
}
</style>
