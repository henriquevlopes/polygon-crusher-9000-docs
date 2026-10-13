# Benchmarks: against Blender Decimate and meshoptimizer

## Where it stands

Three comparisons have been measured, all with the repo's own test
harnesses: Blender Decimate, meshoptimizer and MeshLab. Nothing else has
been, so this page makes no claims about Simplygon, InstaLOD, ZBrush
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

**Against MeshLab** (2026-10-13, engine 2.7.1, six meshes, see below):

- About the same at keep 10% on everything; Polygon Crusher a little ahead
  on the sculpt and the CAD block, MeshLab a little ahead on worst case on
  the noisy scan.
- At keep 2% it splits: Polygon Crusher is clearly better on the sculpt,
  MeshLab clearly better on the small CAD block, and on the noisy scan
  both meshoptimizer and MeshLab have a lower worst case.
- MeshLab's UV-keeping variant stops short of the target on the pole and
  is far worse on the CAD block; its plain decimation reaches every
  target.
- On the same machine MeshLab is about three times slower than Polygon
  Crusher, which is about six times slower than meshoptimizer.

In short: a match for the open-source simplifiers on surface error at
normal targets, ahead on sculpts, behind on a small CAD part and on the
worst case of a noisy scan at extreme targets, between the two on speed,
better integrated for Blender work, and since 2.7.1 no longer the tool
that falls over on multi-part hard-surface models, without being the best
one on those either. The sections below show the numbers.

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

## Against MeshLab

MeshLab's **Quadric Edge Collapse Decimation** (through pymeshlab 2025.7)
is the reducer most people run on scans outside game studios. Measured on
2026-10-13 with the 2.7.1 engine on the six fixtures that ship in the
repository (the 1.26M scan and 1.0M sculpt above are not among them), on
one Linux machine, with meshoptimizer v1.3 run again in the same session so
all three tools are on the same hardware. Same error measure as the
meshoptimizer section. Polygon Crusher runs with its mode's flags; MeshLab
with the filter's defaults ("plain") and with the with-texture variant that
keeps the UV layout ("UVs"); meshoptimizer's better of its two settings.

Mean / worst error, % of the model's size:

| Case | keep | Polygon Crusher | MeshLab plain | MeshLab UVs | meshoptimizer |
|---|---|---|---|---|---|
| CAD block 5.1k | 10% | 0.163 / 0.38 | 0.169 / 0.41 | 0.50 / 4.0 | 0.188 / 0.52 |
| CAD block 5.1k | 2% | 1.98 / 12.1 | **0.99 / 5.4** | 3.5 / 21.8 | 1.38 / 8.0 |
| Smooth CAD part 83k | 10% | 0.163 / 0.39 | 0.163 / 0.40 | 0.163 / 0.40 | 0.163 / 0.39 |
| Smooth CAD part 83k | 2% | 0.163 / 0.40 | 0.163 / 0.39 | 0.187 / 1.69 | 0.164 / 0.39 |
| Sculpt 5.1k | 10% | **0.114 / 0.23** | 0.119 / 0.27 | 0.129 / 0.27 | 0.209 / 0.47 |
| Sculpt 5.1k | 2% | **0.364 / 0.86** | 0.436 / 1.87 | 0.59 / 2.3 | 0.92 / 1.9 |
| Pole 13.4k, 103 parts | 10% | 0.233 / 1.40 | 0.241 / 4.1 | stopped at 14% | **0.170 / 0.85** |
| Pole 13.4k, 103 parts | 2% | 0.424 / 4.2 | 0.470 / 6.9 | stopped at 14% | 0.393 / 5.7 |
| Noisy scan 60k | 10% | 0.173 / 0.58 | 0.181 / 0.50 | 0.167 / 0.45 | 0.182 / 0.48 |
| Noisy scan 60k | 2% | 0.322 / 1.69 | 0.326 / 1.30 | 0.268 / 1.19 | **0.264 / 0.88** |
| Boulder scan 98k | 10% | 0.123 / 0.28 | 0.129 / 0.30 | 0.124 / 0.29 | 0.127 / 0.28 |
| Boulder scan 98k | 2% | 0.181 / 0.70 | 0.244 / 0.81 | 0.182 / 0.73 | 0.186 / 0.51 |

What it says:

- **Keep 10%: the three tools are within a few percent of each other on
  every mesh.** Polygon Crusher is a little ahead on the sculpt and the
  CAD block; on the noisy scan its worst case is the highest of the three
  (0.58 against 0.45–0.50); on the pole meshoptimizer's attribute setting
  is ahead, as the section above says.
- **Keep 2% is where they differ.** Polygon Crusher is clearly better on
  the sculpt (worst case 0.86 against 1.9). MeshLab's plain decimation is
  clearly better on the small CAD block (half the mean, half the worst
  case). On the noisy scan both others beat Polygon Crusher on worst case
  (0.88 and 1.19 against 1.69), and on the boulder scan meshoptimizer does
  (0.51 against 0.70) while the means are equal. Part of the CAD-block gap
  is the 2.7.1 decision to always reach the count: the 2.7.0 engine stopped
  at 272 triangles on this block with a lower error than 2.7.1 gets at
  102. Keep 2% of a 5,000-triangle part is 102 triangles, which is below
  where any of the three keeps the shape; on a part this small, pick the
  target by looking at the result, not the percentage.
- **MeshLab's UV-keeping variant** stops at 1,816 triangles on the pole
  whatever you ask (14% of the input, at both targets) and is far worse on
  the CAD block, for the same reason meshoptimizer's defaults stop on the
  pole: seams and borders it will not cross. On scans it is MeshLab's
  better setting. Polygon Crusher keeps UVs on the scans too (Scan mode
  has Keep UVs and Low Distortion on) and still reaches every target.
- **Speed, same machine:** on the 83k and 98k meshes Polygon Crusher takes
  about 0.3 s, meshoptimizer about 0.05 s, MeshLab about 1.0–1.4 s. So
  MeshLab is about three times slower than Polygon Crusher, which is about
  six times slower than meshoptimizer here. The Polygon Crusher time
  includes starting the engine process and reading and writing the mesh
  files, which the two library calls do not pay; on meshes this small that
  is a real share of its 0.3 s, so its true gap to meshoptimizer is
  smaller than six times and the 1.26M-scan figure in the section above
  (three times) is the fairer one.

Limits: one Linux machine, not the Windows one used for the rest of this
page; the two large meshes were not run; MeshLab and meshoptimizer each
have settings beyond the two tried; error is a surface-distance measure and
does not see shading, UV stretch or whether a hole is still round.
