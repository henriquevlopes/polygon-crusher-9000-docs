# Examples

## Polygon Crusher vs Blender Decimate

Same source mesh, same target reduction (8M → 450K triangles). Polygon Crusher
runs roughly 11× faster and avoids the artifacts and lost detail produced by
Blender's built-in Decimate modifier.

![Polygon Crusher vs Blender Decimate — 8M to 450K triangles, 10s vs 1:49](assets/PolygonCrusher9000_example01.jpg)
