---
layout: post
title:  "Texture rendering demo"
date:   2021-04-25 15:05:00 +0900
categories: dev
---

After practicing and learning how texture rendering works in OpenGL, I added
such functionality to libre2d.

For starters, I shifted around a bit of the OpenGL management. Instead of the
application initializing libre2d via a static Component function, I decided it
was cleaner to do it in Model, as there should only be one Model. That also
makes more sense for rendering, as the Model can pass the shader handle to the
Components via the render call, instead of it being static to all Components.

Then I added a [utility function][load-texture-func] for loading the texture
file from an image. The Model contains the texture, and then the Components
each have their UV map.  Components have a vector of vertices, and now they
have a vector of UV points as well, which are coordinates in the texture. The
UV point is mapped to the vertices by the index in their respective vectors.
Then I just had to add texture to the Component's render function, and it all
worked fine.

For now I'm using [stb_image.h][stb-image], and we're loading the texture from
a file directly. In the end I want to load it from a libre2d model file, but we
don't have that yet, so for now we'll bootstrap with this.

Well, of course I had to make a [texture file][texture-file] for demo (and
testing) purposes. I'm not a very good visual artist, and I didn't bother to
fix the mesh issues that I had last time, but I think it came out fairly well.
(I guess in hindsight I could've compressed the texture file a bit, oh well.)

![Texture face render](/assets/2021-04-25-texture-render-demo.png)

Also I studied the [deformation part][live2d-docs] of Live2D's documentation a
bit more. It looks like they used a grid for each Component, but the grid lines
are actually Bézier curves. It seems like normally there are 2x2 Bézier
divisions (so I guess 1 center Bézier grid line per axis?) and a maximum of
3x4. Then there are also conversion divisions, normally 5x5 with a maximum of
8x8. It seems to me that the conversion divisions are basically interpolations
of the other Bézier grid lines that are on the same axis
(like in [the example][live2d-example]).

Looking at that example (and the hololive girls), I'm starting to think that
this Bézier curve grid based transformation would be smoother. With the
interpolation model that I'm using with endpoint Meshes, it would be very
tedious to create the deformations, and the animation would likely be blocky.
It should also be easier to code (I think/hope), so I think next week I'll have
to overhaul the transformation functions with this first, before I can move on
to parameter setting.

[load-texture-func]: https://github.com/libre2d/libre2d/blob/paul/dev/src/libre2d/utils.cpp#L128
[stb-image]: https://github.com/nothings/stb/blob/master/stb_image.h
[texture-file]: https://github.com/libre2d/libre2d/blob/paul/dev/test/test-face/texture.png
[live2d-docs]: http://sites.cybernoids.jp/cubism_e/modeler/basic/deformer/about
[live2d-example]: http://sites.cybernoids.jp/cubism_e/modeler/basic/deformer/placement-basic
