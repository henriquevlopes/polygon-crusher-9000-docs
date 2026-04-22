# Settings Reference

## Lossless Mode

**Toggle** — default off

Removes only redundant geometry — faces that are co-planar or near-co-planar — with
zero visible change to the mesh shape. Great for cleaning up over-subdivided meshes
or imported CAD/STEP files. Works best on hard-surface models.

When enabled, the **Reduction** slider is ignored.

---

## Reduction

**Slider** — 1% to 99% — default 80%

Controls how aggressively the mesh is simplified.

- **80%** removes most polygons — ideal for background objects or LODs
- **20%** is a light pass — subtle reduction, barely noticeable up close

The panel shows both the current triangle count and the estimated result count as you
adjust the slider.

---

## Keep Normals

**Checkbox** — default on

Transfers the original surface normals onto the decimated mesh using Blender's
Data Transfer modifier (`POLYINTERP_LNORPROJ` mapping). This preserves smooth
shading even at very high reduction levels.

!!! tip
    Leave this on in almost all cases. Disabling it results in flat, faceted shading
    on the decimated mesh.

---

## Keep UVs

**Checkbox** — default off

Re-projects the original UV map onto the crushed mesh using Blender's Data Transfer
modifier. Useful when the decimated mesh needs to receive a texture.

!!! note
    UV transfer works on both standard and lossless crushed meshes. Quality depends
    on how much the mesh topology changed — moderate reductions give the best results.

---

## Keep Original Object

**Checkbox** — default on

When enabled, the original mesh is **hidden** rather than deleted, so you can
compare before/after or recover it later.

When disabled, the original is **permanently deleted** and the crushed result takes
its name.

!!! warning
    With **Keep Original** off, pressing ESC to abort cannot restore originals that
    were already processed in that session.
