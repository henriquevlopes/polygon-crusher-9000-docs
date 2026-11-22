# Hard Surface and Sharps

Organic meshes (scans, sculpts) can be crushed freely. Hard-surface and CAD
parts cannot. Holes must stay round, panel lines must stay straight, and
material borders must stay where they are.

---

## What is kept automatically

With a source of **Auto**, **CAD** or **Hard Surface**, these are kept in
place by default, and the result still reaches the count you asked for:

- Edges sharper than the **Hard Edge Angle** (default 30°) and edges you
  marked Sharp.
- Borders between materials (not in Shape only mode, see below).
- Open borders (holes and cut edges).
- UV seams, when **Textures** is on.

A kept edge is a cost, not a lock. The crush spends its budget everywhere
else first, and when it has to touch a kept line it collapses *along* the
crease rather than across it, so the line stays a line.

**Organic**, **Scan** and **Cloth** sources do not keep angle-sharp edges,
because on those models they are noise or folds. Edges you marked Sharp are
still kept.

Use **Show Hard Edges** (Advanced) to see the kept edges before you crush. See
[Show Hard Edges](show-hard-edges.md).

---

## Sharps

**Sharps** is a toggle in the **Keep** row. It shows for **Hard Surface** and
**CAD** sources, and is on by default (since 2.12.16).

With Sharps on, hard edges stay in place and stay crisp after **Decimate**
or **Remesh**, including corners modelled with a small bevel (a game
weapon's triangle bumps, long grooves and panel lines no longer go soft).

- **Decimate** holds those edges harder while reducing, then marks the
  result's real creases Sharp and shades each side flat.
- **Remesh** gets one quad loop per hard edge. Small round holes come out
  round instead of pinched into stars, and nothing folds.
- Rounded parts and fillets that should look smooth are left smooth.
- When bevel edges cover most of a low-poly model (the game weapon), they
  ease off, so the result still follows the shape.
- It is skipped on sources over 1 million triangles (too slow there).

Turn **Sharps** off if you want the plain method.

---

## Shape only

If you switch **Shading** and **Textures** both off, you get the shape only:
a closed mesh ready to 3D print. The panel says "Shape only: closed, ready to
print". Clean Mesh and Fill Holes (with a big hole size) switch on by
themselves, and material borders are not held. They go back as they were when
you switch Shading or Textures on again.

---

## What a very low target costs

Reaching the count is not free. Every kept edge is a line the crush would
rather not touch, and the more of them there are, the sooner a low target
forces it to. On a low-poly part with many creases, a target of a few
hundred triangles means some of those lines give way: a hole loses a few
sides, a chamfer flattens. The crush picks the lines that change the shape
least, but check the result.

If a line you care about gave way:

- Raise the target.
- Raise **Hard Edge Angle**, so fewer edges count as sharp.
- Put a [Protect Area](protect.md) on the part that must stay.

See [Limits](../limits.md) for the full list.

---

## Decimate of CAD and subdivided models

- **CAD** source: crisp corners and long thin triangles are handled, so round
  holes stay round and flat faces stay flat, with no streaks coming out of
  hole rims.
- **Hard Surface** source (a subdivided high poly): crushed with a method that
  keeps the rows of rounded bevels and puts big clean triangles on flat
  areas. A 5-million-triangle gun body takes half the time it used to. See
  [Source Types](source-types.md).
- **Flat Areas as Quads** and **Tidy Flat Areas** (Advanced → Edges) clean up
  flat areas of a Decimate result. **Flat Areas as Quads** merges the
  triangles of each flat area into one face and pairs the rest into quads.
  **Tidy Flat Areas** flips long thin triangles into even ones without moving
  a point. They show for Auto, CAD and Hard Surface, in Decimate only.
