# Benchmarks: against Blender Decimate and meshoptimizer

## Where it stands

Two comparisons have been measured in full, both with the repo's own test
harnesses, plus a first look at MeshLab on two small meshes. Nothing else
has been, so this page makes no claims about Simplygon, InstaLOD, ZBrush
Decimation Master or any other reducer.

**Against Blender Decimate** (2026-10-10, engine 2.3.2):

- 2.2–3.0× faster on meshes of 1–7.5 million triangles, about even at
  100k, slower (0.6×) on a scene of 103 small parts.
- The same shape at the same triangle count. Decimate also reaches its
  target, so exact counts are not what sets the two apart.
- Ahead on CAD with Hard Surface on (creases stay where they are), and on
  everything Decimate does not do: Keep Normals, UV-aware collapses, scan
  repair, Import and Crush, bakes, LOD chains, rigs and shape keys carried
  over.
- Behind with Keep UVs on a part cut into many small UV islands at a very
  low target, because seam vertices stay on their islands.

**Against meshoptimizer** (2026-10-13, engine 2.7.1):

- The same surface error, within measurement noise, on scans, sculpts and
  smooth CAD at keep 10% and keep 2%; slightly lower on a small CAD block.
- On a hard-surface model of many parts the picture is mixed. Both tools'
  default settings stop short of the target on it; 2.7.1 fixed that for
  Polygon Crusher, and meshoptimizer's attribute setting still has the
  lower error at keep 10%. At keep 2% they are close.
- meshoptimizer is about three times faster, on one core; Polygon Crusher
  uses all cores. On a single core the gap would be wider.

**Against MeshLab** (early readings, two meshes, see below): about the
same at keep 10%; MeshLab's plain decimation is better on the small CAD
block at keep 2%, and its UV-keeping variant is much worse there.

In short: a match for the best open-source simplifier on surface error,
slower per core, better integrated for Blender work, and since 2.7.1 no
longer the tool that falls over on multi-part hard-surface models, without
being the best one on those either. The sections below show the numbers.

---

## Against Blender Decimate

Measured on 2026-10-10 with Polygon Crusher 2.3.2. This section shows how the
speed figure on the store page was measured, so you can judge it yourself.

