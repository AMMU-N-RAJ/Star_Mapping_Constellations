# Star Mapping 
## Truncated Icosahedron

![](img1.png)

This readme needs work but it should at least be informative.

## About

This application is a tool to generate a 3D preview and 2D laser-cutting
template of custom selections of stars and constellations projected onto a
polyhedron of your choice. Stars data comes from actual astrometric
catalogues and includes accurate positions and enables filtering based on
apparent brightness.

Projecting onto a polyhedron means we can easily "un-fold" the projected results
into a 2D template for cutting and then re-fold that into a physical 3D shape.

### Available Geometries

The following shapes are available for projection:

- **Tetrahedron** — 4 triangular faces
- **Cube** — 6 square faces
- **Octahedron** — 8 triangular faces
- **Dodecahedron** — 12 pentagonal faces
- **Icosahedron** — 20 triangular faces
- **Truncated Icosahedron** — 32 faces (12 pentagons + 20 hexagons, the classic soccer ball shape)

The first five are Platonic solids (all faces identical). The truncated
icosahedron is an Archimedean solid with two types of faces, providing a
rounder shape with more surface area for star projections.

The final result is allowing a light source (ideally
very small, bright, and omni-directional) to project individual points of light
onto the walls and ceiling of a room.


## Usage

### Customization

The tool will start up with default selections for the target shape, minimum
brightness level, and constellations. Any of these can be modified to and the
results will be re-projected in the preview pane automatically.

**Note:** the full catalogue includes over 98k stars. Not only can it take quite
a while to project the entire selection, but most of them are very faint stars
(meaning you likely wouldn't recognize them) and they would be bunched very
close together. The latter can be especially problematic due to overlapping
cuts: without manual (tedious!) cleanup you could end up with a ring of
connected stars causing a section of the projector to be unintentionally cut
out. Think of the island in a stencil of the letter "O".

The list of constellations is pre-filtered to only count those that are
"interesting". The full list maps out 88 constellations and you might be
surprised how many of them look like a simple line. My filter requires that a
constellation includes at least one star that connects to at least 3 other
stars.

You can visualize what it's like to unfold the 3D shape by clicking on it. It
looks neat but doesn't actually serve much of a purpose.

## How It Works

The Star Mapping application follows this workflow:

1. **Data Processing**: Real astrometric catalogue data (HD catalogue with over 98,000 stars) is loaded and filtered based on:
   - Apparent brightness magnitude (user-selectable)
   - Constellation membership (pre-filtered for "interesting" constellations with complex patterns)

2. **Projection**: Selected stars and constellation lines are projected from the celestial sphere onto the surface of a chosen 3D polyhedron using Three.js geometric transformations.

3. **Topology Analysis**: The application analyzes the topology and geometry of each face of the polyhedron to determine:
   - Edge connections
   - Polygon boundaries
   - Vertex positions and transformations

4. **2D Unfolding**: The 3D polyhedron is computationally "unfolded" into a 2D net (flat template) with:
   - Cuts shown as solid lines
   - Fold lines shown as dashed lines
   - Stars positioned accurately for laser cutting

5. **Visualization & Export**:
   - A real-time 3D preview is displayed showing the projected stars on the chosen geometry
   - The 2D template is generated as an SVG file suitable for laser cutting and assembly
   - A light source placed at the center projects the cut-out stars onto surrounding surfaces when assembled

## Development

### Building the Project

The project uses Webpack for bundling and development:

```bash
# Start development server on port 8080
npm start

# Build for production
npm run build

# Clean build artifacts
npm run clean
```

### Architecture

- **Frontend**: Vue.js 2.x application with reactive UI components
- **3D Graphics**: Three.js for rendering and geometric transformations
- **State Management**: Vue.js reactive data with async-computed properties
- **Module Bundling**: Webpack with Babel transpilation for ES2015+ support
- **Data Processing**: Python script (`filter.py`) for pre-processing star catalogue data
- **SVG Generation**: Custom SVG rendering pipeline for laser-cutting templates

### Key Modules

- `app.js` — Main Vue application entry point
- `project.js` — Core projection and building logic
- `catalogs.js` — Star catalogue data management and queries
- `topology/` — Geometric topology analysis
- `geometry/` — Polyhedron geometry definitions and transformations
- `projections/` — Star-to-polyhedron projection algorithms
- `template/` — SVG generation and 2D net unfolding
- `extensions/` — Three.js extensions for curves, bezier paths, and custom shapes

## Tools Used

### Frontend Technologies

- **Vue.js 2.1.6** — Progressive JavaScript framework for reactive UI
- **Three.js 0.88.0** — JavaScript 3D graphics library for rendering and geometric calculations
- **Lodash 4.17.4** — Utility library for common programming tasks
- **vue-async-computed 3.1.2** — Vue plugin for async computed properties

### Build & Compilation

- **Webpack 2.5.0** — Module bundler and build tool
- **Babel 6.x** — JavaScript transpiler for ES2015+ syntax support
- **Babel Plugins** — Transform object rest-spread, ES2015 preset support
- **ESLint 3.7.1** — JavaScript linter with React plugin support
- **webpack-dev-server 2.4.5** — Development server with hot module reloading

### Asset Processing

- **html-webpack-plugin 2.28.0** — HTML template generation
- **extract-text-webpack-plugin 2.1.2** — CSS extraction from bundles
- **file-loader 1.1.5** — Asset file bundling
- **html-loader 0.4.5** — HTML content loading
- **style-loader 0.19.0** — CSS injection into DOM
- **css-loader 0.28.7** — CSS module loading
- **worker-loader 0.8.0** — Web Worker bundling

### Core Libraries

- **bezier-js 2.2.3** — Bézier curve calculations and manipulation
- **convexhull-js 1.0.0** — Convex hull computation for polygon analysis
- **parse-svg-path 0.1.2** — SVG path parsing
- **sift 3.2.6** — MongoDB query language implementation for filtering
- **eventemitter3 2.0.3** — Event emitter for pub/sub patterns
- **debounce 1.0.0** — Function debouncing for performance
- **async 2.4.0** — Asynchronous utility functions
- **threestyle 0.2.1** — Three.js styling utilities

### Data Processing

- **Python** — Server-side star catalogue filtering and data pre-processing
- **JSON** — Structured data format for star catalogues and asterism definitions

### Development Utilities

- **whatwg-fetch 2.0.1** — Polyfill for modern Fetch API
- **babel-polyfill 6.23.0** — JavaScript standard library polyfills
