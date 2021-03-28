---
layout: post
title:  "dev: Mesh update"
date:   2021-03-28 12:37:00 +0900
categories: dev
---

This weekend I redesigned how Meshes work with their anchors and center point.
In my last design, I had a KeyFrame to group the Mesh with the anchor points
and the center point, which I figured was an extra level of indirection that we
don't need. So I moved the vertices that make up the center point and the
anchor points into the Mesh, and then added separate member variables to hold
their indices in the vertices array. That makes Mesh transformation operations
more convenient (and also faster? because we can skip vector concatenation or
setting up multiple loops). I also added a triangles vector to the Mesh, to
hold indices from the vertices array to designate triangles, for uv mapping
later. This was in hopes that it would make it easier to send the vertices to
OpenGL, since as far as I know, this matches how index buffer objects work.

Then I plumbed that back into Parameter and Component, and then fixed up
setParameter through the layers, and implemented Model::setParameters() as
well. I also added z-indexing to vertices, because that's a feature that we'll
need.

I'm stuck on how to implement rotation and scaling with the Parameter still
though. Realistically I think there would only be one rotation/scaling
operation per component, since it doesn't make sense to have multiple. Then
maybe I could have a special Parameter attached to a Component...? And it would
have to be processed after all of the other Parameters have been processed...
hm, it might work. I'll queue it up as a task, because I have a feeling that
most motion can be implemented with just mesh interpolation. I really want to
get just *something* working out first, and then polish things up. Though
special parameters might show up later again, when I want to do opacity and
texture swapping, but that's in the far future.

Next time I really want to get rendering working. I've decided to go with a
stateful synchronous design, so the application would call
Model::setParameters(), which would change the Model's state, and then do
Model::render(), or maybe just Model::render() and passing in the parameters.
There isn't much reason for libre2d to be threaded internally I think, so the
application can set up a thread if they so wish. Maybe the application can even
double-buffer the Model, since it's not going to be modified. I'll have to
study OpenGL framebuffers, since I'm hoping to allow rendering to work even
headless. We'll see how it goes.
