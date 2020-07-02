---
layout: post
title: A tiny ray tracer
comments: true
---

Ray tracing in 200 lines of code.


While looking for a fun small project to dust off my rusty C++, I stumbled upon [this](https://github.com/ssloy/tinyraytracer/wiki/Part-1:-understandable-raytracing) 
fun mini-course on ray tracing.

## What is ray tracing ?
According to Wikipedia:

> In computer graphics, ray tracing is a rendering technique for generating an image 
> by tracing the path of light as pixels in an image plane and simulating the effects
> of its encounters with virtual objects. The technique is capable of producing a high
> degree of visual realism, more so than typical scanline rendering methods, but at 
> a greater computational cost.

Ray tracing is mostly used in 3D animated movies and visual effect, where long rendering time 
is fine. Until recently, it wasn't suited for real-time applications, like video games, but newer 
games do support it, thanks to advances in GPU capabilities.
