---
layout: post
title: "Multithreaded Deadlock Prevention Simulator"
date: 2023-04-10 12:00:00 -0600
categories: project school os unix threads
---

## Overview

This is a school project in which we had to simulate a multithreaded application
that are competing for an arbitrary non-sharable resource. The concept of this
project forced two design features: i) we had to prevent a deadlock from
happening, and ii) we had to stop threads from accessing the same resource at
the same time, `stdout` included.
