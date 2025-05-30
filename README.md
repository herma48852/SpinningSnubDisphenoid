# Snub Disphenoid (Johnson Solid J84) Visualizer

## Overview

This project is an interactive 3D visualizer for the Snub Disphenoid, also known as Johnson solid J84. The snub disphenoid is a convex polyhedron with 12 equilateral triangles as its faces, 18 edges, and 8 vertices. This tool calculates its geometric properties based on established formulas and renders it using Three.js, allowing for interactive exploration.

All detailed calculations, vertex coordinates, edge lists, and geometric property verifications (such as plane perpendicularity and symmetry) are output to the browser's developer console upon loading.

## Features

* **Accurate Geometric Calculation:**
    * Calculates the unique positive root `q` of the polynomial `2x³ + 11x² + 4x - 1 = 0`.
    * Derives parameters `r`, `s`, and `t` from `q`.
    * Computes the 8 vertex coordinates for a unit edge length snub disphenoid.
    * Identifies the 18 edges connecting these vertices.
* **Geometric Property Verification (Console Output):**
    * Checks for perpendicularity between specified edges.
    * Verifies coplanarity of specified sets of vertices.
    * Checks for perpendicularity between defined planes (e.g., plane of V1,V5,V4,V0 and plane of V3,V7,V6,V2).
    * Investigates planes of symmetry by checking vertex reflection properties.
* **Interactive 3D Visualization:**
    * Rendered using Three.js in an HTML canvas.
    * **Spinning Animation:** The model automatically spins.
        * **Pause/Resume Control:** A button to pause or resume the animation.
        * **Speed Control:** A slider to adjust the rotation speed.
    * **Orbit Controls:** Full camera control using the mouse:
        * **Rotate:** Left-click and drag.
        * **Zoom:** Mouse scroll wheel.
        * **Pan:** Right-click and drag (or middle-click and drag).
    * **Display Modes:**
        * **Wireframe View:** Shows the edges of the polyhedron (default: green).
        * **Solid View:** Shows the polyhedron as a solid object with distinct faces.
        * **Toggle Button:** "Show Solid" / "Show Wireframe" to switch between modes. The "Color Faces" button will be enabled/disabled accordingly.
    * **Vertex Labels:**
        * Numerical labels (0-7) displayed at each vertex.
        * **Toggle Button:** "Hide Labels" / "Show Labels".
        * **Occlusion:** In solid view, labels hidden behind the model are automatically occluded (not drawn). Labels are hidden when pieces are separated.
    * **Face Coloring (Solid Mode Only):**
        * **Toggle Button:** "Color Faces" / "Monochrome" (enabled only in solid view).
        * **Multi-Color Mode:** Assigns distinct colors to the 12 faces (or the faces of separated pieces) using a greedy algorithm to minimize adjacent faces having the same color. Material properties are adjusted for a flatter, matte appearance of colored faces.
        * **Monochrome Mode:** Displays the solid with a default single color (e.g., blue-gray). When pieces are separated in this mode, one piece is rendered in a distinct color (e.g., orange) for better visual differentiation.
    * **Sharp Edge Delineation:** In solid view, edges are rendered with a contrasting line overlay for clear face separation.
    * **Piece Separation:**
        * **Toggle Button:** "Separate Pieces" / "Join Pieces".
        * Animates the separation of the snub disphenoid into two distinct 6-face shells.
        * The separation occurs through pure translation in opposite directions along an axis defined by the midpoint of edge (V6,V7) and the midpoint of edge (V4,V5).
        * Each 6-face shell has its interior visible (rendered with `THREE.DoubleSide`).
        * The two shells maintain their original orientation relative to each other during separation and joining.

## How to Use

1.  **Open the HTML File:** Save the provided code as an HTML file (e.g., `snub_disphenoid_visualizer.html`) and open it in a modern web browser that supports WebGL (e.g., Chrome, Firefox, Edge, Safari).
2.  **Automatic Load:** The visualization and calculations will begin automatically when the page loads.
3.  **Check Console for Data:** Open your browser's developer console (usually by pressing `F12`) to view:
    * Calculated parameters (`q`, `r`, `s`, `t`).
    * Coordinates of the 8 vertices.
    * List of the 18 edges.
    * Results of the geometric property verifications.
4.  **Interact with On-Screen Controls:**
    * **Pause/Resume:** Click to start or stop the spinning animation.
    * **Speed Slider:** Drag to change the rotation speed.
    * **Show Solid / Show Wireframe:** Click to switch between the solid and wireframe rendering of the model (or separated pieces). The "Color Faces" button will be enabled/disabled accordingly.
    * **Hide Labels / Show Labels:** Click to toggle the visibility of the numerical vertex labels. (Labels are automatically hidden when pieces are separated).
    * **Color Faces / Monochrome:** (Available in Solid mode only) Click to switch the solid model (or separated pieces) between multi-color face rendering and single-color rendering.
    * **Separate Pieces / Join Pieces:** Click to animate the separation or joining of the model into/from two 6-face shells.
5.  **Navigate the 3D View:**
    * **Rotate:** Hold the left mouse button and drag.
    * **Zoom:** Use the mouse scroll wheel.
    * **Pan:** Hold the right mouse button (or middle mouse button) and drag.

## Technical Details

* **Formulas:** Vertex coordinate calculations are based on the formulas provided at [polytope.miraheze.org/wiki/Snub_disphenoid](https://polytope.miraheze.org/wiki/Snub_disphenoid).
* **Technologies Used:**
    * HTML
    * CSS (Tailwind CSS for styling UI elements)
    * JavaScript (for calculations, DOM manipulation, and 3D logic)
    * Three.js (r128, for WebGL-based 3D rendering, orbit controls, and 2D label rendering)

## Files

* The primary code is self-contained within a single HTML file.

---

This README should provide a good overview for anyone looking to understand or use the Snub Disphenoid Visualizer.
