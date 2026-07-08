# Overview
purpose: create a demo project for a first degree sw dev student to add to github portfolio.
This is his first project so it should be simple but functional.

Update: student wants to start much smaller than a full degree-level project — just get a first working thing done, then iterate in stages toward a fuller demo.

## Task
first step propse ideas for the project 
include the ideas here.

## Constraints
- C++ with CUDA (GPU programming) — this should demonstrate real parallel computing understanding, not just a wrapper around a library.
- Visual/interactive demo preferred **eventually** — something screenshot/video-friendly for a portfolio. Not required for the first working version.
- Should stay simple enough for a first-degree student to actually finish, while still being "functional" and demo-worthy.
- No guaranteed real GPU access on the student's own machine — plan for development/testing via free cloud options (Google Colab / Kaggle notebooks support compiling and running CUDA code) or a physical NVIDIA GPU if available (check with `nvidia-smi`).

## Project Ideas

### 1. N-Body Gravity Simulation (Recommended)
Simulate hundreds to tens of thousands of particles attracting each other under gravity, rendered live.
- **Core GPU concept**: each particle's force computation is independent → classic embarrassingly-parallel CUDA kernel (O(n²) all-pairs, one thread per particle).
- **Demo value**: visually striking (galaxies, swirling clusters), and you can show a CPU-vs-GPU speed comparison slider/counter live — great portfolio video.
- **Stretch goals**: shared-memory tiling optimization, spatial partitioning (Barnes-Hut) for extra credit/depth, adjustable particle count with live FPS counter.
- **Rendering**: OpenGL/CUDA interop (write positions to a VBO) keeps it simple — no need for a separate renderer.

### 2. Real-Time Mandelbrot / Julia Set Explorer
Render and let the user pan/zoom into a fractal in real time, with each pixel computed independently on the GPU.
- **Core GPU concept**: perfect one-thread-per-pixel parallelism, easy to reason about and explain in a defense/interview.
- **Demo value**: instantly recognizable, satisfying to zoom live, easy to record a demo GIF.
- **Stretch goals**: double-precision zoom limits → arbitrary precision, colored by iteration count, switch between Mandelbrot/Julia/Burning Ship.
- **Scope**: smallest of the three — good fallback if time is tight.

### 3. GPU Ray Tracer (Simple Whitted-style)
Render a scene of spheres/planes with reflections and shadows, one thread per pixel casting rays.
- **Core GPU concept**: parallel ray-scene intersection tests; naturally extends to soft shadows/reflections as stretch goals.
- **Demo value**: "ray tracer" is a strong portfolio/résumé keyword, renders look impressive for the effort.
- **Stretch goals**: real-time camera movement, simple denoising, basic BVH acceleration structure.
- **Scope**: most ambitious of the three — best if the student has more time or wants to push further for the degree project itself.

## Recommendation (original, for a full degree project)
Start with the **N-Body Simulation**: it has the clearest story about *why* GPU parallelism matters (visible, measurable speedup), is scoped to finish in a reasonable timeframe, and leaves natural stretch goals (Barnes-Hut, tiling) to deepen it if more rigor is needed for degree evaluation.

## Revised plan: staged approach, starting small

Given the student wants to start with something minimal and grow it later, we're splitting the fractal idea (#2 above) into stages instead of building it all at once.

### Stage 1 (first target — start here)
A small C++/CUDA program that:
- computes a Mandelbrot set image, one GPU thread per pixel (each pixel's value is fully independent — simplest possible parallel pattern to write and explain)
- writes the result to an image file (e.g. PPM/PNG) on disk
- no live rendering window, no zoom/pan, no user interaction — just "run it, get a picture"

This alone is a complete, demoable, explainable CUDA program: it shows a real parallel kernel, a measurable CPU-vs-GPU comparison is easy to bolt on, and it fits in a short first session.

### Stage 2 (later)
Add a live interactive window (OpenGL) so the fractal renders in real time and the user can pan/zoom instead of producing a single static file.

### Stage 3 (later)
Optional depth, pick based on interest: colored-by-iteration-count rendering, switching between Mandelbrot/Julia/Burning Ship, or moving on to the N-Body simulation (#1 above) as a bigger follow-up project reusing what was learned about CUDA kernels.

## Next step
Prepare a requirements doc for Stage 1 only (the static Mandelbrot image generator), broken into user stories. Later stages get their own requirements docs once Stage 1 is done.