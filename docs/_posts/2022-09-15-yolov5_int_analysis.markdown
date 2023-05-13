---
layout: post
title: "Yolov5 Traffic Intersection Analysis"
date: 2022-09-15 12:00:00 -0700
categories: project CST CV
---

# Overview

The goal of this project was to make a program that could count and analyse
the objects passing through an intersection. We had gone through many ideas
before settling with using yolov5 object detection and strongSORT object
tracking to keep track of objects. Using this implementation, we were able to
keep track of the labels and coordinates of each object on the screen.

To speed up the process of object detection and tracking, we ran yolov5 on the
Compute Canada high performance computer array with reduced fps and detection
model. This allowed us the cut the running time from five to seven hours to
only around 40 minute for every 30 minutes of input data.

![yolov5example](/resources/images/yolov5_int_analysis/yolodetectiontraffic.png)

To analyse these intersections, we needed to process the outputs from the
yolov5 program. Using multiprocessing, we were able to determine the cars Time
to Collision, Post Encroachment Time, and Time Difference to the Point of
Intersection using different algorithms that took advantage of the coordinate
datas retrieved from the yolov5 detection.

# Implementation

## object detection parsing

An object was created for each detected vehicles and pedestrians, where the
coordinate at every frame will be recorded. This allows for ease of trajectory
calculations and storage. The data related to each object are stored in a
vectorized array.

## Coordinate conversion

An essential feature in this project was the ability to convert a pixel
coordinate taken from the video, and converting it into real world coordinates.
A rudimentary version of this was completed by using a transformation matrix,
using a reference grid in the form of the square crosswalk. Corner points were
selected on the crosswalk, and the coordinates of these points were collected
on both the video feed and google map. In the initial version of the program
the data taken from google map was in reference to the most north-western
corner of the crosswalk. This let us calculate the local coordinates in meters
and create the transformation matrix to convert the pixel coordinate into
meters coordinate, although the values are not very precise.

### Future works

There were plans to improve this feature, in terms of accuracy and gaining
access to proper global coordiates, however, I have left the project by then.
