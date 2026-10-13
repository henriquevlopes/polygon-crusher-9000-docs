# Bake Maps and Auto UVs

Crush can bake the original's detail onto the low-poly copy as texture maps.
It connects the maps in the copy's materials, so the result looks right
straight away.

The **Bake** group in **Advanced** has two toggle buttons (**Sculpt** mode
opens it):

- **Normal Map** — bakes the original's surface detail.
- **Base Color** — bakes the original's color: its image textures, or its vertex colors.

You can turn on one or both. Then click **Crush** as usual. After the crush,
the progress bar shows "Baking" while the maps are made.

---

## Normal Map

Bakes the original's surface detail into a normal map on the crushed mesh's
UVs.

- The crushed mesh uses its **own smooth shading** instead of the
  [Keep Normals](../settings.md#keep-normals) transfer. This is what game
  engines expect. Keep Normals is greyed out while Normal Map is on.
- **Normal Y** picks the green channel convention. **OpenGL** (default) is for
  Blender, Unity and Godot. **DirectX** is for Unreal. A DirectX map still
  looks correct in Blender.
- The normal map is saved as Non-Color data.

## Base Color

Bakes the original's color into a base color map on the crushed mesh's
UVs, so a textured scan crushed to a fraction of its triangles still looks
like the scan.

- **Image textures first.** For each material, the image feeding **Base
  Color** (directly, or through color nodes such as Mix or Hue/Saturation)
  is sampled through the original's own UVs. Materials with several
  textures are handled one by one, so a multi-material object gets one map
  with all of them in place.
- **Vertex colors otherwise.** A material with no image uses the object's
  active color attribute instead. Scans and ZBrush polypaint keep their
  color this way.
- Objects with neither are skipped, and the report says "no image texture
  or vertex colors". A texture whose file is missing is reported as "not
  loaded" and skipped.

---

## Auto UVs

Baking needs UVs on the crushed mesh. Most scans and AI models have none, so
Crush makes them.

- **When:** the original has no UVs, or **Keep UVs** is off.
- **How:** the mesh is split into a few large, projection-based islands. Each
  island is flattened, given equal texel density, and packed with a margin
  that fits the map size.
- **Ask first:** when an original has no UVs, Crush shows a dialog first:
  "No UVs to bake onto" with a **Auto UV and Crush** button. The dialog lists
  the objects that have no UVs. If Keep UVs is off, the crushed copy is
  unwrapped without a dialog.
- The report lists how many UV charts were made for each object.

When baking, the borders of the UV islands are marked as seams and Sharp
on the crushed mesh. This splits the smooth shading along the seams, so the
normal map bakes without visible seams.

If the original already has UVs and **Keep UVs** is on, those UVs are used for
the bake.

### Overlapping UVs

A bake writes each pixel of the map once. If two faces share the same UV
space (stacked or mirrored islands, common in game assets), they fight over
those pixels and the map is wrong there. With Keep UVs on, Crush checks for
this and the report says so: it names the objects and how much of their map
overlaps, and suggests turning **Keep UVs** off so the crushed copy is baked
onto new UVs.

---

## Where the maps go

**Save To** has two choices:

- **Blend File Folder** (default) — maps are saved as PNG files in the same
  folder as the .blend file. If the file is unsaved, the maps are packed into
  the .blend instead. The panel shows "Unsaved: maps get packed".
- **Custom Folder** — maps are saved as PNG files in the folder you pick. A
  path that starts with `//` is relative to the .blend file.

Files are named `<object name>_normal.png` and `<object name>_basecolor.png`.

**Size** sets the map resolution: 512, 1K, 2K (default) or 4K.

## How the maps are connected

- Each crushed mesh gets **its own copies** of its materials, named
  `<object name>_<material name>`. Your original materials are not changed.
- If the mesh has no material, Crush makes one.
- The normal map goes through a Normal Map node into the Principled BSDF
  **Normal** input. The base color map goes into **Base Color**.
- If an input was already connected in the copy, the old link is replaced.
  The report says how many inputs were replaced.

## Viewport

Workbench cannot show textures or normal maps. If the render engine is
Workbench, Crush switches the scene to **EEVEE** and puts Solid-mode 3D views
into Material Preview. The report says when it did this.

---

See also [Limits](../limits.md) for what baking cannot do.
