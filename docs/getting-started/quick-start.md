# Quick Start

## The Polygon Crusher Panel

Once the add-on is installed, open the **N-panel → Polygon Crusher 9000**
tab in the 3D Viewport.

The panel goes top to bottom in four steps:

1. **Source** — what kind of model this is.
2. **Output** — **Decimate** (fewer triangles) or **Remesh** (clean quads),
   and how much you want left.
3. **Keep** — **Shading**, **Textures** and, for hard-surface and CAD
   parts, **Sharps**.
4. **Crush** — **Symmetry**, the **Crush** button (with **Revert** beside
   it) and **Import and Crush...**.

Under these, a collapsed **Advanced** section holds everything else. The
[Settings Reference](../settings.md) describes every control.


---

## Basic Workflow

1. **Select** one or more mesh objects in the 3D Viewport.
2. Under **1 Source**, pick what the model is. Leave it on **Auto** if you
   are not sure: after the run the panel says what it took the model for
   ("Last run treated it as: ..."). See [Source Types](../features/source-types.md).
3. Under **2 Output**, pick **Decimate** or **Remesh**, then **Percent**,
   **Count** or **Budget** and set the value. Picking a source also sets a
   sensible starting amount (scans start at 95% removed, CAD parts at 70%).
4. Under **3 Keep**, leave **Shading** on. Switch on **Textures** if the
   model has a texture you want to keep.
5. Click **Crush**.

The add-on will:

- Evaluate each selected object's full modifier stack and work on the resulting mesh.
- Place the result in a **Crushed** collection at the scene root.
- Hide the original (or delete it, depending on **Keep Original**).

The result lands at the source's exact world transform and is named
`<original name>_crushed`.

The status bar shows a report with the triangle count before and after.
Check it. Some options can stop a crush above the target. See [Limits](../limits.md).

While it works, the Crush button becomes a **Cancel** button and a progress
bar. Click **Cancel** (or press **ESC**) to stop.

---

## Choosing a Target

| Mode | Use it when |
|------|-------------|
| **Percent** | You want to remove a share of the triangles from every object. |
| **Count** | Every object needs the same triangle count. |
| **Budget** | The whole selection has one triangle budget. Detail goes where it costs least. |

Under the target control, the panel shows a live readout (`before → after
triangles`, or `tris → quads` in Remesh), so you can see what the setting
means before you click Crush. See [Target Modes](../features/target-modes.md).

---

## Tips

### For 3D Scans and Photogrammetry

Pick **Scan** under Source. It switches on the scan repairs by itself:
cracks and small holes are closed and spikes are removed. Switch on
**Textures** to keep the photo texture, or tick **Bake to Textures** in
Advanced to put the detail into maps. See
[Clean Mesh and Scan Repair](../features/clean-mesh.md) and
[Bake Maps and Auto UVs](../features/bake-maps.md). To get clean quads
instead of triangles, pick **Remesh**.

### For AI-Generated Models (Meshy, Tripo and others)

Pick **AI**. Seams that split the model are welded and holes are closed.

### For CAD Parts

Pick **CAD**. Corners stay crisp, round holes stay round and flat faces stay
flat. **Sharps** is on by default. With **Remesh** you get a clean grid
with quad loops along the hard edges. See
[Hard Surface and Sharps](../features/hard-surface.md).

### For Subdivided High-Poly Hard Surface

Pick **Hard Surface**. Rounded bevels keep their rows and flat areas become big
clean triangles.

### For Characters, Creatures and Sculpts

Pick **Organic**. Pick **Remesh** to get quads, and use
[Quad Density](../features/quad-density.md) to give the face more of them.

### For Clothing

Pick **Cloth**. Folds bend freely and sewn garments are remeshed as one piece.

### Keeping Detail in One Area

Decimate: in Advanced, tick **Protect** and paint the area or select faces
in Edit Mode. See [Protect Areas](../features/protect.md). Remesh: use
**Quad Density** instead.

### Making LODs

Tick **LODs** in Advanced and click **Crush LODs**. See [LODs](../features/lods.md).

### Working with Modifiers

Polygon Crusher reads the **evaluated** mesh, which is whatever the viewport
shows, including booleans, subdivision, and any other modifier on the stack.
The original object and its modifiers are left untouched. The result is a
fresh static mesh with no modifiers.

### Processing Multiple Objects

Select all the objects you want to crush before clicking **Crush**. They run at
the same time, up to one job per CPU core.

### Going Back

Select the result and click the **Revert** button next to Crush. It deletes
the result and brings back the original. This needs **Keep Original** to be
on. See [Keep Original and Revert](../features/revert.md).

---

## After Crushing

After a Decimate, **Last Crush** at the bottom of Advanced shows the
triangle counts, total time and passes for the active crushed object. Click **Details** for the time
of each phase. After a Remesh, a status box under the Keep section says what
the run did (the real quad count and any notes).
