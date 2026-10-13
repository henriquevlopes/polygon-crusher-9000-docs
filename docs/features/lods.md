# LODs

Tick **Make LODs** and Crush builds a full LOD chain in one click.

---

## How to use it

1. Select the object.
2. In the **LODs** group of **Advanced**, tick **Make LODs**.
3. Set **Levels** (1 to 8, default 3).
4. Set **Ratio** (5% to 95%, default 50%).
5. Click **Crush LODs**. The main button changes its name while Make LODs is
   on.

Each level keeps **Ratio** of the previous level's triangles. With the default
50% ratio and three levels, the chain is:

| Level | Triangles kept |
|-------|----------------|
| LOD0 | 100% (the original) |
| LOD1 | 50% |
| LOD2 | 25% |
| LOD3 | 12.5% |

When an object is active, the panel lists the exact triangle count of each
level under the Levels and Ratio controls. A level never has fewer than 4
triangles.

---

## What you get

- Objects named `<name>_LOD1`, `<name>_LOD2` and so on.
- They go in a collection named `<name>_LODs`, inside the **Crushed**
  collection.
- The original stays as **LOD0**. It is not hidden and not deleted.
- Every level is crushed from the **original**, not from the level before it.
  This gives each level the best surface for its triangle count.

## What changes while Make LODs is on

- The Target section (Percent, Count, Budget) is greyed out. LODs use their
  own Ratio.
- **Keep Original** is greyed out. LODs always keep the original.
- The triangle readout is hidden. Use the LOD list instead.
- Budget mode is not used. Each object gets its own chain.

The Preserve, Protect Area and Clean Mesh settings still apply to every level.

## Revert

**Revert** works on LODs. Select an LOD object and click Revert to delete it.
See [Keep Original and Revert](revert.md).

---

## Limits

- The lowest levels of a hard-surface chain are where kept edges give way
  and small pieces go. Check them. See [Limits](../limits.md).
