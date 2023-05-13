---
layout: post
title: "Yolov5 Traffic Intersection Analysis"
date: 2022-09-15 12:00:00 -0700
category: project
---

 The goal of this project was to make a program that could count and analyse
 the objects passing through an intersection. We had gone through many ideas
 before settling with using yolov5 object detection and strongSORT object
 tracking to keep track of objects. Using this implementation, we were able to
 keep track of the labels and coordinates of each object on the screen.

To speed up the process of object detection and tracking, we ran yolov5 on the
Compute Canada high performance computer array with reduced fps and detection
model. This allowed us the cut the running time from five to seven hours to
only around 40 minute for every 30 minutes of input data.

![yolov5example](/../../resources/images/yolov5_int_analysis/yolodetectiontraffic.png)