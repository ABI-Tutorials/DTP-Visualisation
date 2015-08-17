
Visualisation
=============

This tutorial was created as part of the Computational Physiology module in the `MedTech CoRE <http://cmdt.org.nz>`_ Doctoral Training Programme. The tasks presented in this tutorial are designed to make the reader aware of key visualisation skills used in the context of computational physiology. We will demonstrate these skills across a range of spatial scales and visualisation techniques.

Overview
--------

Models, simulation results and image data in Computational Physiology are often three-dimensional, and may vary with time or other material or input parameters. We use visualisation to convert this raw modelling data into visual representations that illustrate the complexities of underlying biophysical behaviour in a manner that is consumable by the target audience.

This exploits humans' astounding visual capabilities, able to perceive three-dimensional objects and their spatial relationships from two-dimensional images of scenes with lighting/depth cues, possibly stereo, particularly when seen from multiple viewpoints.

Researchers need to perform interactive visualisation, alternately varying what is being spatially visualised and moving about the scene to view it from different distances and directions, and viewing changes with time. Such interaction is necessary to discover salient features in the dataset, since they may be deep within a 3-D model and hidden behind other structures, or happen in an instant of time.

One of the main challenges of visualisation is that typical output media such as documents and computer 
displays are inherently two-dimensional. We employ computer graphics rendering to make 2-D images 
from 3-D scenes, built up solely from point, line and triangle *primitives*. Visualisation algorithms are used to covert features of a model which may be any dimension into these primitives. Computer graphics rendering applies shading (colouring including with textures/images, lighting, translucency) to draw primitives out of coloured pixels on the output image, and combined with the ability to vary time and viewpoint (at interactive speeds due to the enormous processing power of graphics hardware) gives the resulting output depth and communicates temporal changes.

Often the final objective is to output one or more static 2-D images, a movie or increasingly a shareable interactive 3-D visualisation. These outputs are used for papers and theses, presentations and general publicity. It can't be over-estimated how important it is to develop these visualisation skills when working in Computational Physiology, and how effective they are in communicating results and engaging with your audience: *pretty pictures sell research*.

SimpleViz tool
--------------

This tutorial uses the *SimpleViz* MAP client plugin for interactive visualisation. Each tutorial task uses a simple workflow consisting of a File Chooser for specifying a loading script (which loads a model and sets up some initial graphics) which is passed to the SimpleViz step, as shown in  :numref:`fig_dtp_cp_vis_simpleviz_workflow`:

.. _fig_dtp_cp_vis_simpleviz_workflow:

.. figure:: _images/simpleviz-workflow.png
   :align: center
   :figwidth: 80%
   :width: 75%

   Visualisation workflow using SimpleViz in the MAP client framework.

As the name suggests, SimpleViz presents a simplified interface for performing key aspects of interactive visualisation including results output. As shown in :numref:`fig_dtp_cp_vis_simpleviz_viewpage` its interface consists of a large 3-D graphics view and a toolbar with a series of pages for performing key functions. These are described in the following tutorial tasks, however it is hoped that many features will be obvious, and you are encouraged to *play* and *have fun*.

Task 1: Viewing
---------------

Open the *DTP-Visualisation-Task1* workflow and execute it. This loads the heart model construction visualisation in SimpleViz (from the model construction tutorial) as in :numref:`fig_dtp_cp_vis_simpleviz_viewpage`.

.. _fig_dtp_cp_vis_simpleviz_viewpage:
.. figure:: _images/simpleviz-viewpage.png
   :align: center

   Task 1 SimpleViz heart model construction visualisation, with view controls.

This is a made-up example for demonstrating how complex models are built out of simple shapes (finite elements), in this case cubes. Once you play around with it you will see how a good visualisation can explain complex behaviour with great efficiency.

The example supplies the coordinate locations of 60 elements at 4 times:

