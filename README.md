# Immersive Gaussian Splat Worlds (Fantasy, Basketball, Hand-Drawn)

## Overview
This project explores how **Gaussian splatting, 3D models, and spatial audio** can be combined to create immersive, interactive environments.

I developed three distinct scenes:
**Basketball Arena** – a realistic stadium environment with spatial Duke-themed audio  
**Fantasy World** – a stylized, futuristic environment with ambient sound  
**Hand-Drawn Scene** – an experimental, artistic environment exploring non-photorealistic splats  

Each scene allows users to navigate freely and experience **positional audio** that changes based on their position in the world.

---

## Core Concepts

This project combines three main technologies:

### 1. Gaussian Splatting (SparkJS)
- `.spz` files represent dense 3D environments  
- Rendered efficiently using **SparkRenderer**  
- No traditional lighting required  

### 2. 3D Models (GLB)
- Loaded using Three.js `GLTFLoader`  
- Used to add focal objects (e.g., basketball, spaceship)  
- Converted to `MeshBasicMaterial` for compatibility with splats  

### 3. Spatial Audio
- Built using `THREE.PositionalAudio`  
- Audio sources are placed at specific 3D coordinates  
- Volume and intensity change based on distance  

---

## Scenes

### Basketball Scene
- Cameron Indoor arena splat environment  
- Embedded basketball model (`.glb`)  
- Multiple audio emitters playing Duke chants  
- Demonstrates realistic spatial placement in a large-scale environment  

### Fantasy Scene
- Futuristic / stylized splat environment  
- Floating object (spaceship) loaded via GLB  
- Ambient environmental audio  
- Focus on atmosphere and immersion  

### Hand-Drawn Scene
- Experimental scene using stylized or non-photoreal splats  
- Explores how Gaussian splatting can be used creatively  
- Less about realism, more about artistic interpretation  

---

## Project Structure
/project-root

├── basketball.html
├── basketball_splat_audio.html
├── basketball.json

├── fantasy.html
├── fantasy_splat_audio.html
├── fantasy.json

├── handdrawn.html (or equivalent)

├── /media
│ ├── *.spz # Gaussian splat environments
│ ├── *.glb # 3D models

├── /audio
│ ├── *.mp3 # spatial audio files

├── spark.module.min.js
└── index.html


---

## How It Works

Each scene is defined by a JSON file:

```json
{
  "splats": [...],
  "meshes": [...]
}

Rendering Pipeline
Load scene JSON
Render splats using SplatMesh
Load GLB models using GLTFLoader
Add positional audio emitters
Render with Three.js
Setup Instructions
Option 1: Python (Recommended)
python3 -m http.server 8000

Open in browser:

http://localhost:8000
Option 2: Node.js
npx serve .
Usage

Open any of the following in your browser:

basketball.html
basketball_splat_audio.html
fantasy.html
fantasy_splat_audio.html
handdrawn.html
Controls
Click + drag → Look around
Trackpad / WASD → Move through the scene
Play Audio button → Toggle sound
Hide Audio Mesh button → Show/hide emitter spheres
Key Technical Challenges
1. Meshes Not Appearing

GLB models were initially missing in audio scenes because the meshes array from the JSON was not loaded.

Fix:

import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";

const gltfLoader = new GLTFLoader();
for (const mesh of sceneData.meshes ?? []) {
  const gltf = await gltfLoader.loadAsync(mesh.filename);
  scene.add(gltf.scene);
}
2. File Path Issues

Some assets failed to load due to duplicated paths like:

././media/file.glb

Fix:

mesh.filename
3. Audio Not Playing

Browsers block autoplay audio.

Fix:

await listener.context.resume();
4. Objects Appearing Missing

Objects were sometimes:

Too small
Too far away

Debug strategy:

model.scale.set(1, 1, 1);
model.position.set(0, 1, 0);
Why This Project Matters

This project explores a key idea:

How can we move beyond traditional 3D modeling into more immersive, experiential environments?

Gaussian splatting enables:

Fast rendering of complex scenes
Photorealistic and stylized environments
New ways to combine visuals, interaction, and sound

By adding spatial audio, the experience becomes something users don’t just see — but feel and navigate.

Future Improvements
More dynamic audio zones
VR / XR support
Scene switching UI
More stylized splat experiments
Credits
Three.js
SparkJS (Gaussian splatting)
Duke University coursework / project inspiration
