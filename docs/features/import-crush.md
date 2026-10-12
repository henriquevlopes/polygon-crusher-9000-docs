# Import and Crush

Some scans are too big to open. A 500 MB OBJ from a photogrammetry app, a
PLY with ten million faces: Blender's importer takes minutes, uses many
gigabytes of memory, or fails. **Import and Crush** skips that step. The
engine reads the file itself, crushes it, and only the crushed result is
built in Blender.

Find it as the **Import and Crush...** button under **Crush** in the panel, or
in **File > Import > Polygon Crusher: Import and Crush**.

---

## How to use it

1. Click **Import and Crush...** and pick an **.obj**, **.ply** or **.stl** file.
2. In the file browser's side panel, set the target (Percent or Count, same
   as in the panel), **Keep UVs**, **Clean Mesh** with its Scan Repair
   switches and, for OBJ files, whether the file is Y up.
3. Click **Import and Crush**. The progress bar under the panel shows
   "Reading" while the file loads and the crush percentage after that.

The result appears in the **Crushed** collection, named after the file,
shaded smooth and selected. The report in the status bar says how many
triangles the file had, how many the result has, and how long it took.

A 530 MB OBJ with 8.8 million triangles reads in about a second and crushes to
100,000 triangles in about eight seconds on a desktop PC.

## What comes along

| In the file | On the result |
|---|---|
| OBJ `vt` texture coordinates, PLY per-vertex or per-face texture coordinates | The UV map (with **Keep UVs** on) |
| OBJ materials (`usemtl`) and the `.mtl` file | One material per name, faces assigned as in the file |
| `map_Kd` in the `.mtl`, or a PLY `TextureFile` comment | The texture, connected to Base Color, when the image is next to the file and **Keep UVs** is on |
| `Kd` colour in the `.mtl` | The material's Base Color, when there is no texture |
| PLY vertex colours, OBJ `v x y z r g b` colours | A **Col** vertex colour layer, connected to Base Color |
| Quads and n-gons | Triangulated |

Normals in the file are not used: the result is shaded smooth. Faces with
more than four corners are split as fans, so a concave n-gon can come out
wrong. Scans do not have those.

## Settings that apply

From the panel: the **Target**, **Keep UVs**, **Clean Mesh** with **Remove
Small Pieces** and the three **Scan Repair** switches, and the four
**Hard Surface** options (Borders, Materials,
UV Seams and Sharp Edges with its angle). **Keep Normals**, **Protect Area**,
**Bake** and **LODs** do not apply, because there is no original mesh in
Blender to read them from. Crush the result again from the panel if you
need a bake or LODs.

## Clean Mesh runs by itself when needed

Most exported scans are split along their UV seams (every seam vertex is
stored twice). Crushed as they are, they crack open along the seams. The
engine checks the file and, when 2% or more of the vertices sit on another
vertex, or 2% or more of the faces have no area, it runs
[Clean Mesh](clean-mesh.md) first. The report says so: "Clean Mesh ran by
itself (file is split along its seams)".

## Textures

The texture file has to be next to the model, as the `.mtl` or `.ply`
names it. The add-on also looks for the file by its base name, ignoring
case. When it is not found, the material is made without it and the report
lists the missing names. With **Keep UVs** off there are no UVs to show a
texture on, so the report asks you to turn it on.

!!! tip
    An OBJ from most scanning apps and from Blender's own exporter is Y up.
    Leave **OBJ is Y up** on, and the model lands the way Blender's own
    importer would place it. For an OBJ that was written Z up, turn it off.
