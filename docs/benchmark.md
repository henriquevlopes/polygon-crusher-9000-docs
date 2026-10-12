# Benchmark: Polygon Crusher vs Blender Decimate

Measured on 2026-10-10 with Polygon Crusher 2.3.2. This page shows how the
speed figure on the store page was measured, so you can judge it yourself.

These numbers still apply to 2.4.x. 2.4 changed the panel, the bake, and a
Keep Normals correction that only runs on CAD and hard-surface parts. 2.4.2
added two small engine checks (no stray spike vertices, better detail in
protected areas) that leave scan, sculpt and CAD results unchanged. 2.5.0
changed only the bake (base color from image textures, fewer tiny UV islands
on scans); the crush itself is the same. None of these were re-timed.

## Summary

- **Speed:** 2.2–3.0× faster than Blender's Decimate on meshes of 1–7.5
  million triangles. About the same at 100k. Slower on many small parts.
- **Shape:** about the same as Decimate at the same triangle count. On CAD
  parts with **Hard Surface** on, creases stay exactly in place. With
  **Keep UVs** on, very low targets on parts cut into many small UV islands
  keep less shape, because seam vertices stay on their islands.
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
| Rock scan 1.26M → 20k | 5.40 s | 2.42 s | 2.2× | 20,000 / 19,999 |
| Sculpt 1.0M → 20k | 3.68 s | 1.44 s | 2.6× | 19,999 / 20,000 |
| Subdivided scan 7.56M → 450k | 45.0 s | 14.9 s | 3.0× | 449,999 / 450,000 |
| Scan 98k → 5k | 0.27 s | 0.21 s | 1.2× | 5,000 / 5,000 |
| 103 hard-surface parts, 25% each | 0.45 s | 0.71 s | 0.6× (slower) | 50,119 / 50,134 |

## Shape at the same triangle count

Polygon Crusher with **Clean Mesh** on (these test meshes are glTF imports,
split along UV seams), with **Keep UVs** on and off. With Keep UVs on, a
vertex on a UV island border only slides along that border, so textures
stay clean but the shape has less freedom.

| Case | Decimate (mean / max) | PC, Keep UVs on | PC, Keep UVs off |
|---|---|---|---|
| Rock scan 1.26M → 20k | 0.025 / 0.42 | 0.024 / 0.81 | 0.023 / 0.79 |
| Rock scan 1.26M → 2k | 0.085 / 1.07 | 0.092 / 1.56 | 0.079 / 1.38 |
| Boulder scan 98k → 5k | 0.086 / 0.79 | 0.084 / 1.67 | 0.083 / 1.61 |
| Hard-surface part 13.8k → 3.4k | 0.098 / 4.48 | 0.213 / 9.06 | 0.106 / 1.88 |
| CAD part 5.1k → 1k, Hard Surface on | 0.0026 / 0.071 | 0.0000 / 0.0001 | 0.0000 / 0.0001 |

The hard-surface part has many small UV islands, so with Keep UVs on its
thin cables lose the most. Turn Keep UVs off when you don't need the
texture.

## Limits of this test

- One machine, Windows only. Your times will differ.
- The 7.56M mesh is a subdivided scan, not a raw capture.
- Shape numbers are measurements, not renders. They don't capture
  everything you see in shading.
- Some add-on steps (scan-noise detection for Sharp Edges, crease normal
  checks, the report) and the UI/undo overhead are not included, for either
  tool.
