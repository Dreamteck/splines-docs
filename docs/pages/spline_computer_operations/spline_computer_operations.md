# Spline Computer Operations
Going back to the top of the Spline computer panel, there are a couple of buttons which execute some useful operations for the currently selected spline(s).

![](./_images/058.png)
## Closing and breaking a spline
![](./_images/059.png)

![](./_images/060.png)

A spline path consisting of four or more points can be closed. This will connect the first and the last points of the spline making the spline go in a loop. To close a spline, first ensure that there are at least four control points present, go to the inspector and click the “Close” button. If the close was successful, the last spline point will be moved to the first one and the Close button will read “Break”.

### Closing a Spline During Point Creation
If a spline consists of three or more control points, it can be closed in point creation mode if the place where the newly created point on screen overlaps with the position of the first point from the spline’s opposite end. Doing so will open a dialog asking whether the spline should be closed. 
## Breaking
To break a closed spline, press the “Break” button. The first point of the spline will be separated into two points.

Splines can also be broken at a different point. If a single point is selected when clicking “Break”, that point will be separated.
## Reversing a Spline

Sometimes it’s useful to be able to reverse the control point order without changing the spline’s shape. Doing so will reverse the spline’s direction. The Reverse button does exactly that. When a spline is reversed, visually nothing changes but selecting the first point and looking it up in the inspector will reveal that it has become the spline’s last point and vice versa.

![](./_images/061.png)

## Merging Splines

![](./_images/062.png)

It is possible to merge two spline objects into one. To enter merge mode, click on the enclosed “+” button in the top right of the Spline Computer panel. 

![](./_images/063.png)

![](./_images/064.png)

When Merge mode is active on one spline, the rest of the splines in the scene will be drawn with their ends marked as big hollow circles. 

Clicking on either circle, will merge the given spline with the currently selected one.

![](./_images/065.png)

There are two options for merging. One is the merge side. By default, the merge side is set to “End”, meaning that whichever end is selected for the foreign spline, it will be merged to the selected spline’s end point. Setting this option to “Start” will work the other way around. 

“Merge Endpoints” defines whether the end points of the spline should be merged into one or not when merging. 

## Splitting a Spline

A single spline can be split into two splines using the Split mode. In Split mode, a cursor is projected along the spline where the mouse is. Clicking on that cursor will split the spline into two splines immediately.

![](./_images/066.png)

## Transform-only Editing
If the Space property of the Spline Computer is set to Local, an additional toolbar will appear at the bottom of the Spline Computer panel with some transform tools:

![](./_images/067.png)

![](./_images/068.png)

These tools allow for editing the Spline Computer’s transform without changing the spline. 

When either of the tools is selected, the transform handle will appear at the game object’s position and will have a label with the game object’s name beneath.

This is useful when the spline’s pivot point needs to be changed.

Switching to any of the tools from the Edit panel will deselect the currently selected transform tool automatically.