# One Wax Wing - Let Slip Visualizer

An immersive, interactive audio visualizer experience for **"Let Slip"**, the debut album by **One Wax Wing**.

**[Experience it live at onewaxwing.com](http://onewaxwing.com/)**

---

## About

This is not just a music player. It's a visual journey through sound.

Every beat, every frequency, every sonic texture is translated into mesmerizing visuals that dance, pulse, and breathe with the music. Built with Vue.js and the Web Audio API, this visualizer transforms listening into a fully immersive audiovisual experience.

## The Album: Let Slip

**Let Slip** features 7 tracks of genre-defying electronic music:

1. Crate Diggers Local 227
2. Mantra Loupe
3. As Sane As Any Of Us
4. Virtuous Vitreous
5. I Stole A Glance At My Brother's Sketchbook
6. Dr. Remember's Miracle Elixir
7. Down And Out In Sidereal Time

Available on [Spotify](https://open.spotify.com/album/1NsKmobn8B3cgLTzTafNTZ) | [Apple Music](https://music.apple.com/us/album/let-slip/1867399784) | [YouTube Music](https://music.youtube.com/playlist?list=OLAK5uy_kQB-R7LydjI31Gmf-8HSR2iDbT9Kr7UTM) | [Bandcamp](https://onewaxwing.bandcamp.com/album/let-slip)

## Features

### 20 Unique Visualizations

- **Swarm** - Particle systems that flock and dance
- **Spkrwall** - Pulsing speaker cone aesthetic
- **Aurora Waves** - Flowing horizontal color bands
- **Spiral Galaxy** - Cosmic spinning formations
- **Helix** - DNA-inspired ribbon structures
- **Hex Meadow** - Hexagonal flower patterns
- **Wavepools** - Concentric rippling waves
- **Undertide** - Flowing river currents
- **Dot Matrix** - Retro LED-style grid
- **Osmosis** - Liquid crystal movements
- **Petri** - Organic culture-like growth
- **Boombox** - Classic audio equipment tribute
- **Moiré** - Hypnotic interference patterns
- **Toroid** - 3D sphere transformations
- **Kaleido** - Kaleidoscopic reflections
- **Synaptic** - Neural network connections
- **Cylindric** - 3D cylinder formations
- **Pipes** - Retro screensaver inspired pipes
- **From Orbit** - City lights seen from space with solar corona effects
- **Triangulator** - Sierpinski fractal triangles
- **Tendrils** - Flowing color ribbons inspired by classic WMP visualizers

### Dynamic Color Palettes

10 stunning color palettes that auto-cycle or can be manually selected:

Neon • Cosmic • Sunset • Ocean • Fire • Arctic • Gold • Acid Trip • Psychedelic • Rainblow

### Interactive Elements

- **Mouse/Touch Drag** - Draw colorful trails with particles and ripples
- **Mouse Movement** - Warp and influence visualizations
- **Swipe Gestures** - Change palettes on mobile
- **Keyboard Controls** - Full keyboard navigation

### Controls

| Key | Action |
|-----|--------|
| `←` `→` | Previous/Next visualization |
| `↑` `↓` | Cycle color palettes |
| `Space` | Play/Pause |
| `N` | Next track |
| `P` | Previous track |

## Tech Stack

- **Vue.js 3** - Reactive UI framework
- **Web Audio API** - Real-time audio analysis
- **Canvas 2D** - Hardware-accelerated rendering
- **CSS3** - Smooth animations and transitions

## Development

### Prerequisites

- Node.js (v14+)
- npm or yarn

### Setup

```bash
# Install dependencies
npm install

# Start development server
npm run serve

# Build for production
npm run build
```

### Project Structure

```
src/
├── components/
│   ├── AppStage.vue      # Main visualizer (patterns, audio, rendering)
│   └── MusicPlayer.vue   # Track controls and progress
├── App.vue
└── main.js
```

## Performance

The visualizer is optimized for smooth 60fps performance:

- Efficient particle systems with object pooling
- Minimal DOM manipulation
- RequestAnimationFrame-based render loop
- Adaptive complexity based on audio intensity

## Browser Support

Works best in modern browsers with Web Audio API support:

- Chrome/Edge (recommended)
- Firefox
- Safari

---

**One Wax Wing** - *Let Slip*

**[onewaxwing.com](http://onewaxwing.com/)**
