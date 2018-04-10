---
layout: post
title: "Dockerize pytorch to run it on Windows with CPU"
date: 2018-03-27
tags: docker pytorch beginner
author: Jiri Hanousek
---
Imagine you want to learn new framework and instead of diving into it you get stuck on installation of prerequisites.
Imagine that you just want to start low with pytorch only on CPU and Windows while waiting for Bitcoin-mining slavers
to release hard working GTXes which all were sold out during black friday.

Docker comes to rescue! You can stay comfortably in Windows and invite Linux to visit you via container.

I simply modified pytorch [docker file from their github](https://github.com/pytorch/pytorch/blob/master/docker/pytorch/Dockerfile)

Use [my version](https://github.com/zzajc/zzajc.github.io/blob/master/src/docker-pytorch-cpu/Dockerfile) to speed up your beginner phase and modify later.