These numbers still apply to 2.4.x. 2.4 changed the panel, the bake, and a
Keep Normals correction that only runs on CAD and hard-surface parts. 2.4.2
added two small engine checks (no stray spike vertices, better detail in
protected areas) that leave scan, sculpt and CAD results unchanged. 2.5.0
changed only the bake (base color from image textures, fewer tiny UV islands
on scans); the crush itself is the same. 2.6.0 added Low Distortion under
Keep UVs, which costs a few percent of crush time. 2.7.1 changed how kept
edges and small pieces are handled; scans, sculpts and smooth CAD parts
measure the same as before, hard-surface models with many parts do not (see
[Against meshoptimizer](#against-meshoptimizer) below). None of these were
re-timed against Decimate.

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

---

## Against meshoptimizer

Measured on 2026-10-13, engine 2.7.0 and then 2.7.1, against
[meshoptimizer](https://github.com/zeux/meshoptimizer) v1.3 (the open-source
simplifier behind Godot and the glTF tools), built from source. One Windows
machine. This is the test that found the two 2.7.1 fixes, so it is shown
here with both results.

### Method

- The engine binary alone, not the add-on, on OBJ files: the 1.26M rock scan
  and the 98k boulder scan from above, a 60k noisy scan, the 1.0M sculpt, a
  5k sculpt, the 5.1k CAD block, an 83k smooth CAD part, and an electricity
  pole of 103 parts (13.4k triangles).
- Targets: keep 10% and keep 2% of the triangles.
- Polygon Crusher runs with the flags its mode would use (Hard Surface for
  the CAD parts and the pole, Scan and Sculpt for the rest). meshoptimizer
  runs with its defaults and, where it does better, with its attribute
  settings (normals and UVs, permissive).
- **Error:** distance between the original surface and the result, as % of
  the bounding-box diagonal, 200,000 sample points each way. "Mean" is the
  average; "worst" is the 99th percentile, so a few stray points do not set
  it.
- **Time:** the engine only. meshoptimizer runs on one core; Polygon Crusher
  uses all of them.

### Scans, sculpts and smooth CAD: the same error

At keep 10%, mean error of the two tools, Polygon Crusher / meshoptimizer:

| Case | Mean error (PC / mo) | Worst (PC / mo) |
|---|---|---|
| Rock scan 1.26M → 126k | 0.069 / 0.069 | 0.16 / 0.16 |
| Boulder scan 98k → 9.8k | 0.123 / 0.127 | 0.28 / 0.28 |
| Noisy scan 60k → 6k | 0.173 / 0.182 | 0.58 / 0.48 |
| Sculpt 1.0M → 101k | 0.101 / 0.101 | 0.24 / 0.24 |
| Smooth CAD part 83k → 8.3k | 0.163 / 0.163 | 0.40 / 0.39 |
| CAD block 5.1k → 512 | 0.163 / 0.188 | 0.38 / 0.52 |

Keep 2% looks the same: the two tools are within measurement noise of each
other on every one of these, before and after 2.7.1. (The 1.26M scan and the
1.0M sculpt were only run with the 2.7.0 engine; 2.7.1 did not change the
scan or sculpt path.)

### The multi-part pole: what 2.7.1 fixed

The pole has borders, material edges, seams and sharp edges everywhere, and
103 separate pieces. It is where the old engine failed.

| Pole 13.4k, keep 2% | Triangles | Mean error | Worst error |
|---|---|---|---|
| PC 2.7.0, plain crush | 268 | 2.48 | **20.7** |
| PC 2.7.0, Hard Surface mode | 7,081 (stalled at 53%) | 0.21 | 3.3 |
| PC 2.7.1, plain crush | 266 | 0.75 | 5.2 |
| PC 2.7.1, Hard Surface mode | 266 | 0.42 | 4.2 |
| meshoptimizer, defaults | 2,788 (stopped at 21%) | 0.27 | 5.9 |
| meshoptimizer, attributes | 260 | 0.39 | 5.7 |

- At keep 2% the 2.7.0 plain crush disintegrated: whole beams went while
  small insulators stayed, hence a worst-case error of 20.7% of the model's
  size. In 2.7.1 every piece keeps a minimum until the budget is spent, and
  the worst case is 5.2%.
- Hard Surface mode on 2.7.0 stalled at about half the triangles at both
  targets (7,091 at keep 10%, 7,081 at keep 2%), because the kept edges
  were a hard lock. In 2.7.1 it reaches 1,337 and 266.
- meshoptimizer with its defaults also stops short on this model, at 2,816
  triangles whatever you ask; with its attribute settings it reaches the
  count. At keep 10% that setting has the lowest error of all the runs
  (mean 0.17, worst 0.84, against 0.23 and 1.30 for Hard Surface mode).
  At keep 2% the two are close: lower mean for meshoptimizer, lower worst
  case for Polygon Crusher.

On the 5.1k CAD block, Hard Surface mode at keep 10% halved its error
(mean 0.31 → 0.16, worst 1.23 → 0.38) because the hole rims are no longer
locked. At keep 2% it now reaches 102 triangles where 2.7.0 stopped at 272,
and that costs shape: mean error 1.98 against 1.13 at the old, higher count.
Reaching a very low count on a small part is a trade you choose by picking
the target.

### Speed

meshoptimizer is about three times faster: 0.8 s against 2.3 s on the 1.26M
scan at keep 10%, 0.5 s against 1.3 s on the 1.0M sculpt. It gets there on
one core; Polygon Crusher spreads its work over all of them. Both are well
under the time Blender needs to build the result mesh.

### Limits of this test

- Engine against library, both run from Python on one Windows machine. The
  add-on's own read, build and bake steps are not included, and neither is
  any glTF or game-engine pipeline around meshoptimizer.
- meshoptimizer has more settings than the two tried here; a tuned run may
  do better on the pole.
- Error is a surface-distance measurement. It does not see shading, UV
  stretch or whether a hole is still round; look at the result.

---

## Against MeshLab: early readings

MeshLab's **Quadric Edge Collapse Decimation** (through pymeshlab
2025.7) is the reducer most people run on scans outside game studios. It
was added to the bake-off harness on 2026-10-13 and has so far been run on
two of the eight fixtures, on Linux, with the 2.7.1 engine. Two small
meshes are not a verdict; the full set, on the Windows machine used above,
is still to come. Same error measure as the meshoptimizer section;
Polygon Crusher with its mode's flags, MeshLab with the filter's defaults
("plain") and with the with-texture variant that keeps the UV layout
("UVs").

| Case | Polygon Crusher (mean / worst) | MeshLab plain | MeshLab UVs |
|---|---|---|---|
| CAD block 5.1k → 512 | 0.163 / 0.38 | 0.169 / 0.41 | 0.50 / 4.0 |
| CAD block 5.1k → 102 | 1.98 / 12.1 | **0.99 / 5.4** | 3.5 / 21.8 |
| Sculpt 5.1k → 514 | 0.114 / 0.23 | 0.120 / 0.27 | 0.130 / 0.27 |
| Sculpt 5.1k → 102 | 0.364 / 0.85 | 0.437 / 1.87 | 0.59 / 2.3 |

- At keep 10% the two are within a few percent of each other on both
  meshes.
- At keep 2% on the CAD block MeshLab's plain decimation is clearly better:
  half the mean error, half the worst case. Polygon Crusher's Hard Surface
  mode is paying there for reaching 102 triangles while still favouring the
  kept edges; the plain crush (no edges kept) scores 1.63 / 10.2, also
  behind MeshLab. This is the one case so far where another tool is ahead
  on a plain surface-error measure, and it will be checked on the full set.
- MeshLab's UV-keeping variant is far worse on the CAD block at both
  targets: the seams constrain it the way Keep UVs constrains any
  simplifier, and this block has many of them.
- On the sculpt Polygon Crusher is ahead at both targets, more so at keep 2%.
- Times are not comparable yet: a different machine from the rest of this
  page, and meshes this small finish in a few hundredths of a second
  either way.
