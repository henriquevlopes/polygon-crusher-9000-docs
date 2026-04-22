# Troubleshooting

## "Crusher engine not found"

The addon ships with a pre-compiled `crusher_engine.exe` inside the `.zip`.
If you see this error:

- Re-install the addon from the official `.zip` — do not copy files manually
- Check that the file exists at:  
  `<Blender addon folder>/polygon_crusher_9000/bin/crusher_engine.exe`

---

## Crush completes but the mesh looks faceted

**Keep Normals** was probably off. Enable it and crush again.

If normals were transferred but the mesh still looks wrong, the original object may
have had custom split normals that weren't baked. Try applying all modifiers on the
original before crushing.

---

## UVs are missing or distorted after crushing

- Enable **Keep UVs** before crushing
- The original mesh must have at least one UV layer
- Very high reduction levels (90%+) can distort UV seams — use a lower reduction if
  UV quality is important

---

## Blender freezes during crush

The crush step runs asynchronously, so Blender should stay responsive. If it freezes:

- The mesh may be extremely large (10M+ polygons). Give it time.
- Ensure no other heavy operations are running simultaneously.

Press **ESC** to abort at any time. All objects already processed will be removed
and originals restored.

---

## Progress bar doesn't appear

The progress bar is only visible while a crush is actively running. It disappears
when the operation completes. Per-object results are shown in the **Last Crush**
stats panel when you select a crushed object.

---

## Contact Support

Still having trouble? Email [henriquevlopes@gmail.com](mailto:henriquevlopes@gmail.com)
