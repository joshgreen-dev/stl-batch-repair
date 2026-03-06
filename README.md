# stl-batch-repair

Small Python script that batch-runs manifold repair on a folder of STL files using
`trimesh`. Useful when you get a pile of files from a client and half of them fail to slice.

---

## The problem

Clients send STL files. Clients' STL files are often broken — non-manifold edges, flipped normals,
gaps. Running each one through Meshmixer manually is fine for 2 files, not for 20.

---

## Requirements

```
pip install trimesh numpy
```

Optional for better repair:
```
pip install manifold3d
```

---

## Usage

```bash
python repair.py ./input_folder ./output_folder
```

Processes every `.stl` in the input folder, attempts repair, writes fixed files to output.
Prints a summary of which files were clean, which were repaired, which failed.

---

## What it does

1. Load STL via trimesh
2. Check is_watertight
3. If not: fill holes, merge vertices, fix winding
4. If still broken after repair: flag it in the summary (manual review needed)
5. Write output

---

## Limitations

Not magic. Extremely broken geometry (inverted meshes, overlapping shells) needs manual work.
This handles the common cases — gaps, loose verts, small holes.
