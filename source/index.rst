.. DTP Computational Physiology: Visualisation documentation master file, created by
   sphinx-quickstart on Tue May 26 22:11:51 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Computational Physiology - Visualisation
========================================

This tutorial was created as part of the Computational Physiology module in the `MedTech CoRE <http://cmdt.org.nz>`_ Doctoral Training Programme. The tasks presented in this tutorial are designed to make the reader aware of common key visualisation skilss used in the context of computational physiology. We will demonstrate these skills across a range of spatial scales and visualisation techniques.

.. contents::
   :local: 
   :depth: 2
   :backlinks: top

Task 1
------

data/heart.zincview.py – a script that loads heart.exfile and defines some fields and initial graphics.
 
Steps:

#. Use mouse viewing transformations: tumble  (left button), pan (middle), fly-zoom (right) and (shift-right button) camera zoom. Look at the effect on camera controls in the view pane. Explain the viewing frustum and near/far planes. Explain how the closer the near and far are to the model, the more accurate is the hidden graphics removal which is important when making large high quality images. With mouse over the near/far sliders you can roll the mouse wheel to affect it – you can see the result when it is close enough to clip the model.

   * Explain perspective/parallel.
   
#. With perspective camera zoom out then fly-zoom in to get an ultra-wide angle lens view (alternatively enter a view angle above say 100 degrees)
#. View all gets you back to a normal view if things have got crazy.
#. experiment with changing the materials on the graphics.
#. try changing the glyph on the node points to e.g. cube_solid. Play with the scaling. Point to the cmgui documentation which explains how it works.
#. add a ‘point’ graphic, change the glyph to ‘axes_xyz’ and set the base size to 50 to see where the global x, y, z axes are.
#. you can create streamlines from fibres (vector ‘fibres’, square extrusion, base size 1*0.2). Setting the sampling divisions to 1*3*1 adds 3 times as many in the xi3 direction.
#. add contours from the defined lambda/mu/theta/slice fields – 0.75 gives a nice lambda value.
#. try exterior/face on surfaces.
#. try wireframe on individual graphics and explain how everything is reduced to triangles for display. Zoom in on this.
#. on the rendering pane, set the refinement factors (for non-linear interpolation) to 10 and see the result; larger numbers require confirmation! Warn them about the costs. Circle divisions controls quality of spheres and cylinders.
#. select lambda as the data field on the surfaces, which is nicely colourful.
#. change the background colour on the view pane to white (1,1,1); note you’ll need to change lines material from white to another colour such as black.

Task 2
------
 
data/airways/AirwaysLobes.ex{node|elem}.

Steps:

#. Load nodes then elements (it’s a large model so doesn’t load instantly) and get attendees to create lines. They need to do view-all on the view tab to see them.
#. change the line shape to circle extrusion with scale by the ‘general’ field (a radius) with scaling 2 to make it a diameter.
#. zoom in to see the gaps between the line segments. Add nodes with sphere glyphs scaled by the same fields to close off the gaps.
#. colour by radius by picking general as the data field
#. on the data colouring tab. Click on autorange spectrum, try different ranges to make the image pretty. Add a colour bar. The colour bar appears in the list of graphics, but it uses some hidden attributes (not editable) to make it appear on top where it is. You can change the colour of the point graphics for the colour bar which affects its shininess and the colour of the labels. The colour bar is actually just a glyph, but it’s pretty silly to plot it at every node, for example, but it works!

   * (Alan  may be able to get another model with dependent fields by the time of the course, which will make it more interesting to visualise.)

#. [Alan needs to implement output of images here] From the output tab output a [hopefully hires] image of what’s on the screen, including colour bar, suitable for putting in your report.
 
Task 3
------

data/deforming_heart/deforming_heart.zincview.py
 
#. load the model which is similar to the heart in part 1 but has twice as many elements. It also defines strain fields and creates element point graphics which visualise mirrored glyphs to show principal strains: inward and red for compression, outward and blue for extension.
#. on the time pane, adjust the time slider to animate the model
#. zoom in and look at the deformation of parts of the tissue, the twisting of the ventricle etc.
#. change the glyph to arrow_solid (on all 3 element points) and line to see the difference.
#. advanced users may look at the script to see how these additional field are created by expressions; interest them in the possibilities of what they could visualise. They can also see how the time-varying model was loaded.
#. [Alan would have to implement]. Output to ThreeJS an animating, beating heart (probably no glyphs?)

Task 4
------
 
Ideally I would have liked to have got images into the rendering; I’m sure Alan could get the volume texture example going pretty quickly; can use that to segment part of the foot. Images combined with a model requires us to support multiple regions or groups, which I haven’t had time to do; adding a region chooser would be simplest I think, but probably no time. Could always draw image in another region (from the loading script) but wouldn’t be able to hide it.
 