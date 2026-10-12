# Hard Surface Preservation

Organic meshes (scans, sculpts) can be crushed freely. Hard-surface and CAD
parts cannot. Holes must stay round, panel lines must stay straight, and
material borders must stay where they are.

The **Preserve** options keep those lines exactly in place while everything
else gets crushed. They are all off by default.

---

## The options

| Option | What it keeps in place |
|--------|------------------------|
| **Borders** | Open borders: holes and cut edges. They do not shrink or wobble. |
| **Materials** | The borders between materials. |
| **UV Seams** | UV seam lines, so textures do not smear across them. Needs **Keep UVs**. |
| **Sharp Edges** | Edges you marked Sharp, plus any edge sharper than **Sharp Angle**. |

Corner points of preserved edges never move. The crushed mesh also gets
those sharp edges marked Sharp.

## One-click setup

**Hard Surface** in the **Mode** menu turns on all four options and sets
**Sharp Angle** to 30°. **Auto** picks it by itself on a mesh with flat
faces and sharp edges. See [Modes](modes.md).

You can also switch options on one at a time. For example, turn on only
**Materials** to keep material borders on an organic mesh.

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

## Protecting more means fewer triangles removed

Every preserved edge is a line the crush cannot touch. The more you preserve,
the fewer triangles can be removed. On low-poly parts with many creases, the
result can stop above your target. The report then says that the preserve
options kept N triangles and shows the target, so you can see how far off it
is.

If that happens:

- Turn off the options you do not need.
- Raise **Sharp Angle**, so fewer edges count as sharp.
- Raise the target.

See [Limits](../limits.md) for the full list.

---

## Presets

[Presets](../settings.md#presets) remember the four Preserve options and
**Sharp Angle**. Save one preset for hard-surface work and one for organic
meshes.
