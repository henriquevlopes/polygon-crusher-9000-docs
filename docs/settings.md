# Settings Reference

All settings live in the **Crusher** tab of the 3D Viewport sidebar (press **N**).

---

## Reduction

**Slider** — 1% to 99% — default **50%**

Percentage of triangles to remove. **50%** discards half of the original triangle count.

- **20–40%** — light clean-up; barely noticeable up close
- **50–70%** — background props, secondary assets
- **80–95%** — distant objects, LODs, real-time assets

The panel shows a live **Estimated output** triangle count beneath the slider so
you can see how your reduction setting maps to a final triangle count before
pressing **Crush**.

---

## Aggressiveness

**Slider** — 0.1 to 20.0 — default **1.0**

Controls how aggressively the simplifier raises its error tolerance each iteration.

- **Lower values (0.5–2.0)** — more conservative; better feature preservation
  (eyes, hair, fine creases) at the cost of possibly missing the exact target
  reduction. Recommended for high-detail scans and sculpts.
- **Higher values (5.0–7.0)** — converge faster and hit the target reduction more
  reliably on lower-detail or hard-surface meshes, at some cost to fine detail.

!!! tip
    Start at the default (1.0) for organic / photogrammetry meshes. Bump it up
    only if you're not hitting the requested reduction.

---

## Keep original

**Checkbox** — default **on**

When enabled, the original object is **hidden** (eye icon off) rather than deleted,
so you can compare before/after or recover it later.

When disabled, the original is **permanently deleted** and only the crushed result
remains.

---

## Keep Normals

**Checkbox** — default **on**

Bakes per-loop split normals from the source onto the decimated mesh. The C++
binary samples the original surface with a BVH nearest-face lookup, so the
crushed mesh still shades like the original even when the topology has changed
dramatically.

!!! tip
    Leave this on in almost all cases. Disabling it results in flat, faceted
    shading on the decimated mesh — especially noticeable on smooth-shaded
    scans and sculpts.

---

## Statistics (sub-panel)

Collapsed by default. When opened, shows per-phase timings for the most recent
crush of whichever crushed object is currently selected:

| Phase | What it measures |
|-------|------------------|
| **Extract** | Reading the source mesh into NumPy via `foreach_get` |
| **Write** | Serialising the input mesh for the C++ binary |
| **Crush (C++)** | Subprocess invocation of the native simplifier |
| **Read** | Parsing the binary's output |
| **Build** | Creating the new Blender mesh + object, copying the transform |
| **Bake Normals** | Sampling and applying custom split normals |
| **Total** | Per-object total |

When multiple objects are selected, subprocesses run concurrently with
main-thread I/O, so the summed phase timings will exceed wall-clock — that's
expected and is a good signal that concurrency is doing its job.
