# Modes

Which switches suit a model is not obvious: a scan wants Clean Mesh and
the scan repairs, a CAD part wants its edges kept in place, a sculpt wants
neither. The **Mode** menu at the top of the panel picks them for you.

---

## Auto

**Auto** is the default. When a mesh is selected, Auto asks the engine for
a one-second look at it and shows what it sees under the menu, in plain
words:

> Looks like a scan: rough surface all over
> Found: 120 spikes, 11 small holes

It then applies the matching mode (Scan, Sculpt or Hard Surface) and, for
a scan, switches on only the repairs it actually found: **Cut Floor** when
there is a table or ground layer under the model, **Remove Spikes** when
there are spikes or stretched faces, **Fill Holes** when there are small
holes. **Clean Mesh** comes on when the mesh is split along its seams or
full of collapsed faces, in any mode.

How the engine decides:

- **Hard surface**: many flat faces meeting at sharp edges, or the long
  thin triangles of a CAD export.
- **Scan**: a rough surface everywhere (the faces around a typical point
  tilt by 8° or more) and almost no flat patches.
- **Sculpt**: everything else, which is to say a smooth surface.

Auto looks again whenever the selected mesh's triangle count changes, and
only in Object Mode. With several meshes selected it looks at the active
one and applies the result to the whole crush.

Meshes over 600,000 triangles do not get looked at by themselves (it would
take a few seconds on every selection). A **Detect** button appears
instead; a crush in Auto mode always looks first anyway.

Auto's guess can be wrong on a mesh between two kinds (a smoothed
hard-surface model, a very clean scan). Pick the mode yourself in that
case.

## Scan

For 3D scans and photogrammetry. Clean Mesh on, with Remove Spikes and
Fill Holes (and Cut Floor when a table was found), Keep Normals, Keep UVs
with Low Distortion when the mesh has UVs. Nothing is kept in place:
scan noise has no edges worth keeping. The **Clean Mesh** and **Preserve**
groups open in Advanced.

## Sculpt

For sculpts and smooth organic models. Keep Normals and Keep UVs (when
there are UVs), nothing else forced. The **Protect Area** and **Bake**
groups open, because a face to protect and a normal map to keep the
detail are what a sculpt usually needs.

## Hard Surface

For CAD and mechanical models. **Borders**, **Materials**, **UV Seams**
(when there are UVs) and **Sharp Edges** at **30°** are kept in place,
with Keep Normals and Keep UVs (when there are UVs). Since 2.7.1 the kept
edges are a cost rather than a lock, so this mode reaches the count you
asked for. See [Hard Surface Preservation](hard-surface.md). The
**Preserve** group opens.

## Custom

Your own settings; every Advanced group opens. Changing any value a mode
controls (Keep Normals, Keep UVs, Low Distortion, the four Preserve
options, Sharp Angle, Clean Mesh and the Scan Repair switches) turns the
menu into **Custom (from Scan)**, and a **Reset to Scan defaults** button
under it brings the mode's values back.

Protect Area, Bake, LODs, the amount and Keep Original are never touched
by a mode.

---

## Import and Crush

The file browser has the same **Mode** menu. In **Auto**, the engine
reads the file once to look at it and once to crush it, so the import
takes about twice as long as a fixed mode; the report says which mode it
picked and what it found. See [Import and Crush](import-crush.md).
