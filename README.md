# Enhanced Snub Disphenoid (Johnson Solid J84) Visualizer

## Overview

This project is an interactive 3D visualizer for the Snub Disphenoid, also known as Johnson solid J84. The snub disphenoid is a convex polyhedron with 12 equilateral triangles as its faces, 18 edges, and 8 vertices. This enhanced tool calculates its geometric properties based on established formulas and renders it using Three.js, allowing for comprehensive interactive exploration of the polyhedron's internal structure through multiple separation modes.

All detailed calculations, vertex coordinates, edge lists, and geometric property verifications are output to the browser's developer console upon loading.

## Features

### **Accurate Geometric Calculation**
* Calculates the unique positive root `q` of the polynomial `2x³ + 11x² + 4x - 1 = 0`
* Derives parameters `r`, `s`, and `t` from `q`
* Computes the 8 vertex coordinates for a unit edge length snub disphenoid
* Identifies the 18 edges connecting these vertices

### **Interactive 3D Visualization**
* Rendered using Three.js with custom mouse controls
* **Spinning Animation:** The model automatically rotates
  * **Pause/Resume Control:** Button to pause or resume the animation
  * **Speed Control:** Slider to adjust the rotation speed
* **Custom Mouse Controls:**
  * **Rotate:** Left-click and drag to orbit around the model
  * **Pan:** Right-click and drag to shift the view
  * **Zoom:** Mouse scroll wheel to zoom in/out

### **Flexible Separation System**
The visualizer now features an independent control system allowing any combination of separations:

#### **Core Separation Controls**
* **"Separate Cores" / "Join Cores"** button
* Splits the snub disphenoid into its two main core pieces.
* The separation occurs along the primary axis of symmetry, defined by the vector from the midpoint of edge (V6,V7) to the midpoint of edge (V4,V5). Piece 1 Core (associated with V6,V7 side) moves in the negative direction of this axis, and Piece 2 Core (associated with V4,V5 side) moves in the positive direction.
* Animation distance: 1.05 units for cores.

#### **Cap Separation Controls**
* **"Separate Caps" / "Join Caps"** button
* Separates the two caps from their respective core shells:
  * **Piece 1 Cap:** Contains triangles (0,7,6) and (1,6,7) sharing edge (V6,V7).
  * **Piece 2 Cap:** Contains triangles (2,5,4) and (3,4,5) sharing edge (V4,V5).
* The translation of the caps during separation also occurs along the primary axis of symmetry (the same `coreAxisA` used for core separation). Piece 1 Cap moves in the negative direction of this axis, and Piece 2 Cap moves in the positive direction, relative to their initial positions on the unified model or their attached core piece.
* Animation distance: 1.2 units for caps.
* Works independently whether cores are joined or separated. If cores are separated, caps will follow their respective core's translation while also applying their own separation translation.

#### **All Possible Configurations**
1. **Unified Model** (both cores and caps joined)
2. **2-Piece Mode** (cores separated, caps joined to their respective cores)
3. **3-Piece Mode** (cores joined, caps separated from the unified core body)
4. **4-Piece Mode** (both cores and caps separated, with caps maintaining their offset from their respective, now separated, cores)

### **Display Modes**
* **Wireframe View:** Shows the edges of the polyhedron (green)
* **Solid View:** Shows the polyhedron as a solid object with distinct faces
* **Face Coloring (Solid Mode Only):**
  * **Multi-Color Mode:** Assigns distinct colors to faces using a greedy algorithm
  * **Monochrome Mode:** Single color for the unified model, with distinct default colors for separated pieces (Piece 1 Core: Blue-ish, Piece 2 Core: Orange, Piece 1 Cap: Green, Piece 2 Cap: Red-ish) when not in multi-color mode.
* **Sharp Edge Delineation:** Contrasting line overlay for clear face separation in solid mode.

### **Vertex Labels**
* Numerical labels (0-7) displayed at each vertex using HTML overlay system
* **Smart Following:** Labels track vertices through all separation states and model rotations.
* **Occlusion Detection:** Labels automatically hide when obscured by solid geometry in solid view.
* **Duplicate Labels:** For seam vertices (V0, V1, V2, V3), duplicate labels appear on the opposing piece when cores are separated, ensuring each part of the seam is clearly identified.

### **Smooth Animations**
* All transitions between separation states are smoothly animated using linear interpolation (lerp).
* No jarring jumps or discontinuities.
* Animation speeds are defined for clarity.

## How to Use

