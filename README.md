# glb2sprite

Turn a 3D model (`.glb` or `.gltf`) into a game-ready sprite sheet, right in your browser. Nothing is uploaded: the model never leaves your computer.

**Live:** https://jerryfemi.github.io/glb2sprite/

## What it does

1. Drop in a `.glb`, or a `.gltf` with its `.bin` and texture files (dropping the whole unzipped folder works).
2. Orbit the camera to the angle you want. The dashed square is exactly what gets rendered.
3. Render. You get **one sprite sheet** (`name_atlas.png`) and a **JSON map** (`name_atlas.json`) of where each frame sits, saved together as a `.zip`.

Each sheet can contain:

- **A resting frame** from your camera view.
- **Spin frames**, a looping roll across 90°, 120° or 360°. Pick the smallest angle that repeats (90° for four fins or flights) and the sheet stays small.
- **Tilted frames**, leaning left, straight and right, like a dart or arrow stuck in a target.
- **Colour versions**: recolour the widest parts (flights, fletching) and get one row per colour.

- **Animation frames**: if the model has animation clips (a bow drawing, a flag waving, a character idle), pick a clip, scrub to any pose, and sample the clip into evenly spaced frames.

Every frame is cropped to the **same box**, so animations don't jitter.

Compressed models (Draco, Meshopt), common on Sketchfab and in asset packs, load too.

## Angle sweep (for 3D games)

Tick **Angle sweep (3D flight)** under Frames to draw an object in true perspective in a 2D engine, for example a dart flying away from the camera. The sheet gets one **row per viewing angle**, from almost tail-on (4°) to a full side view (90°), and one **column per spin step**. The tip points up in every frame, so the game only has to:

1. Work out the angle between the camera and the object's direction of travel, and pick the nearest row.
2. Rotate the frame to the object's on-screen direction.
3. Scale it for distance.

**Preview angles** shows one frame per row before you render. The sheet saves as `name_sweep.png` and `name_sweep.json`. The JSON adds `angles`, plus `angleRows`, which gives the tip, tail and centre pixel in each row's cell, and `fullLengthPx`, the object's length at a 90° view. Frame keys are `p{colour}_a{angle}_s{spin}`.

## Who it's for

- **2D and web game developers** (Flutter/Flame, Phaser, PixiJS, Godot, Unity 2D) who want a 3D look without running 3D in the game.
- **Game-jam and solo devs** who need sprites from an asset pack fast, without setting up Blender.
- **Motion and UI designers** who need product turntables or animated icons as frame strips.

## The JSON

```json
{
  "image": "arrow_atlas.png",
  "cellWidth": 308, "cellHeight": 246,
  "columns": 12, "rows": 1,
  "spinFrames": 8, "spinDegrees": 90, "stuckFrames": 3,
  "frames": {
    "p1_rest":    { "x": 0,   "y": 0, "w": 308, "h": 246 },
    "p1_spin_0":  { "x": 308, "y": 0, "w": 308, "h": 246 }
  }
}
```

Rows are colour versions (`p1`, `p2`). Columns are `rest`, then `anim_0…`, then `spin_0…`, then `stuck_0…`. Every cell is the same size, so you can also slice the sheet as a plain grid.

## Controls

- **View buttons**: Front, Back, Left, Right, Top and ¾ snap the camera there.
- **Arrows** (buttons or keyboard) turn the view 5° at a time; hold Shift for 1°. `+` and `-` zoom.
- **Double-click** any part to orbit around it.
- **Parts**: sky domes, floors and backdrops that come with Sketchfab scenes are hidden automatically. Tick or untick any part, then press **Frame object**.

## Tips

- Turn **Auto-orient** off for bows, trees, characters and props. It is meant for long objects like darts and arrows.
- If a model is CC-BY, the page shows the author it found in the file. Credit them in your game.

## Run it locally

It's one HTML file with no build step. Open `index.html` through any local server, for example:

```bash
npx serve .
```

Opening the file directly (`file://`) can block the 3D libraries, so use a server.

## Built with

[three.js](https://threejs.org) r160 (MIT) and [JSZip](https://stuk.github.io/jszip/) (MIT), loaded from CDNs.

## License

MIT. See [LICENSE](LICENSE).
