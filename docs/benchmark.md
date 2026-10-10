# Benchmark: Polygon Crusher vs Blender Decimate

Measured on 2026-10-10 with Polygon Crusher 2.3.1. This page shows how the
speed figure on the store page was measured, so you can judge it yourself.

## Summary

- **Speed:** 2.6–3.2× faster than Blender's Decimate on meshes of 1–7.5
  million triangles. About the same at 100k. Slower on many small parts.
- **Shape:** about the same as Decimate at the same triangle count. On CAD
  parts with **Hard Surface** on, creases stay exactly in place.
- **Imported scans:** meshes imported from glTF or FBX are often split into
  separate pieces along their UV seams. Turn on **Clean Mesh** for these, or
  open edges can shrink and leave gaps.

## Method

- Blender 5.2.2 LTS, run headless. Windows 11, Intel i7-12700H (20 threads),
  64 GB RAM.
- Test meshes: a CC0 Poly Haven rock scan (1.26M triangles), a smaller
  boulder scan (98k), a 1.0M-triangle synthetic sculpt, a 7.56M-triangle
  subdivided scan, a CAD part (5.1k), and a set of 103 hard-surface parts
  (200k total).
- **Blender:** Decimate modifier in Collapse mode, ratio = target ÷ input,
  applied to a new mesh.
- **Polygon Crusher:** the add-on's own pipeline (read the mesh with UVs and
  normals, run the native engine with an exact target, build the result with
  UVs and custom normals). No bake.
- **Time:** end to end, median of 3 runs.
- **Shape loss:** distance from the original surface to the result, as % of
  the bounding-box diagonal (mean / max).

## Speed and exact counts

| Case | Decimate | Polygon Crusher | Speed-up | Result triangles (Decimate / PC) |
|---|---|---|---|---|
| Rock scan 1.26M → 20k | 5.18 s | 2.01 s | 2.6× | 20,000 / 19,999 |
| Sculpt 1.0M → 20k | 3.57 s | 1.34 s | 2.7× | 19,999 / 19,999 |
| Subdivided scan 7.56M → 450k | 44.5 s | 14.0 s | 3.2× | 449,999 / 450,000 |
| Scan 98k → 5k | 0.26 s | 0.22 s | 1.2× | 5,000 / 5,000 |
| 103 hard-surface parts, 25% each | 0.44 s | 0.62 s | 0.7× (slower) | 50,119 / 50,134 |

## Shape at the same triangle count

Polygon Crusher with **Clean Mesh** on (these test meshes are glTF imports,
split along UV seams).

| Case | Decimate (mean / max) | Polygon Crusher (mean / max) |
|---|---|---|
| Rock scan 1.26M → 20k | 0.025 / 0.42 | 0.023 / 0.79 |
| Rock scan 1.26M → 2k | 0.085 / 1.07 | 0.079 / 1.38 |
| Boulder scan 98k → 5k | 0.086 / 0.79 | 0.083 / 1.61 |
| Hard-surface part 13.8k → 3.4k | 0.098 / 4.48 | 0.106 / 1.88 |
| CAD part 5.1k → 1k, Hard Surface on | 0.0026 / 0.071 | 0.0000 / 0.0001 |

## Limits of this test

- One machine, Windows only. Your times will differ.
- The 7.56M mesh is a subdivided scan, not a raw capture.
- Shape numbers are measurements, not renders. They don't capture
  everything you see in shading.
- Some add-on steps (scan-noise detection for Sharp Edges, crease normal
  checks, the report) and the UI/undo overhead are not included, for either
  tool.
