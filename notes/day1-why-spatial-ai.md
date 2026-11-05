# Day 1: Why Spatial AI

No code today. The focus was what changes when a computer has to understand the 3D world instead of a flat image.

## Key ideas

- Regular computer vision reads 2D pixels. Spatial AI reads depth, shape and geometry: the layer under self-driving perception, AR/VR, robotics and 3D scanning.
- An image is a grid of numbers with no depth. A 1080p photo is a 1080 × 1920 × 3 array.
- A pinhole camera projects a 3D point by dividing by depth: `u = f·X/Z + cx`, `v = f·Y/Z + cy`. Depth is lost.
- A depth camera measures Z per pixel, so back-projection (`X = (u − cx)·Z/f`) turns its image into a point cloud.
- Cameras, depth cameras, LiDAR and photogrammetry all end as the same thing: an `(N, 3)` array of points.

## Why point clouds are hard

1. Unordered: shuffling rows changes nothing.
2. Uneven density: near surfaces are dense, far ones sparse.
3. Noisy and incomplete.
4. Each scan has its own coordinate frame. Aligning them is registration.

## Five core operations

Load → Downsample (voxel grid) → Normals → Neighbors (KD-tree) → Align (ICP)
