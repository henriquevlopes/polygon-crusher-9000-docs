# What's New

Everything that changed since 2.8.0, grouped by theme, newest first. The
current version is **2.13.0**.

---

## Remesh follows the form (2.13.0)

- **Fewer poles.** Edge loops run longer and cleaner: the sculpted statue went
  from 518 poles to 363, the dog and the scanned human have about half as many.
- **Adaptive Density, with a Strength slider.** More quads where the shape is
  busy, fewer on plain areas, for the same count, with nothing to paint. See
  [Adaptive Density](features/remesh.md#adaptive-density).
- **Flow Guides.** Draw where edge loops should run; small loops around an eye
  or a mouth become clean quad rings. See [Flow Guides](features/flow-guides.md).
- **Stronger Quad Density paint.** Painted areas really get smaller quads, and
  thin unpainted parts no longer tear.
- **A closer fit, always on.** The quads are pulled onto the original surface:
  3 to 29% closer.
- **Symmetry seam welded shut** every time.
- **Ears and tails stay whole** at low quad counts.
- **Cleaner tight spots** (no collapsed sliver faces) and **smooth rounded
  edges** on hard-surface models.
- **Decimate from Python scripts** in background Blender now runs.
- **Same speed as 2.12.20** on most models, despite all of the above.

---

## A new panel in four steps (2.9.3 to 2.11.24)

The panel was rebuilt to ask only what matters.

- **Source** (2.11.24) replaces the old Mode menu. Say what the model is
  (Auto, Scan, AI, CAD, Hard Surface, Organic or Cloth) and the right method is
  used, each starting at a sensible amount. See
  [Source Types](features/source-types.md).
- **Decimate and Remesh tabs** (2.11.1) replace the old Quads checkbox. The
  button below follows the tab.
- **Keep: Shading, Textures** (and **Sharps** for hard-surface sources).
  With both Shading and Textures off, you get the shape only, closed and
  ready to 3D print.
- **Everything else is under Advanced**, in four calm boxes (2.9.9): Result,
  Edges, Extra Steps and Last Crush.
- **Own sidebar tab** (2.8.1). The panel has its own **Polygon Crusher 9000**
  tab and remembers whether you left it open.
- **The Mode menu is gone** (2.9.3). UV seams follow Textures, and the Low
  Distortion and Percent preset boxes were removed. Nothing changes by itself
  any more.
- **Red warnings** (2.9.4, 2.11.7): problems show in red, and a red line tells you when
  Keep Original is off.
- **Show Hard Edges works on every model** (2.9.3), not only CAD.
- **Subdivision Cage was removed** (2.9.3). It did not work well enough.

---

## Remesh: clean quads at the count you ask for (2.9.0 to 2.13.0)

Remesh (first called Quadify) rebuilds a model as even quads. It arrived in
2.9.0 and has been improved in nearly every release since. See
[Remesh (Quads)](features/remesh.md).

- **The count is the count** (2.9.0, 2.12.3). It searches for the setting
  that lands on your number and tells you the real count. On models where the
  engine leaves tears to patch, the average miss fell from 8.6% to 2.3%.
- **Quad Density** (2.12.0, 2.12.1). Paint where you want smaller quads
  (**More Detail**) or larger quads (**Less Detail**). The total stays the
  same. See [Quad Density](features/quad-density.md).
- **Symmetry in Remesh.** One half is remeshed and mirrored, so the seam runs
  straight down the middle.
- **Rebuild Surface** (2.12.4): for triangle soups and models whose surfaces
  run through each other. The Thingiverse triangle soup that came back as 2,805
  loose pieces now comes back as one closed, all-quad piece.
- **Sharp edges steer the quads** (2.12.5). On a 5-million-triangle gun body the
  quads follow 98% of the sharp edges (it was 7%).
- **CAD parts get a clean grid** (2.11.4, 2.11.6, 2.12.8, 2.12.18, 2.12.19):
  flat faces stay flat, every hole is a closed round loop, quad loops run
  along the hard edges, and the quads on flat faces are evened out (badly
  skewed quads around holes fell from about 3 to 8% to about 1%). Auto no
  longer needs a switch, and Hard Surface tries the grid first. Very low counts
  get a coarser all-quad grid.
- **Models of many parts keep every part** (2.11.14, 2.11.15, 2.12.6, 2.12.9).
  Big parts are remeshed one by one, and small parts are simplified and paired
  into quads, so nothing vanishes. On the game weapon: all parts kept, 89%
  quads, near the asked count, no broken edges, and faster. The 1,862-part
  electric pole kit loses nothing.
- **Sewn garments** are sewn together and remeshed in one run (2.12.7). The
  denim jacket comes back in 8 pieces instead of 32.
- **Broken 3D-print models** (2.11.16 to 2.11.21): faces pointing both ways
  are turned to agree, zero-area flat plates are kept, thin walls and bars stay
  closed at low counts, and models of surfaces running through each other are
  rebuilt first. A chair that came back in shreds is now clean, in half the
  time.
- **UVs, materials, vertex colors and baking.** Remesh carries UVs and
  materials and bakes by itself when seams would crack (2.9.x to 2.11.x), and
  keeps vertex colors (2.10.1).
- **Much faster.** A 1-million-triangle sculpt went from 12 minutes to 8
  seconds, a 9-million-triangle scan from 13 minutes to about 1.5 minutes, and
  CAD parts take 2 to 4 seconds (2.11.0). Scans are 30% faster again in
  2.12.9.
- **Progress and no freezes** (2.11.8, 2.11.22). The button becomes a progress
  bar (Reading, Preparing, Remeshing, Building, Shading, Baking) and Blender
  keeps redrawing. A stuck engine is stopped.
- **It repairs damage** (2.9.2): if a remesh tears a closed model, it tries
  again with more quads. It also tells torn models apart from shattered thin
  parts, and gives the right advice for each.
- **Works on Windows, Linux and Intel Macs** (2.9.1, 2.9.2). The result after a
  Remesh is selected and active (2.11.7).

---

## Keeping hard edges: Sharps (2.12.16 to 2.12.20)

- **Sharps** (2.12.16): a toggle in Keep for Hard Surface and CAD. Hard edges,
  including corners modelled with a small bevel, stay in place and crisp after
  Decimate or Remesh. Rounded fillets stay smooth. See
  [Hard Surface and Sharps](features/hard-surface.md).
- **Remesh with Sharps** (2.12.17): small round holes come out round, not
  pinched into stars, and nothing folds.
- **Heavily bevelled models** (2.12.20): when bevels cover most of a low-poly
  model (the game weapon), they ease off so the result still follows the shape.
- **Hard Surface decimates with a better method** (2.11.25): rounded bevels
  keep their rows, flat areas become big clean triangles, and a 5-million-triangle
  gun body takes half the time.
- **Show Hard Edges** (2.9.3): see the kept edges in the viewport before you
  crush. Cyan for marked Sharp, yellow for angle edges, purple for material
  borders, red for UV seams, green for open borders. See
  [Show Hard Edges](features/show-hard-edges.md).

---

## Scans, cracks and holes (2.11.9 to 2.12.20)

- **Fill Holes closes the cracks in scans** (2.11.10 to 2.11.13). Thin cracks
  between scan patches are zipped shut, with no visible crack lines. A noisy rock
  scan comes out closed except for its outer ground edge. Surfaces are no longer
  glued together, and seams are not capped as if they were holes.
- **Flat openings of any size are capped** (2.9.3), such as the end of a pipe.
- **Pick Floor** (2.11.9): click a point on the model to set where **Cut Floor**
  cuts. A blue plane follows the mouse. See
  [Clean Mesh and Scan Repair](features/clean-mesh.md).
- **Scans keep their shading and fine detail** (2.12.20). Before, Fill Holes
  could scramble the shading, and fine detail could flatten. Scans also stop
  wasting triangles on texture-atlas seams.
- **Scan and AI sources** switch on the repairs by themselves (2.11.24) and are
  10 to 40% faster than Auto.
- **Rocks no longer tear in Remesh** (2.11.15).

---

## Cloth (2.11.15 to 2.12.20)

- **Folds are no longer stiff** (2.12.20). On soft cloth (the goose puffer) the
  shape error is less than half of before.
- **Thick cloth lands on the count you ask for** (2.12.20): 50,000 asked gave
  58,700, now 50,100, with no dark patches.
- **Denim-style garments** remesh cleanly, and dark shading flips on cloth are
  fixed (2.11.15).

---

## Textures and baking (2.9.x to 2.11.x)

- **Undo Bake** takes a bake back off an object: the old materials return and
  the baked images are deleted. See [Bake Maps and Auto UVs](features/bake-maps.md).
- **Bake Into Kept UVs** for Remesh.
- **No more black textures from missing image files** (2.10.1): a material
  whose image can't be found is left as it is.
- **Crush with textures no longer turns poles into spikes** (2.11.15), when a
  model's UVs are cut into many tiny islands.

---

## Shading and quality of Decimate (2.8.2 to 2.12.20)

- **Every test model is as good as or better than 2.8.0** (2.12.20): checked on 16
  models, whole and close up, with textures.
- **No more streaks or cracks on CAD** (2.8.2). Thin triangles around holes no
  longer flip over or pick up the hole wall's shading.
- **No stray Sharp edges** (2.11.2, 2.11.3): the Sharp edges on the result are the
  real hard edges, and shading matches the original exactly.
- **Crush no longer glues surfaces together** (2.11.12).
- **No more light teeth on low cylinders** (2.10.0).
- **Models with faces pointing both ways** keep clean shading (2.11.19).
- **Zero-area triangles no longer tear rims** (2.11.16).

---

## Speed and big models (2.9.6 to 2.11.22)

- **One fast Crush method** (2.10.0): up to a third faster on big meshes (a
  9-million-triangle scan: 42 s to 28 s). The old Fast Crush checkbox is gone
  because it is always on.
- **Big meshes no longer freeze at 99%** (2.11.x), and the slow preparation steps
  run in small pieces so the progress bar keeps moving (2.11.22).
- **Cancel**: the Crush button becomes **Cancel** while it runs. Esc still
  works.
- **Selection stays tidy**: after a Remesh the new mesh is selected.

---

## Platforms (2.8.3 to 2.9.2)

- **Intel Macs work** (2.8.3 for Decimate, 2.9.1 for Remesh).
- **Linux** gets the quad engine (2.9.2).
- The Windows quad engine ships in the current build.

---

## Retired

- **Mode menu** (Auto, Scan, Sculpt, Hard Surface, Custom): replaced by Source.
- **Percent presets** and saved presets.
- **Low Distortion, UV Seams and Preserve checkboxes**: now automatic.
- **Fast Crush checkbox**: always on.
- **Subdivision Cage.**
- **Quadify options** (Size by Edge Length, Follow Selected Edges, Mirror, Cloth
  Panels, Show Lines, Auto Creases, Best Quality, Detail Density, Smoothing,
  Retry, CAD Model): Remesh does these by itself now.
- **Hard Surface Preservation checkboxes** (Borders, Materials, Sharp Edges):
  now automatic, with **Sharps** and **Hard Edge Angle** for control.
