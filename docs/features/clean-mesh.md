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
