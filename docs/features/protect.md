# Protect Areas

Some parts of a mesh matter more than the rest: a face, hands, text on a
label. **Protect** keeps their detail while the rest gets crushed.

---

## How to use it

1. In Blender, paint the areas you want to keep into a **vertex group**. Use
   Weight Paint mode, or assign vertices in Edit mode.
2. In the **Preserve** section of the panel, pick that group in the
   **Protect** field. When the active object is a mesh, the field lists its
   vertex groups.
3. Set **Strength**. The default is 50%.
4. Click **Crush**.

Higher weights in the group keep more detail.

## Strength

| Strength | Effect |
|----------|--------|
| 0% | No protection |
| 50% (default) | Painted areas keep noticeably more detail |
| 100% | Painted areas are nearly frozen |

The slider is not linear. The protection grows faster as you move towards
100%.

The Strength slider is greyed out until a Protect group is set.

---

## Good to know

- The group is read **after** modifiers, so it matches the mesh you see in the
  viewport.
- The group name is matched on every selected object. Objects that do not
  have a group with that name are crushed normally.
- If the group exists but has no weights, the object is crushed without
  protection and Blender shows a warning.
- Protect works with all three [target modes](target-modes.md).
- Strong protection on large areas can leave the result above the target
  triangle count. Check the **after** count in the report. See
  [Limits](../limits.md).
