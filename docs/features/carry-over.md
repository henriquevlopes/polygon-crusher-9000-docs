# Rigs, Shape Keys and Vertex Colors

The crushed copy keeps what the original carried per vertex. You don't need
to re-rig, re-paint or re-bake anything. This happens on every crush, LOD and
Budget part, with no setting to turn on.

---

## What carries over

- **Vertex groups (skin weights).** Every group of the original, with its
  weights.
- **Armature modifier and parent.** The crushed copy gets the same Armature
  modifier (same rig, same options) and the same parent as the original, so
  it poses, animates and moves with the rig.
- **Shape keys.** Every key with its value, slider range, relative key and
  vertex group mask.
- **Vertex colors.** The active color attribute: scan colors, ZBrush
  polypaint, AI-model colors.

Each new vertex takes its values from the nearest point on the original's
surface, so weights, shapes and colors stay smooth across the crushed mesh.

## Crushing a posed character

You can crush a character in any pose, or with shape keys dialed in. The
add-on reads the original in its **rest shape** (the Armature modifier and
the shape key mix are switched off for a moment and restored right away), so
the crushed copy is made from the rest mesh and its own rig poses it. The
pose is not baked in twice.

On a test character crushed from 18,000 to 3,000 triangles mid-pose, the
crushed copy stayed within 0.1% of the character's height of the original on
average, in every pose tested.

---

## Limits

- **Shape keys need the same vertex list.** If a modifier changes the vertex
  count (Subdivision, Mirror, Remesh, Boolean...), shape keys can't be matched
  and are not carried. The current shape key mix is then crushed into the
  copy as it looks. Weights, the rig and colors still carry over.
- **Vertex colors on huge meshes.** Sources over 3 million triangles skip the
  vertex color step, and the report says so. Bake **Base Color** instead (see
  [Bake Maps and Auto UVs](bake-maps.md)). Rigs and shape keys are still
  carried at any size.
- **Envelope-only rigs.** An Armature modifier that deforms by bone envelopes,
  with no vertex groups on the mesh, is not carried: the crushed copy keeps
  the pose it had at the time of the crush.
- Very low targets merge nearby details, so weights at tight spots (fingers,
  eyelids) follow the coarser mesh. Check the deformation at extreme poses.
