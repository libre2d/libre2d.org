---
layout: post
title:  "Learning opengl"
date:   2021-04-04 14:37:00 +0900
categories: dev
---

I spent this weekend studying OpenGL, and trying to figure out how I would fit
it in to libre2d. tbh I was hoping to get framebuffers to work, so that I could
also allow headless rendering, like for implementing a uvc-gadget, but it looks
like a window context is required. I suppose all users of the library have
rendering as a use case anyway.

So I got a framebuffer tutorial working, but I don't see much point in it
besides allowing simple post-processing. Maybe it's best just to use the
default framebuffer, and allow a user input for a non-default framebuffer if
the user wants to do post-processing with framebuffers.

Now I think I have all (or most) of the parts and knowledge that I need for
putting OpenGL into libre2d, so hopefully next week I'll have something up.
Probably just vertices or wireframe rendering first, then after that I'll move
on to image loading and textures and z-indexing.

On another note, I found [documentation for Live2D's cubism][cubism-docs]. I
see that they use Bezier curves instead of defining keyframe meshes like I do.
But I think our library is fine with low-level mesh interpolation, and
higher-level and more fluid transformation definitions could be implemented by
the application. Though I guess we could move it into the library if it's a
really common operation.

I also found [Live2D's API reference][live2d-api], though the documentation is
really sparse. At a glance it looks like they have a much higher-level API for
manipulating models, compared to what I have designed.

[cubism-docs]: http://sites.cybernoids.jp/cubism_e/modeler/basic/deformer/about
[live2d-api]:  http://doc.live2d.com/api/core/cpp1.0e/