1. All elements converged to a single cube (time = 0.0)
2. The elements are exploded into a regular lattice and not connected (time = 0.2)
3. The elements are merged into a block mesh of 10x3x2 elements (time = 0.4). This stage shows that corners, edges and faces of touching elements have merged (except for those eventually on the right ventricle cavity -- these open up).
4. The block mesh is deformed into the heart model, merging into a ring where ends touch, closing the apex, and opening the right ventricle (time = 1.0)

At any time switch to the time page and move the time slider to animate the model which smoothly interpolates between the above times. Note that interpolation between times 0.4 and 1.0 is not appropriate for some outside elements which get very distorted, but it is good enough for this demonstration. The following section explains how to change your view of the model which you should be constantly doing when visualising models.

Before proceeding we need to explain some concepts in order to make sense of the following tasks. This model has a **domain** consisting of a mesh of 60 cube-shaped elements which are eventually connected along certain faces. Over the domain we describe the **field** 'coordinates' which is a function mapping each of the elements' local 'xi' coordinates to their positions in the 3-D coordinate system; in this case the coordinates are interpolated from coordinates stored at discrete *node points* within the model, which can be visualised and labelled as described in task 2. In computational models there can be any number of fields defined over a domain, representing quantities ranging from material properties to the results of simulations. A key feature of visualisation is that separate fields can be used to set various visualisation attributes, including coordinates, colours, orientation and scaling, labels etc. creating an explosion in the number of permutations of possible graphics that can be displayed.

Manipulating the View
.....................

We can manipulate the view with mouse actions: clicking and dragging with the mouse in the graphics window area allows you to rotate, pan and zoom the view. The following table describes which mouse button controls which transformation.

============ ==============
Mouse Button Transformation
============ ==============
Left         Tumble/Rotate
------------ --------------
Middle       Pan/Translate
------------ --------------
Right        Fly Zoom
------------ --------------
Shift+Right  Camera Zoom
============ ==============

When we transform the view with the mouse you can see the corresponding settings change in SimpleViz' view page (see :numref:`fig_dtp_cp_vis_simpleviz_viewpage`). You can also directly enter values into the controls. Regular Fly Zoom moves the eye point closer to the lookat point. Camera Zoom changes the angle of view but the eye doesn't move; if you make a very wide angle of view and then move in close, it is like looking through a very wide angle lens. The Tumble/Rotate control rotates about an axis in the scene, like pulling on a tangent to a large sphere filling the window. Play with these controls until they make sense to you. If things start looking too weird, click the 'View All' button to restore a normal view.

In real life you can see from in front of your eyes to infinity, albeit not all in focus. In typical 3-D computer graphics everything is in focus, but you can only see a range of distances in front of your eye in the direction of the 'lookat point': between the near and far clipping plane distances. When you view the world in perspective mode (the default in SimpleViz), the part of space you see is called a *viewing frustum*, which is a pyramid seen from above but with its top chopped off at the near clipping plane. By turning off perspective you get a *parallel projection* where sizes of objects are unchanged by distance from the eye, like an extreme telephoto lens effect. Note that parallel projection uses the near and far clipping planes in exactly the same way.

Ideally we want to position the near plane just in front of everything that should be visible and position the far plane just behind everything that should be visible. The better the job we do of this the better the hidden graphics removal will work, which is important when making large high-quality, high-resolution images. SimpleViz sets the range more conservatively than this so that it doesn't need to change the ranges when objects are rotated out-of-plane. (You will notice in this example that multiple graphics drawn at the same depth appear to flash as they battle for which is in front and therefore seen. With lines and surfaces at the same depth the lines look like stitching; under the rendering page is a *perturb lines* option which brings the lines nicely in front. Try it out.)

As their names suggest, the clipping planes can also be used to good effect in hiding graphics that are in the way of what we want to see. Here we will use them to gain an insight into what graphics are actually on the screen.

