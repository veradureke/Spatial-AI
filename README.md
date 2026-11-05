# PointFusion3D: 3D Point Cloud Registration

A Python + Open3D pipeline that takes two partial, noisy 3D scans of the same object and fuses them into one aligned model using Iterative Closest Point (ICP). It checks itself against a known ground-truth transform and runs unchanged on real, partially overlapping room scans.

Built during **Spatial AI Foundations** (Reboot), a four-session intro to 3D computer vision taught by Dwayne Jones.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/veradureke/Spatial-AI/blob/main/notebooks/PointFusion3D.ipynb)

<!-- Add your before/after ICP plot here, e.g. ![Before and after ICP](assets/icp_before_after.png) -->

## What it does

| Step | Function | What I implemented |
| --- | --- | --- |
| Clean | `remove_outliers()` | KD-tree statistical outlier filter: drop points whose mean k-NN distance is above mean + 2 std |
| Match | `find_correspondences()` | Nearest-neighbor correspondence search with Open3D's `KDTreeFlann` |
| Align | `icp()` | Match, reject far pairs, fit R and t (Kabsch/SVD), move, repeat until the error stops shrinking |
| Validate | Parts 5 and 6 | Graded against a known transform, then coarse-to-fine ICP on real room scans |

## Results

| Test | Metric | Result |
| --- | --- | --- |
| Stanford Bunny | Final mean correspondence error | 0.00214 |
| Stanford Bunny | Rotation error vs. ground truth | 0.139° |
| Stanford Bunny | Translation error vs. ground truth | 0.00010 |
| Open3D room scans | Average distance from true pose | about 0.9 cm |

## Run it

Click **Open in Colab** above and run the cells top to bottom. The setup cell installs Open3D and pulls the scan data from the course starter repo, [Dwaynekj/3d-portfolio-starter](https://github.com/Dwaynekj/3d-portfolio-starter).

## Repo layout

```
notebooks/PointFusion3D.ipynb   the full pipeline, Parts 1 to 9 plus homework
notes/                          my learning notes from each session
```

## Learning notes

- [Day 1: Why Spatial AI](notes/day1-why-spatial-ai.md)
- [Day 2: Seeing in 3D](notes/day2-seeing-in-3d.md)
- [Day 3: From Data to Models](notes/day3-from-data-to-models.md)

## Status

- [x] Day 1: field overview, pixels to point clouds
- [x] Day 2: outlier removal and nearest-neighbor search
- [x] Day 3: ICP loop, ground-truth check, room-scan validation
- [ ] Homework: sample a point cloud from the Elmo phone scan mesh
- [ ] Day 4: Gaussian splat seed and three.js demo

## Tech

Python · NumPy · Open3D · Google Colab
