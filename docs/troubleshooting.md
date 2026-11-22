# Troubleshooting

## "Native binary not found"

The addon ships with platform-specific simplifier binaries in its `bin/` folder
(`simplify-win-x86_64.exe`, `simplify-darwin-arm64`, `simplify-darwin-x86_64`,
`simplify-linux-x86_64`, `simplify-linux-arm64`). The operator refuses to run if it can't find a binary
matching your platform. If you see this:

- Re-install the addon from the official `.zip` — do not copy files manually
- Check that the binary exists at:
  `<Blender extensions folder>/polygon_crusher_9000/bin/simplify-<your-platform>`
- On macOS / Linux, the executable bit is restored automatically at runtime, but
  if your installer stripped it entirely, re-installing usually fixes it.

There is no Python fallback.

---

## The result has more triangles than my target

This is the most common question. Read the report in Blender's status bar. It
shows the actual triangle count and usually says why. See [Limits](limits.md)
for the full list. The usual causes:

- **"the mesh may be broken: try Clean Mesh"** — duplicate or zero-area faces
  blocked the crush. Turn on [Clean Mesh](features/clean-mesh.md) and crush
  again.
- **"it can't be reduced further without breaking the shape"** — Clean Mesh was
  already on. The mesh is at its limit for this shape.
- **Protect** with a high Strength on a large area can leave the result above
  the target. Lower the Strength.

---

## A hole came out as a pentagon, or a crease flattened

The target was too low for the number of edges you asked to keep. The crush
reaches the count you asked for even on CAD and Hard Surface, so at a very
low count some kept lines have to give way. It picks the ones that change the
shape least. Raise the target, raise **Hard Edge Angle** (Advanced → Edges),
or put a [Protect Area](features/protect.md) on the part that must stay. See
[Hard Surface Preservation](features/hard-surface.md#what-a-very-low-target-costs).

---

## Small parts disappeared

At a heavy reduction the budget runs out and whole pieces go. Since 2.7.1
every separate piece keeps a minimum number of triangles until nothing else
is left to remove, and then the smallest pieces go first; before, pieces
went in whatever order they happened to collapse. Raise the target, or
protect the pieces that must stay. If pieces are missing right after
**Clean Mesh**, lower **Small Pieces**: it deletes loose bits on
purpose. See [Limits](limits.md#small-pieces-at-heavy-reductions).

---

## Hard edges kept almost nothing on my scan

On a scan-like mesh, and with the Scan, Organic or Cloth source, angle-based
detection is skipped, because surface noise would make almost every edge
sharp. Only edges you marked Sharp are kept. Mark the edges you want to keep
as Sharp in Blender, then crush again. Show Hard Edges shows what is kept. See
[Limits](limits.md).

---

## The report warns about non-manifold edges

Very low targets on dense CAD meshes can fold hole walls together, leaving
edges shared by three or more faces. Raise the target. See [Limits](limits.md).

---

## Crushed mesh looks faceted

**Shading** (under Keep) was probably off. Turn it on and crush again.

If you baked a **Normal Map**, the crushed mesh uses its own smooth shading
and the normal map carries the detail. Make sure the normal map is connected
in the crushed mesh's material and the viewport is in Material Preview or
Rendered mode.

---

## My baked maps do not show

- Workbench cannot show textures or normal maps. Crush switches a Workbench
  scene to EEVEE for you and tells you in the report. If you changed it back,
  switch the viewport to Material Preview.
- **Base Color** comes from the image textures feeding Base Color in the
  original's materials, or from vertex colors. If the report says an object
  has "no image texture or vertex colors", no base color was baked for it. A
  texture reported as "not loaded" is missing on disk: fix its path on the
  original and crush again. See [Bake Maps and Auto UVs](features/bake-maps.md).
- Use **Undo Bake** (Advanced → Bake to Textures) to remove a bake and try again.
- Look in the folder you chose under **Save To**. For an unsaved .blend, the
  maps are packed into the file instead of saved as PNG files.

---

## Crusher ignored my modifiers

It shouldn't — Polygon Crusher always reads the evaluated (post-modifier) mesh.
If the result still looks like the un-modified base mesh, check that the
modifier is actually visible in the viewport (the camera/eye toggles in the
modifier properties panel). Anything hidden in the viewport is hidden from
Polygon Crusher too.

---

## Blender freezes during crush

Polygon Crusher runs the heavy work in a separate process, so Blender should
stay responsive. If the UI freezes:

- Very large meshes can take real time. Give it a few seconds.
- The progress bar under the **Crush** button updates as the run goes: if
  it is still moving, the run is healthy.

Click **Cancel** (the Crush button while it runs) or press **ESC** to stop.

---

## Where did my crushed object go?

All crushed outputs are linked into a **Crushed** collection at the scene root
(created automatically on the first crush). LOD chains go into a
`<name>_LODs` collection inside it. The original object is either hidden
(default) or deleted, depending on **Keep Original**.

---

## The Revert button is greyed out

Revert needs a selected crushed object whose original still exists. It is
greyed out when:

- the crushed object is not selected (the hidden original is not enough),
- the crush was made with **Keep Original** off, or
- you deleted the original.

It is also greyed out while a crush is running. See
[Keep Original and Revert](features/revert.md).

---

## Last Crush says there are no stats

**Last Crush** (in Advanced) only shows Decimate results for the active **crushed** object,
from the current Blender session. If you select the original (or anything
else), it says "No stats for" that object. Click a crushed object in the
Crushed collection to see its timings.

---

## Remesh says the quad engine is not available

The quad engine is bundled for Windows x64, Linux x64 and macOS (Apple
Silicon and Intel). On Linux ARM64 only Decimate works. On the other systems,
re-install the add-on from the official `.zip`: the panel names the missing
file.

---

## Remesh: the result is not what I expected

- The status box under Keep says the real count and what happened. If it had
  to spend more quads to avoid damage, it says that too.
- A CAD part with many holes needs a minimum number of quads for a clean grid.
  The status says how many.
- Models that are not one clean surface (triangle soups, self-crossing
  sheets) may need **Rebuild Surface** (under Keep, Remesh only).
- Hard edges on a low-poly model with many bevels come out crisper with
  Decimate. See [Remesh (Quads)](features/remesh.md).

---

## Contact Support

Still having trouble? Email [henriquevlopes@gmail.com](mailto:henriquevlopes@gmail.com)