On the view page, drag the near clipping plane until close parts of the model disappear; when you are close you can hover over the slider and rotate the mouse wheel which moves it with more precision. Similar clipping occurs if you zoom in close enough to the model since you can't see things behind you. The far clipping plane has a similar effect on the far side of the view.

With the front part of the model being clipped, rotate the view: you will see all the elements are hollow! This reinforces that only points, lines and triangles (surfaces) are ever drawn in computer graphics. Have a look at the list of graphics under the graphics page: it consists of lines and surfaces on the edges and faces of 3-D cube elements. You can assure yourself that the elements are 3-D by making other graphics such as elements points that are calculated in its interior; you'll need to hide the surfaces by un-checking the box next to the surfaces graphics on the list.

For the rest of this task use the viewing controls to look closely at how the bottom of the heart is merged to form an apex, and generally how the initially cube-shaped elements are distorted to make a physically realistic shape.

Task 2: Graphics and Image Output
---------------------------------

In this task we will create basic graphics to visualise a 3-D heart model, and output an image suitable for a publication. Open the *DTP-Visualisation-Task2* workflow and execute it. :numref:`fig_dtp_cp_vis_simpleviz_graphicspage_heart` shows the model visualised how we want at the end of the task, and shows the graphics page controls in SimpleViz.

.. _fig_dtp_cp_vis_simpleviz_graphicspage_heart:

.. figure:: _images/simpleviz-graphicspage-heart.png
   :align: center

   Task 2 SimpleViz graphics page showing heart model ready for print output.

The graphics page lists all the individual graphics that make up the visualisation of the model. Each listed graphics item has a square checkbox that controls whether it is visible or not. The heart model is initially visualised with lines, surfaces and node points (drawn as spheres).

Graphics Types
..............

Following are all the main graphics types that can be created with SimpleViz:

  * **Lines**: Graphics made from 1-D elements or edges of higher dimensional elements. Drawn by default with line primitives, extra controls allow them to be shown as scaled cylinders.
  * **Surfaces**: Surface graphics generated from 2-D elements or faces of 3-D elements.
  * **Points**: Visualisations of discrete locations in the model. These are each drawn with the chosen *glyph* (standard shapes including point, sphere, arrow, cone etc.) which can be scaled, oriented and labelled by different fields in the model. Variants include *point* (a single point, e.g. for drawing the axes glyph at the origin), *node points* (points in the model at which parameters are stored for interpolation), *data points* (an additional set of points not used for interpolation), and *element points* (points sampled from the interior of elements, with extra controls for sampling).
  * **Contours**: For 3-D models, produces *iso-surfaces* at which the specified scalar field equals a chosen value or values. In 2-D domains, produces *iso-lines*.
  * **Streamlines**: visualisations of the path of a fluid particle tracking along a stream vector field specified for the specified length of time. Sampling and line attributes are also settable; different line shapes allow lateral directions or curl to be visualised.

All graphics share some common attributes, for example the field giving their coordinates, the material chosen to colour the graphics, and as appropriate, limiting to exterior or particular faces of parent elements. There is also a *data field* which is used to colour the graphics by the value of the chosen field, as described later.

Select the surfaces and change the material to 'blue'. Experiment with different materials, exterior state and face values for the lines and surfaces.

Point Glyphs and Scaling
........................

Select the node points and change the glyph (e.g. to 'cube_solid') and try different values for base size. Glyphs can be oriented and scaled by fields with the final sizes each given by::

   size = base_size + scaling*scale_field

If you want the glyph to be fixed size, give it a base size and either no scale field or zero scaling. If you want the size to be proportional to a field, give it zero base size, choose a scale field and a scaling value which specifies the length/diameter/size of the glyph (since they are all unit sized). If you want to visualise a vector, make the base size '0*width*width' and the scaling 'scale*0*0' to ensure the width is fixed and the length is proportional to the magnitude of the vector. 

