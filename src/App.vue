<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <AppStage msg="Welcome to Your Vue.js App"/>
</template>

<script>
import AppStage from './components/AppStage.vue'

export default {
  name: 'App',
  components: {
    AppStage
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Courier New', monospace;
  background: linear-gradient(135deg, #0a0015 0%, #1a0033 50%, #0a0015 100%);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

#app {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

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
}

.file-input-label:hover {
  transform: translateY(-2px);
  box-shadow: 0 0 25px rgba(255, 0, 255, 0.8);
}

input[type="file"] {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
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
