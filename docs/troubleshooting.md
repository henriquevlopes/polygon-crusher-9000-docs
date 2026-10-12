# Troubleshooting

## "Native binary not found"

The addon ships with platform-specific simplifier binaries in its `bin/` folder
(`simplify-win-x86_64.exe`, `simplify-darwin-arm64`, `simplify-linux-x86_64`,
`simplify-linux-arm64`). The operator refuses to run if it can't find a binary
matching your platform. If you see this:

- Re-install the addon from the official `.zip` — do not copy files manually
- Check that the binary exists at:
  `<Blender extensions folder>/polygon_crusher_9000/bin/simplify-<your-platform>`
- On macOS / Linux, the executable bit is restored automatically at runtime, but
  if your installer stripped it entirely, re-installing usually fixes it.
- Intel Macs have no bundled binary.

There is no Python fallback.

---

## The result has more triangles than my target

This is the most common question. Read the report in Blender's status bar. It
shows the actual triangle count and usually says why. See [Limits](limits.md)
for the full list. The usual causes:

- **"preserve options kept N tris"** — Borders, Materials, UV Seams or Sharp
  Edges locked too much geometry. Turn off the ones you do not need, raise
  **Sharp Angle**, or raise the target.
- **"the mesh may be broken: try Clean Mesh"** — duplicate or zero-area faces
  blocked the crush. Turn on [Clean Mesh](features/clean-mesh.md) and crush
  again.
- **"it can't be reduced further without breaking the shape"** — Clean Mesh was
  already on. The mesh is at its limit for this shape.
- **Protect Area** with a high Strength on a large area can leave the result above
  the target. Lower the Strength.

---

## Sharp Edges kept almost nothing on my scan

On a scan-like mesh, angle-based Sharp detection is skipped, because surface
noise would make almost every edge sharp. Only edges you marked Sharp are kept.
The report names the objects. Mark the edges you want to keep as Sharp in
Blender, then crush again. See [Limits](limits.md).

---

## The report warns about non-manifold edges

Very low targets on dense CAD meshes can fold hole walls together, leaving
edges shared by three or more faces. Raise the target. See [Limits](limits.md).

---

## Crushed mesh looks faceted

**Keep Normals** was probably off. Turn it on and crush again.

If you baked a **Normal Map**, Keep Normals is greyed out on purpose. The
crushed mesh uses its own smooth shading and the normal map carries the
detail. Make sure the normal map is connected in the crushed mesh's material
and the viewport is in Material Preview or Rendered mode.

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

Polygon Crusher runs the heavy work on a worker thread, so Blender should stay
responsive. If the UI freezes:

- Very large meshes can take real time. Give it a few seconds.
- The progress bar on the **Crush** button updates as each object completes
  — if it's still moving, the run is healthy.

Press **ESC** at any time to cancel.

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

## Last Crush says "(No run yet)"

**Last Crush** (in Advanced) only shows results for the active **crushed** object,
from the current Blender session. If you select the original (or anything
else), it says "No stats for" that object. Click a crushed object in the
Crushed collection to see its timings.

---

## Contact Support

Still having trouble? Email [henriquevlopes@gmail.com](mailto:henriquevlopes@gmail.com)
