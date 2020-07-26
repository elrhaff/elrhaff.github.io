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
<em> [global illumination](https://en.wikipedia.org/wiki/Global_illumination) </em>  (as real as it gets!).


## Ray tracing main steps:
In the following, I'll describe briefly the steps and difficulties that arise when implementing this algorithm. Fore more details, please check the original [tutorial](https://github.com/ssloy/tinyraytracer/wiki/Part-1:-understandable-raytracing).

### Object ray intersection
Our algorithm starts by casting rays from the camera and through each pixel of the image. For each of these rays, we need to find what objects (meshes or surfaces) of the scene it is
intersecting with.

This task is relatively simple (from a time complexity perspective) for regular volumes that can be described easily by a mathematical equation. In the
case of a sphere, solving the intersecton of a ray (defined by an origin and a direction vector) with a sphere requires basic trigonometry:

```cpp
struct Sphere {
    Vec3f center;
    float radius;
    Material material;

    Sphere(const Vec3f &c, const float r, const Material &m) : center(c), radius(r), material(m) {}

    bool ray_intersect(const Vec3f &orig, const Vec3f &dir, float &t0) const {
        Vec3f L = center - orig;
        float tca = L*dir;
        float d2 = L*L - tca*tca;
        if (d2 > radius*radius) return false;
        float thc = sqrtf(radius*radius - d2);
        t0       = tca - thc;
        float t1 = tca + thc;
        if (t0 < 0) t0 = t1;
        return t0 >= 0;
    }
};
```

If there's no intersection, then we just set the pixel that the ray goes through to some backgroud color. The main loop (over all pixels) looks like this:

```cpp
Vec3f cast_ray(const Vec3f &orig, const Vec3f &dir, const Sphere &sphere) {
    float sphere_dist = std::numeric_limits<float>::max();
    if (!sphere.ray_intersect(orig, dir, sphere_dist)) {
        return Vec3f(0.2, 0.7, 0.8); // background color
    }
    return Vec3f(0.4, 0.4, 0.3);
}

void render(const Sphere &sphere) {
    for (size_t j = 0; j<height; j++) {
        for (size_t i = 0; i<width; i++) {
            float x =  (2*(i + 0.5)/(float)width  - 1)*tan(fov/2.)*width/(float)height;
            float y = -(2*(j + 0.5)/(float)height - 1)*tan(fov/2.);
            Vec3f dir = Vec3f(x, y, -1).normalize();
            framebuffer[i+j*width] = cast_ray(Vec3f(0,0,0), dir, sphere);
        }
    }
}
```

The intersection of a ray with a triangular meshes is much more time consuming. For each ray, we need to verify the intersection with all of the triangles. That's why ray tracing is slow in practice.
This can be sped-up by only considering rays that intersect the bounding box of the mesh (min and max coordinates of the vertices over the different axes). Rays that don't interset 
the bounding box won't intersect the mesh.

* Lighting
* Refractions
* Envmap
