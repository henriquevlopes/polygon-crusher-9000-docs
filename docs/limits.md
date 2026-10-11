# Limits

Polygon Crusher 9000 does not always reach the target. This page lists what
can stop it, and what the report tells you.

After every crush, Blender's status bar shows a report: triangles before and
after, time taken, and any warnings. **Always check the "after" count.**

---

## The result can stop above the target

The crush aims for your target, but some things stop it early:

- **Preserve options.** Borders, Materials, UV Seams and Sharp Edges lock
  geometry in place. If they lock a lot, the crush cannot remove enough
  triangles. The report says the preserve options kept N triangles and shows
  the target.
- **Low-poly CAD parts.** Every crease corner stays fixed when Sharp Edges is
  on. A part with many creases can stop above a very low target.
- **Protect Area.** Strong protection on large areas can leave the result
  above the target.
- **Broken geometry.** Duplicate or zero-area faces block collapses. Clean
  Mesh runs by itself on meshes with many duplicate vertices or zero-area
  triangles. Otherwise, with no preserve options on, the report says the mesh may be broken and suggests
  [Clean Mesh](features/clean-mesh.md). If Clean Mesh is already on, it says
  the mesh cannot be reduced further without breaking its shape.

The report only raises this warning when the result is clearly above the
target (more than 20% over), so small differences do not show up as warnings.

## Objects can be skipped

An object is left untouched, and named in the report, when it:

- has no geometry after modifiers,
- has fewer than 4 triangles, or
- gets no triangles in a [Budget](features/target-modes.md#budget) crush.

If every selected object is already at or below the target, the copies are left
unchanged and the report says so.

---

## Scans and noisy meshes: Sharp Edges

On a scan-like mesh, almost every edge has a small bend because of surface
noise. Treating all of them as sharp would lock the whole mesh.

So on meshes that read as scans (mostly noise, few flat faces), **angle-based
Sharp detection is skipped**. Only the edges you marked Sharp in Blender are
kept. The report names the objects where this happened.

If you want specific edges kept on a scan, mark them Sharp yourself first.

## Very low targets on dense CAD

Very low targets on dense CAD meshes, a few hundred triangles, can fold the
walls of holes together. This leaves edges that are shared by three or more
faces (non-manifold edges).

The report warns when a crushed copy has more of these edges than its source.
If that matters for your export or for baking, raise the target.

---

## Baking

- **Base Color** is baked from **vertex colors** only. An object with no vertex
  colors is skipped for Base Color. An object whose color comes from image
  textures only gets the normal map. The report says when objects have no
  vertex colors.
- A bake needs UVs on the crushed mesh. Meshes without UVs are unwrapped
  automatically. See [Bake Maps and Auto UVs](features/bake-maps.md).
- The baked maps are connected in copies of the materials. If an input was
  already connected there, the old link is replaced.
- The bake happens after the crush. A failed bake is listed in the report as
  a note and the crushed mesh is kept.

## Rigs, shape keys and vertex colors

Skin weights, the Armature modifier, shape keys and vertex colors carry over
to the crushed copy, with a few limits: shape keys are dropped when a
modifier changes the vertex count, and vertex colors are skipped on sources
over 3 million triangles. See
[Rigs, Shape Keys and Vertex Colors](features/carry-over.md).

## Keep UVs

Keep UVs is off by default. Turn it on only when the crushed mesh must keep
its UV layout. UV transfer quality depends on how much the topology changed. Moderate
reductions give the cleanest results. Very heavy reductions can stretch UVs
near seams. Turn on **UV Seams** to hold the seam lines in place.

## Revert

Revert needs the original. It only works when the crush was made with **Keep
Original** on, or with Make LODs. See
[Keep Original and Revert](features/revert.md).

---

## Platform

- **Blender 4.2 or later.**
- **Windows x64, Linux x64, Linux ARM64 and macOS Apple Silicon** have bundled
  native binaries. Intel Macs have no bundled binary.
- The add-on needs its native binary. There is no Python fallback. If the
  binary is missing, the panel shows "Native binary not found" and Crush does
  not run.