Create new *element points* graphics via the 'Add...' pull down menu and change the label field to 'xi' to show the element's local coordinates at their respective centres. You may need to hide surfaces to see points inside elements. Change the number of divisions to 2 (interpreted as 2*2*2 in 3-D) and change the mode to 'cell_corners'. Be aware that if you label points with the coordinates for this model the values are in a prolate spheroidal coordinate system so will not match the common x, y, z coordinates.

Data Colouring
..............

Click 'Done' and restart Task2. Hide the node points. For the surfaces graphics, choose 'fibres' for the 'data field' (we will explain what this field represents later; here we are just treating it as an interesting field to colour graphics by). Go to the 'Data Colouring' page and click 'Autorange spectrum', then 'Add colour bar' to see the full range of values of 'fibres' over the drawn surfaces. Note that the colour bar is a special *point* graphics added to the graphics list.

Colouring by a field is a key method for visualising variation of solution values across visible parts of a model. You are free to arbitrarily set the range of data values mapped to colours. Enter minimum=0, and maximum=2.

Contours
........

Add *contours* graphics and choose scale field 'slice' and isovalues=0. The slice field is defined as a scalar (single component value) given by the plane equation Ax + By + Cz with the right-hand-side given by the isovalues. Experiment with other fields such as lambda, mu, theta (the prolate coordinates) and different isovalues such as 0.7 (or multiple comma-separated isovalues) for these fields.

Drawing contours/isosurfaces is one of the key techniques for visualising the interior of a 3-D model. Often there is a threshold value of a scalar field where interesting or problematic behaviour occurs: where stress exceeds what the material can handle, or where the electric potential of the heart cells rises to a point where the muscle contracts. In such case a single image can often communicate the main features of what is happing at that time.

Always rotate, zoom and pan around to see what you have created.

Tessellation Quality
....................

On the contours graphics, restore the scalar field to 'slice' and the isovalues to '0'. Set the data field to 'fibres'. Tick the *wireframe* check box to see the outline of the actual triangles being drawn for the contours.

Change to the Rendering page of the tool bar and inspect the Tessellation divisions. The elements making up the model are divided into linear segments for graphics creation. The Minimum divisions is the number of divisions for a linear element, and these are multiplied by the Refinement factors for non-linear interpolation and coordinate systems. Hence in this example the heart elements are divided into 4 segments in each dimension.

Type '8' following by Enter in the Refinement control. You will see that all curved lines and surfaces suddenly look much smoother. Enter '1' to see how bad linear interpolation looks on these curved elements. Now Enter '16'; you will be asked to confirm this number since the 3-D elements are divided into 16*16*16 small cubes for generation of the contours, which for 60 elements requires evaluating the scalar field at 0.25 million locations, and more graphics means it may be considerably slower to generate graphics and even perceptably slower to draw on-screen. Zoom in and look around this fine visualisation.

The divisions are specified as the product of 3 numbers, one for each element 'xi' direction. Since the elements of this mesh are thinner and more simply described through the xi3 direction, enter 16*16*4 to see an almost identically high quality result with 1/4 of the calculations.

Tessellation quality is a compromise; use fewer divisions for interactive speed, and raise the number for high quality image output.

Streamlines
...........

Hide the contours and create *streamlines*. Normally streamlines are used to visualise fluid flow, however muscle tissue is fibrous and to model its deformation and electrical conduction requires the orientation of these fibres to be described throughout the domain. The 'fibres' field describes the orientation of the muscle fibres, but also the lateral sheet direction and sheet normal. This field is suitable for visualisation with streamlines.

You must specify a vector field to track along, in this case 'fibres'. Set the time length to 100 to see many fibres drawn as lines. You're free to seed streamlines from multiple sampling points, but we'll stick with the default centre of each element. Now set the bases size to '1*0.1' and the line shape to 'square extrusion'. Set the material to 'silver'. This visualises not only the direction of the muscle fibres, but also the planes of muscle fibre sheets, which have different material properties to the sheet normals. Zoom in and have a close look at the resulting graphics.

Printed Output
..............

