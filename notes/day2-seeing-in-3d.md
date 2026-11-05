# Day 2: Seeing in 3D

Notebook Parts 1 to 3: load a point cloud, clean it, and match points across scans.

## Part 1: a point cloud is an array

The Stanford Bunny scan loads as an `(N, 3)` NumPy array. No mesh, no faces, only surface samples.

## Part 2: `remove_outliers()`

Real scans have four kinds of mess: different viewpoints, noise, partial coverage and outliers. A real surface point has neighbors nearby; an outlier sits alone.

1. KD-tree search for k + 1 neighbors per point.
2. Skip result 0 (the point itself).
3. `np.sqrt` the squared distances, then average.
4. Keep points below `global_mean + 2 * global_std`.

Gotcha: Open3D returns squared distances. Skip the sqrt and the threshold is wrong.

## NumPy moves used everywhere

`axis=0` for centroids · broadcasting `(N, 3) - (3,)` · boolean masks · index arrays · `rng.choice(..., p=...)`. A per-point `(N,)` weight needs `[:, None]` before multiplying an `(N, 3)` array.

## Part 3: `find_correspondences()`

Each source point's match is its nearest neighbor in the target cloud. Brute force is O(n²); a KD-tree makes each lookup about O(log n).

Why k = 1 here but k + 1 in Part 2? Part 3 searches the other cloud, so a point never finds itself.

## Resume line

Implemented KD-tree outlier removal and nearest-neighbor correspondence search for 3D point clouds in Python and Open3D.
