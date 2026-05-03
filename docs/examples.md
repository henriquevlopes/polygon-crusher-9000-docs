# Examples

## High-poly sculpt — 8M → 450K triangles

A photogrammetry-grade scan crushed to about 6% of its original triangle count.
Polygon Crusher runs roughly 11× faster than Blender's built-in Decimate and
avoids the artifacts and lost detail visible in the Decimate result.

![Polygon Crusher vs Blender Decimate — 8M to 450K triangles, 10s vs 1m49s](assets/PolygonCrusher9000_example01.jpg)

---

## Hard-surface mesh — 70K → 2K triangles

A 97% reduction on a hard-surface mesh. Polygon Crusher preserves the silhouette
and curvature of the original shape on a tight triangle budget, where Decimate
distorts the form.

![Polygon Crusher vs Blender Decimate — 70K to 2K triangles on a hard-surface mesh](assets/PolygonCrusher9000_example02.jpg)
