# Quadify

**Quadify** rebuilds a mesh from scratch as clean **quads** (four-sided
faces), at the quad count you ask for. It is a separate panel under the main
Polygon Crusher 9000 panel, with its own **Quadify** button. Use it when you
want tidy, all-quad topology to keep editing, sculpting or subdividing.

It works best on scans, sculpts and organic shapes. For mechanical parts with
hard edges, use **Crush** instead (with [Flat Panels](../settings.md#flat-panels)
if you want fewer, bigger faces): a quad remesh rounds off hard edges.

Quadify makes a new object called `<name>_quad`. The original is hidden, and
**Revert** brings it back, as long as **Keep Original** is on.

---

## How to use it

1. Select one or more meshes.
2. Open the **Quadify** panel and set **Quads**.
3. Click **Quadify**.

The status bar reports what happened, for example the real number of quads,
how long it took, and the extra steps described below. All the settings are
in the [Settings Reference](../settings.md#quadify).

---

## Thick cloth

Some models are cloth with real thickness: a jacket or a flag made of two
layers, an outside and an inside, very close together. When the two layers
are closer than about half a quad, a normal remesh cannot tell them apart and
tears the model.

Quadify spots this by itself. It remeshes **one** layer, then rebuilds the
thickness around it. The result stays closed and is still all quads. You do
not need to switch anything on.

The report says: *thick cloth: one side remeshed, thickness X rebuilt*.

---

## Small tears are closed

A remesh sometimes leaves small holes that were not in your model. Quadify
patches them with quads.

The model's **own** openings stay open: the neck of a shirt, cuffs, the edge
of a sheet, gaps in a scan. Only small tears that appeared during the remesh
are closed.

The report says: *closed N small tears*. Because the patches add faces, the
final face count can come out a few percent above the number you asked for.

---

## Automatic flow choice

With **Auto Creases** on (the default), Quadify does not pick one way to
flow the quads and hope. It tries three and keeps the one that stays closest
to your original:

- **Creases off**: the quads ignore sharp edges.
- **Surface-smoothed flow**: the quads follow the smoothed surface.
- **Creases 90°**: edges sharper than 90° are kept as creases.

Results that leave holes are scored worse, so a clean one wins. The report
tells you which one it used: *surface-smoothed flow*, *creases off* or
*creases 90°*.

Trying three ways costs a little extra time. If you would rather choose
yourself, turn **Auto Creases** off and set **Crease Angle** by hand.