White or coloured graphics on a black background looks great on-screen but terrible on the printed page, plus it is a huge waste of ink/toner! On the view page change the background colour to '1,1,1' i.e. white. The problem now is that the white graphics are invisible over the white background! On the graphics page select the *lines* graphics and change the material to 'black'. Do the same to the *point* graphics used to show the colour bar, so the labels appear in black. We now have what we want on the printed page (admittedly in a more powerful graphics package we may want to make the lines thicker, and change the font, however SimpleViz hides these options).

Adjust the window to the size you want, and the orientation of the heart so it looks balanced. From the Output page of the toolbar, click on 'Save image...' and enter a name, say 'myheart.png'. From outside MAP Client / SimpleViz browse to the file location and have a look at the final output image, which is ready to put in your publication.

Task 3: Deformation and strain fields
-------------------------------------



Point graphics are drawn using glyphs.  There are pre-defined glyphs that have already been created that can be used to produce interesting visualisations, sometimes a cube glyph is more appropriate representation of the data.  Select the 'node points' from the graphics list [3] and change the glyph to 'cube_solid'.  We can use the scaling to change the size of the glyph (and other graphics).  The final size of the scaled object is defined as::

If no scale field is set and the base size is zero then the graphics will be not be visible.

.. _dtp_cp_vis_graphicspane:
.. figure:: _images/graphicspane.png
   :align: center
   :alt: Graphics pane image
   
   Graphics pane

We can also add new graphics with the `add button` [4].  Add a 'point graphic', change the glyph for the new graphic to 'axes_xyz' and set the base size to 50.  This point graphic is representing the global x, y, z axes in the current scene.

The heart model contains data on the direction of the fibres within the heart wall, to visualise this we can add some more graphics.  Using the following instructions we can visualise the heart fibre direction::

   1. Add 'streamlines' using the Add combobox [3]
   2. Set the Streamlines vector field to 'fibres' using the Vector field combobox [8]
   3. Using the Shape combobox [9] set the shape to 'square extrusion'
   4. Set the base size to 1*0.2
   5. Set the sampling divisions to 1*3*1
   6. Set the Time length [10] value to 50

Here we have set the base size and sampling divisions using a special notation.  This notation allows us to set different values for different components, we can also just set one value which will be propagated across all components automatically.

It is often desirable to view the contours of the data, a contour is where the function has a constant value.  We can show contours through the heart wall volume.  To do this::

   1. Add a 'contour' graphic using the Add button [4]
   2. Set the value field to 'lambda'
   3. Set the iso value to 0.75

We can see now that the visualisation is getting quite busy.  To reduce some of the graphics visible we can try setting the exterior checkbox [5] on the surfaces.  The exterior checkbox allows us to only view the exterior surfaces of a volume.  This can be very useful especially when using transparent materials where we do not wish to show the construction of the mesh used for the model.

We can colour the graphics according to some data available in the model.  We will colour the surfaces with the lambda field to do this::

   1. Select the surfaces in the graphics list
   2. From the data combobox [6] choose the 'lambda' field

For the final rendering before we produce a publishable image we may decide that the background colour is not suitable for our target medium.  We can change the background colour one the view pane.  Using the view pane thingy box set the RGB values for the background colour, 1,1,1 will set the background colour to white for example.

We can also control the quality of the rendering via the refinement option on the rendering pane (:numref:`dtp_cp_vis_renderingpane`).  Use this control carefully it can take a long time to render highly refined graphics.  The circle divisions option controls the quality of spheres and cylinders.  Set the refinement factor to 10 and see the result.

.. _dtp_cp_vis_renderingpane:
.. figure:: _images/renderingpane.png
   :align: center
   :alt: Rendering pane
   
   Rendering pane

All of this visualisation is done through OpenGL and we can see what is actually being rendered by using the wireframe option [7] on the graphics pane (:numref:`dtp_cp_vis_graphicspane`).


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
 