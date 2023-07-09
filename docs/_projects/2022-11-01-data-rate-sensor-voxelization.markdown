---
layout: single
title: "Data Rate Voxelization"
date: 2022-11-01 12:00:00 -0700
categories: project CST pointcloud
excerpt_separator: <!--more-->
---

## Motivation

The program was created because we wanted to see if we could quantify the amount
of data that an autonomous vehicle would require in a particular lattitude and
longitude.

## Overview

In this project, I was tasked with analysing the data rate required to process
data from a sensor as an autonomous vehicle moves through an environment. To
do this, we analyzed dense LiDAR point clouds and simulated what is observed
by a sensor at a particular point.
Points on the road following the trajectory of the LiDAR scanning vehicle were
created to base the simulated scenes on. We simulated the scene
by removing irrelevant points that are outside the sensor range, followed by
voxelization of the remaining points in a spherical coordinate instead of the
conventional cartesian coordinates.
Occlusions in the scan was then simulated by removing points with the same
angular coordinates, keeping only the closest.

![This image shows a simulated scan from one observer.](/assets/images/sensor_vox/single_sensor_scan.jpg)

After the processed voxels were finalized, they had to be rendered. This was
done by writing the shape of the voxel into a .ply file.

![This image shows a simulated scan from multiple observers, combined together](/assets/images/sensor_vox/two_sensor_scans_100_and_250.jpg)

## Reference

Citations are in progress
