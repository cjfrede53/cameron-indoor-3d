# Cameron Indoor: After Hours 🏀

**[Take the tour →](https://cjfrede53.github.io/cameron-indoor-3d/)**

No ticket. No crowd. No lines. A private, after-hours walk through an empty Cameron Indoor Stadium — a photorealistic 3D Gaussian splat capture with the roar of game night still hanging in the air.

Visitors enter through a Duke Athletics–styled landing page, then follow a guided tour through the arena — courtside, center court, the student section, the team bench, the tunnel — with narrated stops and spatial crowd audio that swells as they approach the stands. Everything renders live in the browser: no plugins, no install, no backend.

---

## The Concept

Gaussian splatting is remarkable at capturing spaces and terrible at capturing people — crowds render as smears. Rather than fight that constraint, this project reframes it: the capture is of Cameron **empty**, a view of the building almost nobody gets, while the spatial audio (recorded at real games) haunts the arena with the sound of a full house. The limitation became the concept.

---

## Features

- 🏟️ Photorealistic Gaussian splat capture of Cameron Indoor Stadium, navigable in first person
- 🎧 Layered spatial audio: a global ambient crowd bed plus positional emitters placed in the stands — chants swell as you approach the student section
- 🗺️ Guided tour with narrated stops, smooth camera glides between them, and free-roam mode with a "Back to Tour" resume
- 🎨 Duke Athletics–styled landing page: blue-duotone Cameron Crazies photo, varsity typography, broadcast-style entrance animation
- 📇 Perpetual "ribbon board" credit marquee, styled after arena LED boards
- ⚡ Runs entirely in the browser — static site, no server, deployed on GitHub Pages

---

## Tech Stack

- **Three.js** — rendering, camera, GLB model loading, Web Audio integration
- **SparkJS** (`SparkRenderer`, `SplatMesh`, `SparkControls`) — Gaussian splat rendering from `.spz`
- **THREE.PositionalAudio + THREE.Audio** — spatial emitters layered over a listener-attached ambient bed
- **Vanilla HTML/CSS/JS** — no framework, no build step
- **GitHub Actions + GitHub Pages** — automatic deploy on every push
- **ffmpeg** — crowd audio assembled from my own game recordings: clips crossfaded into one continuous track, with the loop point sealed so the restart is inaudible

---

## How It Works

1. The scene is defined in `basketball.json`: splat transforms and GLB mesh placements.
2. `SplatMesh` streams the ~8 MB `.spz` arena capture; a basketball GLB is placed on the court with materials converted to `MeshBasicMaterial` for compatibility with splats.
3. While the splat streams in behind the landing page, the Enter button reads "Loading the arena…" — by the time visitors finish reading the controls, it flips to "Enter the Arena."
4. The Enter click doubles as the browser's user-gesture requirement, unlocking the Web Audio context so the crowd starts immediately — no autoplay block.
5. The guided tour glides the camera between authored stops (position-only lerp, so visitors keep full look control) while a caption card narrates each location.

---

## Design Decisions

**Frame the constraint as the concept.** Splats can't render people, so the experience is marketed as exactly what it is: an exclusive after-hours tour of an empty arena, with the crowd present only as sound.

**Sound design like a game.** One positional emitter per crowd hotspot creates dead zones; a single global track has no space. The solution is layered: a low-volume ambient bed attached to the listener guarantees the crowd is always audible, while positional emitters in the stands add direction and swell. All sources play in sync — real crowds chant in unison, and desynced sources read as dueling radios, not atmosphere.

**The tour is the "so what."** A splat you can walk around is a demo; a narrated tour with insider captions is an experience. The tour also directs attention toward the areas the splat renders beautifully and away from signage, which splats render poorly. Stops were authored in-scene: clicking logs the camera position to the console, which gets pasted into the tour config.

**Athletics, not glassmorphism.** Flat royal blue, Oswald block type with athletic tracking, sharp corners, a duotone crowd photo (luminosity blend), and a scrolling ribbon-board credit line — the visual language of a team site, not a tech demo.

---

## Key Technical Challenges

**Making a 2-minute audio loop feel endless.** Short chant loops are maddening. The track is assembled from four of my own game recordings, crossfaded into one ~2:40 sequence — and the loop point is sealed by fading the track's tail out underneath its fading-in head, so the restart can't be heard. All three audio sources share one downloaded buffer.

**Positional audio dead zones.** Away from the emitters, the arena went silent — the falloff that creates the spatial effect also creates emptiness. Fixed with the ambient bed layer (attached to the listener, immune to distance) under the positional emitters.

**Deploying splats to GitHub Pages.** The `.spz` capture, models, and audio had been excluded from version control; Pages can only serve what's committed. The assets (~25 MB total, well under GitHub's limits) are now committed, and a GitHub Actions workflow uploads the repo as-is on every push — no build step needed for a vanilla static site.

**Camera glides that don't fight the controls.** The tour animates only the camera's *position* (a per-frame lerp toward the stop), never its rotation — visitors keep full look control during transit, so the glide feels like being walked somewhere rather than a cutscene.

---

## Running Locally

No installation needed to try it — the tour is live at [cjfrede53.github.io/cameron-indoor-3d](https://cjfrede53.github.io/cameron-indoor-3d/). To run it yourself:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`. (A local server is required — opening `index.html` directly blocks the scene's `fetch` calls.)

## Controls

- **Click + drag** — look around
- **Arrow keys / two-finger drag** — move around the court
- **Two-finger click + drag** — rise and descend
- **Back to Tour** — resume the guided tour from wherever you left off

---

## Deployment

Deploys automatically to GitHub Pages via GitHub Actions (`.github/workflows/deploy.yml`) on every push to `main`.

---

## Future Improvements

- More tour stops and audio hotspots (championship banners, Krzyzewskiville)
- VR / XR support
- Additional scenes (a stylized fantasy environment is already captured)
- Higher-fidelity splat recapture

---

## Credits

- [Three.js](https://threejs.org/) — 3D rendering
- [SparkJS](https://sparkjs.dev/) — Gaussian splat rendering
- Landing page photo: ["Cameron Crazies" by Adam Glanzman](https://commons.wikimedia.org/wiki/File:20131203_Cameron_Crazies.jpg), via Wikimedia Commons, [CC BY 2.0](https://creativecommons.org/licenses/by/2.0)
- Crowd audio recorded by me at Duke games
- Built for Duke University coursework, then extended into this deployed experience
