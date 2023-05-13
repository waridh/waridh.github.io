---
layout: post
title: "Python Point Cloud Geometric Roughness"
date: 2022-10-10 12:00:00 -0700
categories: project CST pointcloud
excerpt_separator: <!--more-->
---

## Overview

In this project, I challenged myself to recreate the geometric roughness analysis from the CloudCompare software in Python. The original code was written in C++, and it measures roughness by creating a best fitting least square plane based on neighbouring points, where the neighbours are points that are within a certain radius of the point being analysed. From there the roughnesss is determined to be the distance the point being analysed is from the best fitting plane.

I recreated this method on python using the functions and libraries available
on there. Some of the major libraries that was used to achieve this was Laspy,
Numpy, and Scipy to name some. The goal of this project was to make a program
that could count and analyse
the objects passing through an intersection. We had gone through many ideas
before settling with using yolov5 object detection and strongSORT object
tracking to keep track of objects. Using this implementation, we were able to
keep track of the labels and coordinates of each object on the screen.

To speed up the process of object detection and tracking, we ran yolov5 on the
Compute Canada high performance computer array with reduced fps and detection
model. This allowed us the cut the running time from five to seven hours to
only around 40 minute for every 30 minutes of input data.

![Point cloud geometric roughness rendering](/assets/images/python_geo_roughness/roughness0.3road1.jpg)

The resulting output from running Python point cloud roughness on a sample LiDAR input was identical to those from the CloudCompare geometric roughness calculations. I was very proud of the result that was achieved by this program. To analyse these intersections, we needed to process the outputs from the
yolov5 program. Using multiprocessing, we were able to determine the cars Time
to Collision, Post Encroachment Time, and Time Difference to the Point of
Intersection using different algorithms that took advantage of the coordinate
datas retrieved from the yolov5 detection.

