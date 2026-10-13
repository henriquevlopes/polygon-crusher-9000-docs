# Hard Surface Preservation

Organic meshes (scans, sculpts) can be crushed freely. Hard-surface and CAD
parts cannot. Holes must stay round, panel lines must stay straight, and
material borders must stay where they are.

The **Preserve** options keep those lines in place while everything else
gets crushed. They are all off by default.

---

## The options

| Option | What it keeps in place |
|--------|------------------------|
| **Borders** | Open borders: holes and cut edges. They do not shrink or wobble. |
| **Materials** | The borders between materials. |
| **UV Seams** | UV seam lines, so textures do not smear across them. Needs **Keep UVs**. |
| **Sharp Edges** | Edges you marked Sharp, plus any edge sharper than **Sharp Angle**. |

The crushed mesh also gets those sharp edges marked Sharp.

## One-click setup

**Hard Surface** in the **Mode** menu turns on all four options and sets
**Sharp Angle** to 30°. **Auto** picks it by itself on a mesh with flat
faces and sharp edges. See [Modes](modes.md).

You can also switch options on one at a time. For example, turn on only
**Materials** to keep material borders on an organic mesh.

---

## How kept edges behave (2.7.1)

Up to 2.7.0 a kept edge was a hard lock: the crush was not allowed to touch
it at all. That had two costs. On a model with many borders, seams and
creases (an electricity pole made of 103 parts) the crush ran out of edges
it was allowed to collapse and stopped at about half the triangles, no
matter what you asked for. And on a CAD block, the locked rims of the holes
pushed every collapse into the faces next to them, so round holes came out
as pentagons.

Since 2.7.1 a kept edge is a **cost instead of a lock**. The crush spends
its budget everywhere else first, and when it has to touch a kept line it
collapses *along* the crease rather than across it, so the line stays a
line. In practice:

- **Hard Surface always reaches the count you asked for.** The pole crushes
  to 2% with its structure intact; on the CAD block at keep 10% the holes
  stay round and the surface error is half of what it was.
- **Small parts no longer vanish at heavy reductions.** Every connected
  piece keeps a minimum number of triangles until the error budget is
  truly spent. Before, at keep 2% whole beams of the pole disappeared while
  small insulators floated on; now pieces go in order of how little they
  matter, smallest first, and only once nothing else is left to remove.
- **Models with UVs no longer stop short of the target.** With
  **Low Distortion** on (Hard Surface and Scan modes use it), the crush used
  to give up at around 12% above the target because it refused every
  collapse that touched a seam or might fold a UV face. It now tries the
  plain collapses first and only relaxes the UV rules, in stages, once the
  model is truly out of safe ones.

Scans, sculpts and smooth CAD parts crush the same as before (within
measurement noise). See the [Benchmark](../benchmark.md#against-meshoptimizer)
for the numbers.

<!-- TODO screenshot: the CAD block at keep 10% in Hard Surface mode, 2.7.0 vs 2.7.1, wireframe, showing a round hole that was a pentagon -->

---

## Sharp Edges and Sharp Angle

**Sharp Edges** uses two sources:

1. Edges you marked Sharp in Blender.
2. Edges where the two faces meet at more than **Sharp Angle** (default 30°).

Bumps from scan noise do not count as sharp. An edge over the angle only
counts when it sits next to flat faces or bends one way, like a real crease.

On a mesh that reads as a scan (mostly noise, few flat faces), the angle test
is skipped. Only the edges you marked Sharp are kept. The report names the
objects where this happened. See [Limits](../limits.md).

---

## UV Seams

**UV Seams** needs **Keep UVs** to be on. If Keep UVs is off, the UV Seams
button is greyed out.

---

## What a very low target costs

Reaching the count is not free. Every kept edge is a line the crush would
rather not touch, and the more of them there are, the sooner a low target
forces it to. On a low-poly part with many creases, a target of a few
hundred triangles means some of those lines give way: a hole loses a few
sides, a chamfer flattens. The crush picks the lines that change the shape
least, but check the result.

If a line you care about gave way:

- Raise the target.
- Turn off the Preserve options you do not need, so the budget goes to the
  lines that matter.
- Raise **Sharp Angle**, so fewer edges count as sharp.
- Put a [Protect Area](protect.md) on the part that must stay.

See [Limits](../limits.md) for the full list.

---

## Presets

[Presets](../settings.md#presets) remember the four Preserve options and
**Sharp Angle**. Save one preset for hard-surface work and one for organic
meshes.
