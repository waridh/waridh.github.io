---
layout: post
title: "Data Rate Voxelization"
date: 2022-11-01 12:00:00 -0700
categories: project CST pointcloud
excerpt_separator: <!--more-->
---

## Overview

In this project, I was tasked with analysing the data rate required to process data from a sensor as an autonomous vehicle moves through an environment. To do this, we used LiDAR point clouds taken by Transport Alberta as the input. The trajectory of the scanning vehicle was created by making a best fitting least square parametric based on the points that were scanned directly beneath the scanner vehicle. We then created evenly spaced roadpoints on this parametric. From there, we take a snapshot of the point cloud from each point, where we remove points that are too far away to be seen. We then voxelize those points in a spherical coordinate instead of the conventional cartesian. Occlusions in the scan was then simulated by removing points with identicle angular coordinates that are not the closest one.

![Point cloud geometric roughness rendering](/assets/sensor_vox/Single sensor scan.jpg)

The resulting output from running Python point cloud roughness on a sample LiDAR
input seen above was identical to those from the CloudCompare geometric
roughness calculations.

The github repository hosting the code for this project can be found [here](https://github.com/waridh/python_roughness_pointcloud).

## Methodology

The goal of this project was to recreate the output from CloudCompare, no GUI
was developed for it.

### I/O

The input and output file are of .las format, a common format to store LiDAR
point cloud. The library used to parse this file format was chosen to be Laspy,
as it was flexible with the amount of content provided by the input file, while
still being able to detect more niche fields. It also allows point cloud object
to be exported to a .las file.

### Datastructure

- A class was constructed to store the input point cloud based on numpy arrays.
- Methods for initializing and processing data are included in this class.

### Roughness calculations

When calculating the geometric roughness, a method on the object is called that
will create multiple processes (python multiprocessing) to calculate the
roughness in parallel. Each process will iterate through every point that has
been assigned to it and use a range query on a kd-tree to create a least square
fitting plane around the point. The distance from the target point to this
least square fitting plane built on the points returned from the range query
will be taken as the geometric roughness value of that point. By using
multiprocessing, we were able to speed up the processing time of the geometric
roughness calculations.
