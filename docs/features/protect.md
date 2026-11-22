# Protect Areas

Some parts of a mesh matter more than the rest: a face, hands, text on a
label. **Protect** keeps their detail while the rest gets crushed. It is in
**Advanced → Extra Steps**, and it only shows for **Decimate**. For
**Remesh**, use [Quad Density](quad-density.md) instead.

---

## How to use it

In **Advanced → Extra Steps**, tick **Protect**. Then either paint it or
select it.

**Paint it:**

1. Click **Paint Protect**. Blender opens Weight Paint on the protect group.
2. Paint **red** where the detail must stay.
3. Click **Done Painting**.

**Or select it:**

1. Select the object and press **Tab** for Edit Mode.
2. Select the faces, edges or vertices you want to keep.
3. Click **Add**. The selection goes into the protect vertex group, and the
   field under the buttons shows it. **Remove** takes selected parts back out.
   Add and Remove only show in Edit Mode.

Then set **Strength** and **Blur** if the defaults don't suit you, turn on
**Show Heatmap** to check the result in the viewport, and click **Crush**.

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

Strength and Blur show once a protect group is set.

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
