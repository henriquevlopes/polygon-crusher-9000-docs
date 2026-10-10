# Benchmark: Polygon Crusher vs Blender Decimate

Measured on 2026-10-10 with Polygon Crusher 2.3.1. This page shows how the
numbers on the store page were measured, so you can judge them yourself.

## Summary

- **Speed:** 2.6–3.2× faster than Blender's Decimate on meshes of 1–7.5
  million triangles. About the same at 100k. Slower on many small parts.
- **Textures:** at the same triangle count, far fewer texture points end up
  misaligned (1.7–6.9% vs 9–29% with default settings; 0.2% with UV Seams on).
- **Shape:** about the same as Decimate on organic meshes. On CAD parts with
  **Hard Surface** on, creases stay exactly in place.
- **Defaults matter:** on open-border parts, turn on **Hard Surface**. At very
  low targets on textured meshes, turn on **UV Seams**. Without them,
  Decimate can do better on those two cases (see the tables).

## Method

- Blender 5.2.2 LTS, run headless. Windows 11, Intel i7-12700H (20 threads),
  64 GB RAM.
- Test meshes: a CC0 Poly Haven rock scan (1.26M triangles, textured), a
  smaller boulder scan (98k), a 1.0M-triangle synthetic sculpt, a
  7.56M-triangle subdivided scan, a CAD part (5.1k), and a set of 103
  hard-surface parts (200k total).
- **Blender:** Decimate modifier in Collapse mode, ratio = target ÷ input,
  applied to a new mesh.
- **Polygon Crusher:** the add-on's own pipeline (read the mesh with UVs and
  normals, run the native engine with an exact target, build the result with
  UVs and custom normals). No preserve options unless a row says so. No bake.
- **Time:** end to end, median of 3 runs.
- **Shape loss:** distance from each surface to the other, as % of the
  bounding-box diagonal (mean / max).
- **Texture misaligned:** for sampled corners of the result, compare the
  result's UV with the original's UV at the nearest point on the original
  surface. "Misaligned" means off by more than 0.01 in UV space, about 20
  pixels on a 2K texture.

## Speed and exact counts

| Case | Decimate | Polygon Crusher | Speed-up | Result triangles (Decimate / PC) |
|---|---|---|---|---|
| Textured scan 1.26M → 20k | 5.18 s | 2.01 s | 2.6× | 20,000 / 19,999 |
| Sculpt 1.0M → 20k | 3.57 s | 1.34 s | 2.7× | 19,999 / 19,999 |
| Subdivided scan 7.56M → 450k | 44.5 s | 14.0 s | 3.2× | 449,999 / 450,000 |
| Scan 98k → 5k | 0.26 s | 0.22 s | 1.2× | 5,000 / 5,000 |
| 103 hard-surface parts, 25% each | 0.44 s | 0.62 s | 0.7× (slower) | 50,119 / 50,134 |

## Quality at the same triangle count

| Case | Texture misaligned (Decimate) | Texture misaligned (PC) | Shape loss % (Decimate) | Shape loss % (PC) |
|---|---|---|---|---|
| Textured scan 1.26M → 20k, defaults | 9.3% | 4.2% | 0.025 / 0.42 | 0.027 / 0.79 |
| Textured scan 1.26M → 2k, defaults | 11.3% | 20.6% | 0.085 / 1.07 | 0.108 / 1.90 |
| Textured scan 1.26M → 2k, UV Seams on | 11.3% | **0.2%** | 0.085 / 1.07 | 0.218 / 3.46 |
| Scan 98k → 5k, defaults | 16.5% | 6.9% | 0.086 / 0.79 | 0.102 / 2.24 |
| Hard-surface part 13.8k → 3.4k, defaults | 29.1% | 1.7% | 0.098 / 4.48 | 0.534 / 9.22 |
| Same part, Hard Surface on | 29.1% | 10.2% (stops at 5.4k) | 0.098 / 4.48 | 0.097 / 3.27 |
| CAD part 5.1k → 1k, Hard Surface on | 1.6% | 2.4% | 0.0026 / 0.071 | 0.0000 / 0.0001 |

## Limits of this test

- One machine, Windows only. Your times will differ.
- The 7.56M mesh is a subdivided scan, not a raw capture.
- Shape and texture numbers are measurements, not renders. They don't
  capture everything you see in shading.
- Some add-on steps (scan-noise detection for Sharp Edges, crease normal
  checks, the report) and the UI/undo overhead are not included, for either
  tool.
