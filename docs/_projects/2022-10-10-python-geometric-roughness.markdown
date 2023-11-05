---
layout: single
title: "Python Point Cloud Geometric Roughness"
date: 2022-10-10 12:00:00 -0700
categories: project CST pointcloud
excerpt: "Python implementation of geometric roughness point cloud analysis from CloudCompare."
---

[repository](https://github.com/waridh/python_roughness_pointcloud)

## Overview

Python implementation of the geometric roughness
analysis from the CloudCompare software in Python. The original code was
written in C++, and it measures roughness by creating the best fitting least
square plane based on neighbouring points, where the neighbours are points
that are within a certain radius of the point being analysed. From there the
roughnesss is determined to be the distance the point being analysed is from the
best fitting plane.

This function was recreated on python using available functions and libraries.
Some of the major libraries that was used to achieve this was Laspy, Numpy,
and Scipy.

![Point cloud geometric roughness rendering](/assets/images/python_geo_roughness/roughness0.3road1.jpg)

The resulting output from running Python point cloud roughness on a sample LiDAR
input seen above was identical to those from the CloudCompare geometric
roughness calculations.

The github repository hosting the code for this project can be found [here](https://github.com/waridh/python_roughness_pointcloud).

## Implementation

The goal of this project was to recreate the output from CloudCompare, no GUI
was developed for it.

### I/O

The input and output file are of .las format, a common format to store LiDAR
point cloud. The library used to parse this file format was chosen to be Laspy,
as it was flexible with the amount of content provided by the input file, while
still being able to detect more niche fields. It also allows point cloud object
to be exported to a .las file.

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

To improve on the performance, threading should be used instead of
multiprocessing.

## References

- [CloudCompare Geometric Roughness](https://www.cloudcompare.org/doc/wiki/index.php/Roughness)
- [Multiprocessing library](https://docs.python.org/3/library/multiprocessing.html)
