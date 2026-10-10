# Keep Original and Revert

Two controls decide what happens to your original mesh: the **Keep Original**
checkbox and the **Revert** button.

---

## Keep Original

**Checkbox** — default **on**

- **On:** after the crush, the original is **hidden** (eye icon off). It stays
  in the scene, so you can compare, or come back to it later.
- **Off:** the original is **deleted**. Only the crushed copy remains.

The checkbox is greyed out while [Make LODs](lods.md) is on, because LODs
always keep the original.

!!! warning
    With Keep Original off, there is nothing to go back to. Revert needs the
    original. Right after the crush, Blender's Undo (Ctrl+Z) can still take it back.

## Revert

**Button** — the small icon next to **Crush**

Revert deletes the **selected crushed object** and brings back its original:
the original is un-hidden and selected.

- Works on crushed copies and on LOD objects.
- Select the crushed object first, not the hidden original.
- You can select several crushed objects and revert them together.
- Greyed out when none of the selected objects has an original to go back to,
  and while a crush is running.
- Only available for crushes made with **Keep Original** on (or with Make
  LODs).

When it is done, Blender's status bar says how many objects were reverted.

### Where the link is kept

Each crushed object remembers its original, and this is saved in the .blend
file. Revert still works after you save and reopen. If you delete the original
yourself, the link is cleared and Revert is greyed out for that object.
