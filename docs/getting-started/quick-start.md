# Quick Start

## Basic Workflow

1. **Select** one or more mesh objects in the 3D Viewport
2. Open the **Polygon Crusher** panel (N-panel → Polygon Crusher tab)
3. Set your desired **Reduction** percentage
4. Click **CRUSH IT**

The addon will:

- Export the mesh, run the FQMS engine, and re-import the result
- Transfer the original surface normals onto the decimated mesh
- Place the result in a **Crushed** collection
- Hide the original (or delete it, depending on your settings)

---

## Choosing a Reduction Amount

| Reduction | Use Case |
|-----------|----------|
| **20–40%** | Light clean-up; barely noticeable up close |
| **50–70%** | Background props, secondary assets |
| **80–95%** | Distant objects, LODs, real-time assets |

---

## Tips

### For 3D Scans and Photogrammetry

Standard mode (FQMS) works well for both hard-surface and organic/scan geometry.
Enable **Keep Normals** to preserve the smooth shading of the original scan.

### For Imported CAD or Subdivided Models

Try **Lossless Mode** first. It removes redundant geometry — faces that are co-planar
or near-co-planar — without any visible change to the shape. This can dramatically
reduce polygon count on imported STEP/CAD meshes with no quality loss.

### Processing Multiple Objects

Select all the objects you want to crush before clicking **CRUSH IT**.  
They will all be processed in sequence and placed in the **Crushed** collection.

Press **ESC** at any time to abort — objects already crushed in the batch are
removed and originals are restored.

---

## After Crushing

The **Last Crush** stats panel shows triangle counts and processing time for whichever
crushed object is currently selected.
