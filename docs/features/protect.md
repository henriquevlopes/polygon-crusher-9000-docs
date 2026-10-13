# Protect Areas

Some parts of a mesh matter more than the rest: a face, hands, text on a
label. **Protect Area** keeps their detail while the rest gets crushed. It is
its own group inside **Advanced**; **Sculpt** mode opens it.

---

## How to use it

1. Select the object and press **Tab** for Edit Mode.
2. Select the faces, edges or vertices you want to keep.
3. Under **Protect Area**, click **Add**. The selection goes into a vertex
   group named **Protected**, and the field under the buttons shows it.
4. Set **Strength** and **Blur** if the defaults don't suit you.
5. Turn on **Show Heatmap** to check the result in the viewport.
6. Click **Crush**.

To take parts back out, select them in Edit Mode and click **Remove**.
Add and Remove only work in Edit Mode. Until an area is set, the panel
shows a hint: "Edit Mode: select faces, then Add".

### Using your own vertex group

You can also paint a vertex group yourself (Weight Paint mode, or assign
weights in Edit Mode) and pick it in the field. When the active object is a
mesh, the field lists its vertex groups. Higher weights keep more detail.
With a group picked, **Add** puts the selection into that group.

## Strength

| Strength | Effect |
|----------|--------|
| 0% | No protection |
| 50% (default) | Protected areas keep noticeably more detail |
| 100% | Protected areas are nearly frozen |

The slider is not linear. The protection grows faster as you move towards
100%.

## Blur

Protection fades out softly from the marked area instead of stopping at a
hard line. **Blur** sets how far, as a share of the object's size. The
default is **5%**. **0%** makes a hard edge.

## Show Heatmap

Colors the selected objects in the viewport by how strongly each part is
protected, with the Blur fade included: blue is free to crush, red is fully
protected. It updates as you add, remove or change Blur, including in Edit
Mode. It is only a viewport overlay; nothing is added to your scene.

Strength and Blur are greyed out until a Protect Area is set.

---

## Good to know

- The group is read **after** modifiers, so it matches the mesh you see in the
  viewport.
- The group name is matched on every selected object. Objects that do not
  have a group with that name are crushed normally.
- If the group exists but has no weights, the object is crushed without
  protection and Blender shows a warning.
- Protect Area works with all three [target modes](target-modes.md).
- Strong protection on large areas can leave the result above the target
  triangle count. Check the **after** count in the report. See
  [Limits](../limits.md).
