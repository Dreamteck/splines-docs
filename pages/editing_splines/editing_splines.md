# Editing Splines
A spline control point has a couple of properties:

- Position – the position of the point
- Normal – a normal vector used for calculating rotations around the spline
- Tangent 1 and 2 – Tangent positions for Bezier splines
- Size – A size of the point (1 by default)
- Color – a color for the point (white by default)

These parameters can be edited quickly in a user-friendly editor, dedicated for that. 

Once the control points of a spline are created, they can be edited one by one or together in a group. The most basic editing mechanism is dragging the points directly inside the scene view when not in any of the editor modes (Create, Delete, Move, Rotate, etc.). This moves the points along the camera plane so when working in 3D it is recommended that a Top, Right or Forward perspective is used in the editor. If more than one point is selected, when dragging a point on screen, the rest of the selected points will also move.
## Move Tool (W)
![](./_images/019.png)

The spline point Move Tool (which is different than Unity’s move tool) can be toggled from the Edit panel toolbar by clicking the Move icon, or by pressing W while the Edit panel is open and the mouse is inside the scene view.

![](./_images/020.png)

The Move tool provides a position handle which works exactly like the regular editor move tool. The difference is that it only operates on spline points and when the spline point Move tool is on, the editor move tool cannot be selected.

The Edit Space property of the move tool defines how the handle will be oriented. World will orient the handle along the world’s axis. Transform will orient the handle along the Spline Computer’s transform rotation and “Spline” will orient the handle along the spline. Note that “Spline” edit space only works when a single spline point is selected.

![](./_images/021.png)

![](./_images/022.png)

The Move On Surface toggle allows the points to be projected onto a collider when moving them (uses Raycasting). This is very similar to the Surface point creation mode, except this option allows to project already created points onto surfaces. When the Move On Surface toggle is checked, the position handle is replaced with a circle handle. Clicking and dragging the circle handle over a surface will position the points at the surface point where the mouse hovers.

## Rotate Tool (E)
![](./_images/002.png)

![](./_images/023.png)

The Rotate tool will rotate a single point’s tangents and normal or rotate multiple points around their average center.

Two checkboxes define whether the normals of the points should be rotated and whether the tangents of the points should be rotated. 



## Scale Tool (R)
![](./_images/003.png)

The Scale tool will scale a single point’s tangents and size. When multiple points are selected, the points’ positions will also get scaled and offset based on their average center.

![](./_images/024.png)

Similarly to the Rotate tool, scaling tangents and sizes can be toggled on or off separately using the tool’s properties displayed under the toolbar when the tool is selected. 
## Normal Tool (T)
![](./_images/025.png)

The Normal Tool is a new kind of tool developed to ensure easy and intuitive normal editing for spline points. It has two modes. 
### Auto Mode
In Automatic mode, the normal are automatically calculated to always be perpendicular to the spline. This is usually the desired result so Auto is the mode that is selected by default. When a point is selected, a circular handle is drawn around it with a line going from the point, towards the normal direction. At the tip of the line, there is a circular free-move handle. Dragging this handle will rotate the normal around the spline.

![](./_images/026.png)
### Free Mode
Free mode allows the circular handle to be moved freely in each direction. This is a legacy tool which is usually not used anymore, however, there might be cases where spline normals don’t need to be perpendicular to the spline and this is where this mode comes in handy.

![](./_images/027.png)
### Normal Operations
![](./_images/028.png)

The Normal tool offers a range of normal operations which can be performed on the currently selected points. Using a normal operation is done by simply selecting it from the Normal Operations dropdown list while the desired spline points are selected. 

The available normal operations are:

- Flip – Inverts the direction of the normal
- Look at Camera – The normal will look towards the editor camera’s position
- Align with Camera – The normal will align with the editor camera’s direction
- Calculate – The normal will automatically be calculated based on the location of the previous control points
- Left – World Negative X
- Right – World Positive X
- Up – World Positive Y
- Down – World Negative Y
- Forward – World Positive Z
- Back – World Negative Z
- Look at Avg. Center – will look at the average center of the selected points (works only when multiple points are selected)
- Perpendicular to Spline – Will make the normal perpendicular to the spline while trying to preserve the general direction

