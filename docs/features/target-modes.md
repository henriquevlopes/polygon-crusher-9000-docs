# Target Modes

The **Target Mode** buttons in the **Target** box, under the Mode menu,
choose how you ask for a triangle count. There are three modes: **Percent**, **Count** and **Budget**.

Triangle counts are measured on the evaluated mesh. That is the mesh you see
in the viewport, with all modifiers applied.

---

## Percent

Removes a share of the triangles. Set **Reduction** from 1% to 99%. The
default is 50%.

| Reduction | Typical use |
|-----------|-------------|
| 20–40% | Light clean-up |
| 50–70% | Background props, secondary assets |
| 80–95% | Distant objects, real-time assets |

Each selected object is reduced by the same percentage. A 100,000-triangle
object and a 10,000-triangle object, both at 80%, end up with 20,000 and
2,000 triangles.

**Presets** are available in this mode only. See
[Presets](../settings.md#presets).

## Count

Crushes **each** selected object to **Target Triangles** (default 5000).

If an object already has fewer triangles than the target, its copy keeps the
original's triangle count. If that is true for every selected object, the
report says the copies were left unchanged.

Use this when every object needs the same triangle count, for example a
set of props that all share one budget per object.

## Budget

Sets **one triangle total for the whole selection** (**Total Triangles**,
default 100,000).

Count mode gives every object the same count. Percent mode gives every object
the same share. Budget does neither. It removes detail where it costs the
least, across all selected objects together. Big or detailed objects keep
more triangles. Simple objects are cut harder.

Objects never merge into each other. Each one comes back as its own crushed
copy.

If the budget is so low that an object would get no triangles at all, that
object is skipped and left as it is. The report names it.

!!! note
    Budget works on the selection as a whole. If you select only one object,
    the budget applies to that object alone.

---

## What happens to the result

In every mode:

- The crushed copy goes into a **Crushed** collection at the scene root. The
  collection is created on the first crush.
- The copy has the same world transform as the original and the name
  `<original name>_crushed`.
- The copy keeps smooth shading if the original had any smooth faces.
- The report in Blender's status bar shows the triangle count before and
  after. Check the **after** number. Some settings stop a crush above the
  target. See [Limits](../limits.md).

Make LODs overrides the target mode. See [LODs](lods.md).
