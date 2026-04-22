# How It Works

## The Standard Pipeline

When you click **CRUSH IT** (without Lossless mode), the addon runs this pipeline:

```
Original mesh
     │
     ▼
Export to OBJ  ──────────────────────────────┐
     │                                        │
     ▼                                        │
crusher_engine.exe (FQMS)                    │ original mesh
     │                                        │ stays in Blender
     ▼                                        │
Import result OBJ                            │
     │                                        │
     ▼                                        │
Transfer normals  ◄──────────────────────────┘
     │            (Data Transfer modifier,
     ▼              POLYINTERP_LNORPROJ)
Transfer UVs (optional)
     │
     ▼
Crushed result in "Crushed" collection
```

Because the operation runs via `subprocess.Popen`, Blender stays fully responsive
during the crush step. The progress bar updates in real time.

---

## FQMS — Fast Quadric Mesh Simplification

The crusher engine uses the **Fast Quadric Mesh Simplification** algorithm, which
works by iteratively collapsing the edge that introduces the least geometric error
(measured as a quadric error metric) until the target polygon count is reached.

Key properties:

- **Topology-aware** — preserves sharp features and boundaries
- **Deterministic** — same input always produces the same output
- **Fast** — processes million-polygon meshes in seconds

---

## Normal Transfer

After decimation, the result mesh has auto-computed normals that don't match the
original's smooth shading. The addon applies a **Data Transfer** modifier from the
original to the result, using `POLYINTERP_LNORPROJ` (project along polygon normal)
mapping to copy the original split normals onto every loop of the decimated mesh.

This is the same workflow you would use manually in Blender — the addon just
automates it.

---

## Lossless Mode

Lossless mode uses Blender's built-in **Decimate (Dissolve)** modifier with a very
small angle threshold (0.1°). This dissolves edges between co-planar faces without
moving any geometry. The result is visually identical to the original.

Because it operates entirely inside Blender, lossless mode requires no file I/O and
is typically very fast.