## Mirror Tool (Y)

![](./_images/004.png)

The Mirror tool does what its name suggests – it duplicates and mirrors the spline points along a given axis.

![](./_images/029.png)

The mirror tool reflects the spline based on the mirror center which by default matches the position of the SplineComputer’s Transform. If the symmetry center isn’t visible, make sure to look for it around the computer’s transform position. 

![](./_images/030.png)

The mirror center can be moved with the position handle attached to it or by modifying the center coordinates in the inspector.

By default, the spline is reflected along the X axis, but this can be changed using the Axis dropdown menu. If flip is unchecked, the points to the left of the center will be reflected to the right, otherwise the points on the right will be reflected to the left.

The “Weld Distance” setting defines at what distance from the symmetry plane two reflected points will be merged into one. If the spline is of Bezier type, the merged point will have broken tangents.

The Apply and Revert buttons are used to either finalize or revert the changes made with the mirror tool. If the mirror tool is exited without clicking the Apply button, a dialog will appear asking if the made changes should be kept or reverted.
## Primitives 

![](./_images/005.png)

Dreamteck Splines provides a list of procedural primitives and a preset saving system. Both are located in the Primitives & Presets tab inside the Edit panel (last button on the right).
### Using primitives
To enter the Primitives mode, click on the “Primitives” button under the toolbar. Immediately, the first available primitive will be loaded and the spline will be changed.

![](./_images/031.png)![](./_images/032.png)

Different primitives can be selected using the dropdown menu just bellow the Primitives button. Each primitive has its own set of settings. Clicking “Apply” will save the primitive and make it into an editable spline. Revert will revert the previous state of the spline.
### Using presets
When preset mode is entered, the spline is not immediately changed. Instead, there is a Use and a Create button:

![](./_images/033.png)

The Create New button will create a new preset out of the current spline.

The Use button will use the currently selected preset from the dropdown menu. Delete will delete this preset. 

Spline presets are saved as files under Dreamteck/Splines/Presets When updating the plugin version, it is a good idea to backup this folder if custom presets are used.
## Point Operations

Dreamteck Splines provides a set of simple and handy operations for editing points. These operations can be found in a dropdown menu at the bottom of the Edit panel when at least one point is selected.

![](./_images/034.png)

After selecting a point operation, click the “Apply” button beneath to execute it.
### Center to Transform
When splines are created, their points can be positioned very far from the position of the Spline Computer’s Transform. Sometimes the control points need to be brought to the center of their transform so proper transformation can be applied. This operation brings all selected points to the center of the transform while maintaining their relative position.

![](./_images/035.png)
### Move Transform To
This is the reverse equivalent of Center to Transform. This operation will move the position of the Spline Computer’s transform to the average center of the selected points without moving any points.
### Flat X, Y and Z
These operations flat the selected points’ positions along the given axis.

![](./_images/036.png)![](./_images/037.png)

If the spline type is Bezier, a dialog will appear upon selecting the flat operation giving the option to flat only the points’ positions, only the points’ tangents or everything.
### Mirror X, Y and Z
Will mirror the selected points around their average center.
### Distribute evenly
Attempts to distribute the points at even distances from each other.

Before:

![](./_images/038.png)

After:

![](./_images/039.png)
### Auto Bezier Tangents
Will automatically calculate the position for the Bezier points’ tangents so that the spline starts to look like a Hermite spline.
## Editing the Spline Computer’s Transform
When a spline object is selected and the Edit panel is open in the inspector, the editor tools are not available and therefore its Transform cannot be changed unless values are directly typed inside the transform’s property fields. In order to move, rotate and scale a spline using the editor tools, Edit mode must be exit by folding up the Edit panel:  
![](./_images/040.png)

Once the Edit panel is closed, the editor tools will become available for the spline’s object.
## Framing control points
When a Spline Computer is selected and is in edit mode, the framing functionality of the Unity editor can be used to frame all or only the selected spline points. If the spline computer has one or more spline points, then going to Edit->Frame selected will frame the selected points or all points if none are selected. If the Spline Computer doesn’t have spline points, then the default command behavior is executed.

