---
layout: post
title: "Data Rate Voxelization"
date: 2022-11-01 12:00:00 -0700
categories: project CST pointcloud
excerpt_separator: <!--more-->
---

## Overview

### Motivation

The program was created because we wanted to see if we could quantify the amount
of data that an autonomous vehicle would require in a particular position on
the planet.

In this project, I was tasked with analysing the data rate required to process
data from a sensor as an autonomous vehicle moves through an environment. To
do this, we used dense LiDAR point clouds taken as the input.
Points on the road following the trajectory of the scanning vehicle were then
created to base a simulated scene on. We then begin the simulated scene creation
by removing irrelevant points that are outside the sensor range.
We then voxelize the remaining points in a spherical coordinate instead of the
conventional cartesian coordinates.
Occlusions in the scan was then simulated by removing points with the same
angular coordinates but different distance from the sensor, with the idea being
that only the closest point should be seen by the sensor.

![Point cloud geometric roughness rendering](/assets/sensor_vox/single_sensor_scan.jpg)

The resulting output from running Python point cloud roughness on a sample LiDAR
input seen above was identical to those from the CloudCompare geometric
roughness calculations.

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

## Future Works

### Occlusion Handling

Although working with larger angluar resolutions, once the resolution increases,
inconsistent and sometimes incorrect voxels are rendered. Essentially, points
that realistically should not be showing up are appearing because the current
system does not take into account the real geometry, and instead, only the
points present in the input. A filtering method should be implemented to
increase the accuracy of occlusion handling.

## Reference

Citations are in progress
