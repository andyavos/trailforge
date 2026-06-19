# TrailForge — GPX to 3D Print

A single-file web app that turns a GPX track from Strava, Garmin, Komoot, or any GPS device into a 3D-printable STL file and an interactive 3D preview. Everything runs in the browser — no server, no upload, no account.

<img width="2006" height="3094" alt="gpx-to-3d-print" src="https://github.com/user-attachments/assets/854e88a8-f7ac-4ca2-b6c9-74037548dcfe" />

## Getting Started

Open `gpx-to-3d-print.html` in any modern browser (Chrome, Firefox, Edge, Safari). No installation required.

1. Drag and drop a `.gpx` file onto the upload zone, or click **Choose File**.
2. The app shows your route stats, a 2D map, and an elevation profile immediately.
3. Adjust the print settings if needed.
4. Click **Generate STL for 3D Printing** to download the `.stl` file.

## Features

### 2D Preview tab
- **Route map** — your track drawn over a dark background, coloured by elevation (blue = low, green = mid, orange/red = high). Start and end are marked with green and red dots.
- **Elevation profile** — filled area chart with distance ticks along the bottom, elevation labels on the left, and the peak annotated.
- **Stats bar** — total distance (km), elevation gain (m), maximum altitude (m asl), and raw track point count.

### 3D Model tab
An interactive Three.js viewer of the exact geometry that will be printed.

| Control | Action |
|---|---|
| Left-click drag | Orbit / rotate |
| Right-click drag | Pan |
| Scroll wheel | Zoom |
| Pinch (touch) | Zoom |
| One-finger drag (touch) | Orbit |

**Preset views:** Top · Front · Isometric · Along Route  
**Style toggle:** Solid (elevation-coloured) or Wireframe

The model rebuilds automatically when you change any print setting while the 3D tab is open.

### STL Export
Generates a valid ASCII STL ready for any slicer (Cura, PrusaSlicer, Bambu Studio, OrcaSlicer, etc.).

The model consists of:
- A flat rectangular base plate
- Your route extruded as a raised ridge, with height proportional to real elevation (plus optional exaggeration)
- Aspect ratio preserved using the cosine correction for your track's latitude, so the shape is geographically accurate

### Save Preview Image
Exports a composite PNG of the route map and elevation profile with your track name and stats, suitable for sharing.

## Print Settings

| Setting | Default | Description |
|---|---|---|
| Base width | 100 mm | X dimension of the base plate |
| Base depth | 100 mm | Y dimension of the base plate |
| Base thickness | 3 mm | Height of the flat base |
| Route ridge height | 8 mm | Maximum height of the route above the base (before exaggeration) |
| Route ridge width | 1.5 mm | Width of the extruded route ribbon |
| Elevation exaggeration | 2× | Multiplier applied to the elevation relief — increase for flat routes |

### Recommended starting points

**Palm-sized keepsake (100 × 100 mm)**  
Base 3 mm · Ridge height 8 mm · Ridge width 1.5 mm · Exaggeration 2×

**Desk plaque (150 × 150 mm)**  
Base 4 mm · Ridge height 12 mm · Ridge width 2 mm · Exaggeration 1.5×

**Wall art (200 × 200 mm)**  
Base 3 mm · Ridge height 10 mm · Ridge width 2.5 mm · Exaggeration 1×

If your slicer flags thin walls, increase ridge width to 2 mm or more. For very flat routes (< 200 m total elevation change), set exaggeration to 3–5× so the relief is visible.

## Supported GPX Sources

Tested with exports from Strava, Garmin Connect, Komoot, OSMAnd, AllTrails, and Wahoo. The parser reads `<trkpt>`, `<rtept>`, and `<wpt>` elements, so both track and route GPX files work.

## Privacy

No data leaves your machine. The GPX file is parsed entirely in the browser with no network requests (other than loading Google Fonts and Three.js from CDN on first open).

## Technical Notes

- Three.js r128 is loaded from cdnjs for the 3D viewer.
- The STL exporter downsamples tracks longer than 2,000 points to keep file size reasonable while preserving the route shape.
- The 3D viewer downsamples to 1,500 points for smooth frame rates.
- STL normals are computed per-triangle via cross product, which is compatible with all major slicers.
