# Troubleshooting

## "Native binary not found"

The addon ships with platform-specific simplifier binaries in its `bin/` folder
(`simplify-win-x86_64.exe`, `simplify-darwin-arm64`, `simplify-linux-x86_64`,
etc.). The operator refuses to run if it can't find a binary matching your
platform. If you see this:

- Re-install the addon from the official `.zip` — do not copy files manually
- Check that the binary exists at:
  `<Blender extensions folder>/polygon_crusher_9000/bin/simplify-<your-platform>`
- On macOS / Linux, the executable bit is auto-restored at runtime, but if your
  installer stripped it entirely, re-installing usually fixes it.

---

## Crushed mesh looks faceted

**Keep Normals** was probably off. Enable it and crush again.

If normals were transferred but the shading still looks wrong, the source may
have non-default custom split normals that aren't sampling cleanly. Try
applying any modifiers that affect normals on the original before crushing.

---

## Crusher ignored my modifiers

It shouldn't — Crusher always reads the evaluated (post-modifier) mesh. If the
result still looks like the un-modified base mesh, check that the modifier is
actually visible in the viewport (the camera/eye toggles in the modifier
properties panel). Anything hidden in the viewport is hidden from Crusher too.

---

## Result didn't hit the requested reduction

The simplifier stops early when it can't collapse any more edges without
violating its error tolerance. To push closer to the target:

- Increase **Aggressiveness** (try 3.0–7.0). The simplifier raises its error
  tolerance more aggressively per iteration and is more likely to hit the
  target on lower-detail meshes.
- Make sure the source actually has enough redundant geometry to remove —
  already-optimal meshes can't be reduced further without visible loss.

---

## Blender freezes during crush

Crusher runs the heavy work on a worker thread, so Blender should stay
responsive. If the UI does freeze:

- Very large meshes (10M+ triangles) can take real time. Give it a few seconds.
- The progress label on the **Crush** button updates as each object completes
  — if it's still moving, the run is healthy.

Press **ESC** at any time to abort.

---

## Where did my crushed object go?

All crushed outputs are linked into a **Crushed** collection at the scene root
(created automatically on the first crush). The original object is either
hidden (default) or deleted, depending on **Keep original**.

---

## Statistics panel says "(No run yet)"

The Statistics sub-panel only shows timings for the currently selected
**crushed** object. If you select the original (or anything else), it will say
"No stats for this object". Click a crushed object in the Crushed collection
to see its timings.

---

## Contact Support

Still having trouble? Email [henriquevlopes@gmail.com](mailto:henriquevlopes@gmail.com)
