# Limits

Polygon Crusher 9000 does not always reach the target. This page lists what
can stop it, and what the report tells you.

After every crush, Blender's status bar shows a report: triangles before and
after, time taken, and any warnings. **Always check the "after" count.**

---

## The result can stop above the target

The crush aims for your target, but some things stop it early:

- **Protect Area.** Strong protection on large areas can leave the result
  above the target.
- **Broken geometry.** Duplicate or zero-area faces block collapses. Clean
  Mesh runs by itself on meshes with many duplicate vertices or zero-area
  triangles. Otherwise the report says the mesh may be broken and suggests
  [Clean Mesh](features/clean-mesh.md). If Clean Mesh is already on, it says
  the mesh cannot be reduced further without breaking its shape.

The report only raises this warning when the result is clearly above the
target (more than 20% over), so small differences do not show up as warnings.

Kept edges (hard edges, material borders, UV seams) do not stop a crush
above the target: a kept edge is a cost, not a lock. What a very low target
costs on such a model is shape, not triangles: see
[What a very low target costs](features/hard-surface.md#what-a-very-low-target-costs).

## Small pieces at heavy reductions

A model made of many separate pieces (a pole with its insulators, a set of
screws) loses whole pieces when the target is a small fraction of its
triangles. Since 2.7.1 every connected piece keeps a minimum number of
triangles until the error budget is truly spent, and then pieces go in order
of how little they matter, smallest first. At an extreme target pieces still
disappear; raise the target, or put a [Protect Area](features/protect.md) on
the ones that must stay. **Small Pieces** under Clean Mesh is a
different thing: it deletes loose bits before the crush, on purpose.

## Objects can be skipped

An object is left untouched, and named in the report, when it:

- has no geometry after modifiers,
- has fewer than 4 triangles, or
- gets no triangles in a [Budget](features/target-modes.md#budget) crush.

If every selected object is already at or below the target, the copies are left
unchanged and the report says so.

---

## Scans and noisy meshes: hard edges

On a scan-like mesh, almost every edge has a small bend because of surface
noise. Treating all of them as sharp would lock the whole mesh.

So the **Scan**, **Organic** and **Cloth** sources do not keep angle-sharp
edges, and on a model that reads as a scan under another source,
angle-based detection is skipped as well. Only the edges you marked Sharp in
Blender are kept. Show Hard Edges says "Looks like a scan: only marked Sharp
kept".

If you want specific edges kept on a scan, mark them Sharp yourself first.

## Sharps

- **Sharps** is skipped on sources over 1 million triangles.
- On a low-poly model where bevel edges cover most of the surface (a game
  weapon), the bevels ease off so the shape is still followed. For crisp hard
  edges there, use Decimate rather than Remesh.
- A hard-surface source falls back to the usual method (not the
  HiPoly-style one) with Textures on, several materials, Symmetry, protected
  areas, keeping under 5%, or low-poly kits it cannot bring down to the
  target. See [Source Types](features/source-types.md).

## Very low targets on dense CAD

Very low targets on dense CAD meshes, a few hundred triangles, can fold the
walls of holes together. This leaves edges that are shared by three or more
faces (non-manifold edges).

The report warns when a crushed copy has more of these edges than its source.
If that matters for your export or for baking, raise the target.

---

## Remesh

- Remesh makes quads (and, on models of many loose parts, some triangles
  joined into quads). It lands close to the count you ask for, usually within
  a few percent. A CAD part with many holes can need more quads than you
  asked for (a block with many holes needs at least about 1,240); the status
  line says so.
- A quad remesh rounds off fine detail finer than a quad. On low-poly hard
  surface with many bevels, Decimate keeps hard edges crisper.
- Thin walls and bars at low counts can tear. Models that are not one clean
  surface (triangle soups, self-crossing sheets) can fail or come out as
  loose pieces. Switch on **Rebuild Surface** for those: it rebuilds a closed
  skin, but loses detail finer than its grid and your Sharp marks.
- Remesh does not use LODs, Clean Mesh, Protect, Import and Crush or the
  flat-area options.
- The quad engine's result differs slightly between processor types (Apple
  Silicon vs Intel). The same computer always gives the same mesh.
- On Linux ARM64 there is no quad engine, so only Decimate is available.
- Quad Density: a part smaller than a big quad loses its shape under **Less
  Detail**.

## Baking

- **Base Color** is baked from the image textures feeding each material's
  Base Color, or from vertex colors when a material has none. Colors made by
  procedural nodes (Noise, Gradient, a plain color value) are not baked. An
  object with neither textures nor vertex colors is skipped for Base Color,
  and the report says so.
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

## Textures (Keep UVs)

**Textures** is off by default. Turn it on only when the result must keep its
UV layout. UV transfer quality depends on how much the topology changed.
Moderate reductions give the cleanest results. UV seams are held in place and
collapses that would fold a face's texture mapping are refused. The crush
relaxes those rules in stages once it runs out of safe collapses, so the
target is reached; the last few percent of a very low target can then show
some stretch near seams.

## Revert

Revert needs the original. It only works when the crush was made with **Keep
Original** on, or with LODs. See
[Keep Original and Revert](features/revert.md).

---

## Import and Crush

- Reads **.obj**, **.ply** (ascii and binary) and **.stl** (ascii and binary).
  Other formats (glTF, FBX, USD) have to go through Blender's own importer.
- Normals in the file are ignored; the result is shaded smooth. Shading,
  Protect, Bake and LODs do not apply to an imported file.
- A texture is only connected when the image is next to the model and
  **Keep UVs (and textures)** is on. Textures other than the base color (`map_Kd`) are not
  read.
- Faces with more than four corners are split as fans. A concave n-gon can
  come out wrong; scans do not have those.
- Memory: the engine needs about 1 GB of RAM per 3 million triangles in
  the file. A 32 million triangle OBJ (2.5 GB on disk) peaked at 10 GB and
  crushed to 200,000 triangles in 72 seconds. On a 16 GB machine, stay under
  about 40 million triangles.

## Platform

- **Blender 4.2 or later.** 4.2 is the lowest version the extension accepts
  and has not been tested; the add-on is tested on Blender 5.2.
- **Windows x64, Linux x64, Linux ARM64, macOS Apple Silicon and macOS
  Intel** have a bundled Decimate engine. Remesh (the quad engine) is bundled
  for Windows x64, Linux x64 and macOS (Apple Silicon and Intel), not for
  Linux ARM64.
- The add-on needs its native binary. There is no Python fallback. If the
  binary is missing, the panel shows "Native binary not found" and Crush does
  not run.
