# Day 3: From Data to Models

Notebook Parts 4 to 6: the ICP loop, a ground-truth check, and real room scans.

## ICP: match, fit, move, check

1. **Match** each source point to its nearest target point; drop pairs beyond `dist_threshold`.
2. **Fit** the best rotation R and shift t with `best_fit_transform()`.
3. **Move** every source point: `src_points = (R @ src_points.T).T + t`.
4. **Check**: stop when the mean error changes less than `tolerance`.

Early matches are partly wrong; each move makes the next matches better.

Gotcha: reassign `src_points` each pass, or the loop never moves.

## Inside `best_fit_transform()`: Kabsch

1. Center both clouds on their centroids.
2. Cross-covariance `H = AA.T @ BB`.
3. SVD: `R = Vt.T @ U.T`. If `det(R) < 0`, flip Vt's last row (a mirror, not a rotation).
4. `t = mean(B) - R @ mean(A)`.

A rigid move `p' = R p + t` never changes distances between points. Undo a rotation with `R.T`.

## Results

| Test | Metric | Result |
| --- | --- | --- |
| Bunny | Mean correspondence error | 0.00214 |
| Bunny | Rotation error | 0.139° |
| Bunny | Translation error | 0.00010 |
| Room scans | Distance from true pose | about 0.9 cm |

## In practice

ICP refines a rough guess; it doesn't find one from scratch. On the partially overlapping room scans, run it coarse-to-fine: 0.3 m threshold, then 0.1 m.

## Resume line

Built an ICP point-cloud registration pipeline in Python and Open3D, validated against ground truth and on noisy room scans.
