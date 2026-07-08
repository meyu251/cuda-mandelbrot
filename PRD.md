# Cuda-mandelbrot PRD

## Overview
Stage 1 of the cuda-mandelbrot project (per vision-kickoff.md): a small C++/CUDA
command-line program that computes a Mandelbrot set image on the GPU — one thread
per pixel — and writes it to an image file on disk. No live window, no pan/zoom,
no interactivity. Goal is a complete, working, explainable CUDA program that fits
in a short first session and is portfolio-demoable via a screenshot of the output
image.

## Goals
- Produce a correct, recognizable Mandelbrot image using a real CUDA kernel (not
  a CPU fallback wrapped to look like GPU code).
- Keep the scope small enough for a first-degree student to finish end-to-end.
- Make the CPU-vs-GPU performance story easy to demonstrate later (Stage 1 should
  not block that, even if it isn't built yet).
- Run on free cloud CUDA environments (Google Colab / Kaggle) as well as a local
  NVIDIA GPU if available.

## Non-Goals (deferred to later stages)
- Live/interactive rendering window (Stage 2).
- Pan/zoom, arbitrary precision zoom (Stage 2).
- Colored-by-iteration-count palettes, Julia/Burning Ship variants (Stage 3).
- N-Body simulation follow-up (Stage 3+).

## User Stories

### US-1: Generate a Mandelbrot image
As a student showcasing this project, I want to run one command and get a PNG/PPM
image of the Mandelbrot set, so that I have a concrete artifact to show and explain.
- **Acceptance criteria**
  - Running the compiled binary produces an image file on disk (e.g. `mandelbrot.ppm`
    or `.png`).
  - The image visually matches the known Mandelbrot set shape.
  - No manual steps beyond running the executable are required to get the image.

### US-2: Compute pixels in parallel on the GPU
As a student learning CUDA, I want each pixel's Mandelbrot iteration computed by
its own GPU thread, so that the project demonstrates genuine data-parallel GPU
programming I can explain in an interview.
- **Acceptance criteria**
  - A CUDA kernel launches with one thread mapped to one pixel (via thread/block
    indexing), not a loop that serializes work on a single thread.
  - The kernel computes the standard escape-time Mandelbrot iteration count per
    pixel and writes it to GPU memory.
  - Kernel output is copied back to host memory before being written to a file.

### US-3: Configure image parameters
As a student, I want to set basic parameters (image width/height, max iterations,
viewport bounds) before compiling or via simple constants/arguments, so that I can
adjust the output without rewriting the algorithm.
- **Acceptance criteria**
  - Width, height, and max iteration count are adjustable (constants or CLI args
    are both acceptable for Stage 1).
  - Default parameters produce a reasonable, clearly-Mandelbrot image (e.g. 800x600,
    the classic full-set viewport).

### US-4: Build and run on a CUDA-capable environment
As a student without guaranteed local GPU access, I want clear build instructions
that work on Google Colab/Kaggle notebooks or a local machine with `nvidia-smi`
available, so that I can actually compile and run the project.
- **Acceptance criteria**
  - A documented build command (e.g. `nvcc`) compiles the program without errors
    on a standard CUDA toolkit install.
  - README or comment block documents the Colab/Kaggle path (installing/using
    `nvcc`, running the compiled binary) as an alternative to a local GPU.

### US-5: Verify correctness
As a student, I want a simple way to confirm the output is correct, so that I trust
the GPU kernel is actually computing the right thing before presenting it.
- **Acceptance criteria**
  - Documented or scripted way to visually inspect the output image (e.g. open the
    PPM/PNG).
  - Edge case sanity check: points known to be inside the set (e.g. origin) hit
    max iterations; points known to escape quickly (e.g. far outside the set) have
    low iteration counts.

## Success Criteria for Stage 1
- Program compiles with `nvcc` and runs on at least one CUDA-capable environment
  (local GPU or Colab/Kaggle).
- Output image is a correct, recognizable Mandelbrot set.
- Core computation is implemented as a genuine parallel CUDA kernel (one thread
  per pixel), suitable for explaining in a portfolio/interview context.

## Open Questions
- Output format: PPM (simplest, no dependencies) vs PNG (needs a library like
  `stb_image_write.h`)? PPM recommended for Stage 1 to minimize dependencies.
- CLI args vs compile-time constants for parameters — CLI args are nicer but add
  scope; compile-time constants are acceptable for a first version.