1. **Open the HTML File:** Save the provided code as an HTML file and open it in a modern web browser that supports WebGL.
2. **Automatic Load:** The visualization and calculations begin automatically when the page loads.
3. **Check Console for Data:** Open your browser's developer console (F12) to view calculated parameters (`q`, `r`, `s`, `t`), vertex coordinates, and other geometric details.
4. **Interact with Controls:**
   * **Pause/Resume:** Control the spinning animation.
   * **Speed Slider:** Adjust rotation speed.
   * **Show Solid / Show Wireframe:** Switch between rendering modes.
   * **Hide Labels / Show Labels:** Toggle vertex label visibility.
   * **Color Faces / Monochrome:** Switch face coloring (only available in solid mode).
   * **Separate Cores / Join Cores:** Control main core piece separation.
   * **Separate Caps / Join Caps:** Control cap separation.
5. **Navigate the 3D View:**
   * **Rotate:** Left-click and drag.
   * **Pan:** Right-click and drag.
   * **Zoom:** Mouse scroll wheel.

## Technical Details

### **Mathematical Foundation**
* Vertex coordinate calculations based on formulas for the Snub Disphenoid, ensuring unit edge length. The process involves finding the root `q` of `2x³ + 11x² + 4x - 1 = 0`, then deriving `r = sqrt(q)/2`, `s = sqrt((1-q)/(8q))`, and `t = sqrt((1-q)/2)`.
* Source for formulas often referenced in polyhedron geometry resources (e.g., Wikipedia, Wolfram MathWorld, polytope.miraheze.org).

### **Technologies Used**
* **HTML5** for structure.
* **CSS3** (including Tailwind CSS via CDN) for styling.
* **JavaScript (ES6+)** for calculations, logic, and DOM manipulation.
* **Three.js (r128 via CDN)** for WebGL-based 3D rendering.
* **Custom HTML overlay system** for dynamic vertex labels.

### **Architecture**
* **Independent State Management:** Core and cap separations are controlled by separate boolean state variables and lerp factors for animation.
* **Smooth Animation System:** Uses `requestAnimationFrame` and linear interpolation for all piece movements.
* **Smart Visibility Control:** Logic in `updateShapeAndColorControls` function dynamically shows/hides the unified model or individual pieces (cores, caps) based on the current separation state and whether an animation is in progress. This prevents visual artifacts like lingering faces.
* **Label Tracking System:** Vertex labels (HTML divs) are repositioned in each animation frame based on the transformed 3D coordinates of their associated vertices, including occlusion culling.

## Geometric Structure

### **Vertex Assignments and Piece Association**
* **V0, V1:** Seam vertices. Primarily associated with Piece 2 Core. Duplicate labels appear on Piece 1 Core during separation.
* **V2, V3:** Seam vertices. Primarily associated with Piece 1 Core. Duplicate labels appear on Piece 2 Core during separation.
* **V4, V5:** Vertices unique to Piece 2 Cap.
* **V6, V7:** Vertices unique to Piece 1 Cap.

*(Note: Piece 1 refers to the core/cap on the -coreAxisA side, Piece 2 to the +coreAxisA side)*

### **Face Groupings (Indices from `J84_FACES_INDICES`)**
* **Piece 1 Core Faces:** `[2, 4, 6, 8]`
* **Piece 1 Cap Faces:** `[3, 7]` (these are the two triangles sharing edge V6-V7)
* **Piece 2 Core Faces:** `[0, 1, 5, 9]`
* **Piece 2 Cap Faces:** `[10, 11]` (these are the two triangles sharing edge V4-V5)

### **Separation Definitions**
* **Core Separation Axis (`coreAxisA`):** Defined as the normalized vector from the midpoint of edge (V6,V7) to the midpoint of edge (V4,V5).
  * Piece 1 Core translates along `-coreAxisA`.
  * Piece 2 Core translates along `+coreAxisA`.
* **Cap Separation:**
  * The *boundary* of Piece 1 Cap is defined by faces involving edges (V0,V6), (V6,V1), (V1,V7), (V7,V0).
  * The *boundary* of Piece 2 Cap is defined by faces involving edges (V2,V4), (V4,V3), (V3,V5), (V5,V2).
  * During separation, Piece 1 Cap translates along `-coreAxisA` relative to its base position (either on the unified model or its associated core).
  * During separation, Piece 2 Cap translates along `+coreAxisA` relative to its base position.

## Browser Compatibility

Requires a modern web browser with WebGL support:
* Chrome 60+
* Firefox 55+
* Safari 12+
* Edge 79+

## Files

* Single self-contained HTML file with embedded CSS and JavaScript.
* No external dependencies except Three.js and Tailwind CSS, both loaded via CDN.

---

This enhanced visualizer provides insight into the geometric structure of the snub disphenoid, allowing users to explore how this fascinating polyhedron can be decomposed and reassembled in multiple configurations.
