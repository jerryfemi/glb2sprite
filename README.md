# Sprite Renderer

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

Every frame is cropped to the **same box**, so animations don't jitter.

Compressed models (Draco, Meshopt), common on Sketchfab and in asset packs, load too.

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

Rows are colour versions (`p1`, `p2`). Columns are `rest`, then `spin_0…`, then `stuck_0…`. Every cell is the same size, so you can also slice the sheet as a plain grid.

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
