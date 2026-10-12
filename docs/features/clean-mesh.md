# Clean Mesh

AI-generated and scanned meshes often have broken geometry. When you crush
them, they fall apart into fragments or stop far above the target. **Clean
Mesh** repairs the mesh before the crush.

Turn it on with the **Clean Mesh** checkbox in the **Cleanup** section.

---

## What it does

- Merges duplicate vertices.
- Removes broken faces (zero-area faces) and duplicate faces.
- Fixes flipped faces.
- Deletes small floating pieces (floaters).

## Remove Small Pieces

The **Remove Small Pieces** slider sets how small a loose piece has to be to
get deleted. It is a share of the mesh's triangles.

- Default: **0.1%**. Range: 0% to 5%.
- **0%** keeps every piece.
- Greyed out unless Clean Mesh is on.

Raise it when a scan has lots of floating bits. Keep it low for models that
really are made of separate parts, like a set of small screws.

## Runs on its own when a mesh needs it

You don't always have to switch it on. Clean Mesh runs by itself on an
object (and the report says so) when:

- **2% or more of its vertices are duplicates.** Scans from glTF, FBX and OBJ
  are often split along UV seams; crushing them without welding opens cracks
  along the seams.
- **2% or more of its triangles have no area.** This is typical of an OBJ
  saved with too few decimals, or a scan snapped to a coarse grid. Without
  cleaning, the crush would stop with "Unable to reduce mesh".

Welded meshes (most CAD parts and sculpts) score 0% and are left alone.
Turn the checkbox on yourself for other broken meshes, for example flipped
faces or floating debris.

---

## Scan Repair

Scans bring their own kind of damage that welding does not touch. Under
**Clean Mesh** there are three switches for it. They need Clean Mesh on,
work with a crush from the panel and with
[Import and Crush](import-crush.md), and the report says what they did.

### Cut Floor

The table, turntable or ground captured under the model is removed. With
**Height** at **0%** the floor is found by itself: it is the dense, flat
layer at the very bottom of the model. If the scan has no such layer,
nothing is cut and the report says *no floor found*. Set **Height** to cut
at a share of the model's height instead (everything below that goes).

A ground scan (a patch of terrain) has a dense flat top of its own. That
is not mistaken for a floor: only the lowest dense layer counts.

### Remove Spikes

Two fixes in one switch:

- A point shot far out of the surface (farther than twice the size of its
  neighbour ring) is pulled back to the centre of that ring.
- A long stretched face, with an edge more than ten times the typical edge
  of the mesh, is deleted. Scanners make these where two surfaces got
  bridged. The small hole left behind is closed by Fill Holes.

### Fill Holes

Small missing patches are closed with a fan of faces around a new centre
point. **Size** is the longest outline a hole may have to be filled, as a
share of the model's size (default 5%). Bigger openings stay open: the
bottom of a bust, the back of a relief, the edge of a ground scan. The
report counts the holes filled and the openings left open.

### Order

Floor, spikes and stretched faces are handled first, then small floating
pieces are removed, then holes are filled, then the winding is fixed so the
new faces point the right way. The crush runs on the repaired mesh.

---

## Only the copy changes

Clean Mesh only changes the crushed copy. Your original is never repaired or
edited.

When something was fixed, the report in the status bar lists it, for example
how many vertices were merged and how many bad faces were removed.

---

## When to use it

- A crush ends far above the target and the report says *the mesh may be
  broken: try Clean Mesh*.
- The crushed result has holes, spikes or fragments that the original does not.
- The mesh comes from an AI model generator or a photogrammetry scan.

If you already have **Clean Mesh** on and the report still says the mesh stays
above the target, the mesh cannot be reduced further without breaking its
shape. See [Limits](../limits.md).
