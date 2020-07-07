---
layout: post
title: A tiny ray tracer
comments: true
---

Ray tracing in 200 lines of code.


While looking for a fun small project to dust off my rusty C++, I stumbled upon [this](https://github.com/ssloy/tinyraytracer/wiki/Part-1:-understandable-raytracing) 
fun mini-course on ray tracing. I used it to render this simple scene (composed of four reflective 
spheres, three light sources, and the background image): 

<img src='{{ site.baseurl }}/public/resources/raytraced.gif' width="1080" />

(Background image credit: [P. Horálek](https://www.facebook.com/PetrHoralekPhotography) / ESO)
The source code used to generate this GIF can be found [here](https://github.com/elrhaff/tinyraytracer).

## What is ray tracing ?
According to Wikipedia:

> In computer graphics, ray tracing is a rendering technique for generating an image 
> by tracing the path of light as pixels in an image plane and simulating the effects
> of its encounters with virtual objects. The technique is capable of producing a high
> degree of visual realism, more so than typical scanline rendering methods, but at 
> a greater computational cost.

Ray tracing is mostly used in 3D animated movies and VFX, where long rendering time 
isn't an issue. Until recently, it wasn't suited for real-time applications, like video games, but newer 
games support it (to some extent), thanks to advances in GPU capabilities.


## Ray tracing algorithm description:

<br/>

<img src='{{site.baseurl}}/public/resources/Ray_trace_diagram.svg' width="720" />

The main idea is to cast rays from the camera through each pixel in the image, and make them "bounce"
off the objects of the scene a fixed number of iterations (called the <em> depth </em>). A depth of one is what is
called <em> [ray casting](https://en.wikipedia.org/wiki/Ray_casting) </em> . Higher depths lead better results. A depth of > 6 is generally good 
enough for to get a realistic looking scene. An infinite depth is what is called 
<em> [global illumination](https://en.wikipedia.org/wiki/Global_illumination) </em>  (how real world
illumination works).


## Ray tracing main steps:

* Object ray intersection
* Lighting
* Refractions
* Envmap
