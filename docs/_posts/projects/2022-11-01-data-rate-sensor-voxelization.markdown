---
layout: post
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
created to base the simulated scenes on. We then begin the simulated scene
creation by removing irrelevant points that are outside the sensor range.
We then voxelize the remaining points in a spherical coordinate instead of the
conventional cartesian coordinates.
Occlusions in the scan was then simulated by removing points with the same
angular coordinates but different distance from the sensor, with the idea being
that only the closest point should be seen by the sensor.

![Point cloud geometric roughness rendering](/assets/images/sensor_vox/single_sensor_scan.jpg)

The resulting output from running Python point cloud roughness on a sample LiDAR
input seen above was identical to those from the CloudCompare geometric
roughness calculations.

## Methodology

### Input

The program takes dense point cloud data taken from LiDAR scanners as an input
for sensor simulation. The format of this file was a .las filetype.

### Trajectory and Observers

The trajectory of the scanning vehicle was recreated by fitting a least square
parametric curve on the points that layed directly under the scanning vehicle.
Points were then evenly placed on this curve with a the distance between them
set to a user defined value. Points are then placed directly on top of the
points on the parametric curve. These point are named observer points, and it
is the reference points for the simulations.

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
