# Source Types

Step 1 of the panel asks what kind of model you have. The right method for
that kind is used, and each kind starts at a sensible amount.

There used to be a **Mode** menu (Auto, Scan, Sculpt, Hard Surface, Custom).
It was removed in 2.9.3: it only ticked checkboxes, sometimes behind your
back. **Source** replaced it in 2.11.24 and works differently: it picks the
*method*, and what it switches on is shown to you.

---

## The seven sources

| Source | Use it for | Starting amount (Percent) |
|--------|------------|---------------------------|
| **Auto** | Not sure. The model is looked at and the method picked for you. | unchanged |
| **Scan** | 3D scans and photogrammetry: noise, cracks, holes. | 95% removed |
| **AI** | AI-generated models (Meshy, Tripo and others): split seams, baked texture atlas. | 90% removed |
| **CAD** | CAD parts (STEP exports): crisp corners, long thin triangles, exact holes. | 70% removed |
| **Hard Surface** | Subdivided high-poly hard surface: rounded bevels instead of crisp corners. | 75% removed |
| **Organic** | Characters, creatures and sculpts: smooth, no hard edges. | 90% removed |
| **Cloth** | Garments made of sewn panels. | 85% removed |

You can change the amount afterwards. The starting amount is set only when you
pick the source.

**CAD** and **Hard Surface** are separate on purpose. CAD means crisp
corners and exact round holes. Hard Surface means a subdivided high poly
with rounded bevels.

## Auto

Auto looks at each model and picks the method by itself. After a run, the
panel shows a line under the buttons: **Last run treated it as: ...**, so you
can see what it decided. If it guessed wrong, pick the source yourself.

## What each source does

- **Scan** and **AI** switch on **Clean Mesh** and **Fill Holes** by
  themselves (and **Remove Spikes** for Scan). They switch them off again when
  you change the source. Scans and AI models are also faster than Auto,
  because the model no longer has to be analysed first. See
  [Clean Mesh and Scan Repair](clean-mesh.md).
- **CAD** and **Hard Surface** show the **Sharps** toggle under Keep and keep
  hard edges in place. See [Hard Surface and Sharps](hard-surface.md).
  A Hard Surface source is crushed with the method made for subdivided hard
  surface: rounded bevels keep their rows, flat areas become big clean
  triangles, and the points stay exactly on the original surface. It steps
  aside by itself, and the usual method runs, when it does not fit: with
  **Textures** on, several materials, **Symmetry**, protected areas, keeping
  under 5%, or low-poly models it cannot bring down to the target.
- **Organic**, **Scan** and **Cloth** do not keep angle-sharp edges: on those
  models a sharp-looking edge is noise or a fold. (Edges you marked Sharp
  yourself still count.) The **Hard Edge Angle** setting is hidden for them.
- **Cloth** lets folds bend freely, so soft garments keep their shape.
- **CAD** and **Hard Surface** with **Remesh** try a clean grid first. See
  [Remesh (Quads)](remesh.md).

Some settings only show for the sources they apply to. **Tidy Flat Areas**
and **Flat Areas as Quads** show for Auto, CAD and Hard Surface. **Cut
Floor** and **Remove Spikes** show for Auto, Scan and AI.
