# Editor Tools
The Spline User is a component that is mostly meant to work in runtime although it can be configured in the editor. Sometimes, however, level designers just need a tool to help them in the work process. For pure level design purposes, Dreamteck Splines offers the Spline Tools window which is found in **Window/Dreamteck/Splines/Tools.**

![](./_images/145.png)

Each tool can be selected by clicking the corresponding button.  Some tools require an object with a Spline Computer to be selected in the editor in order to work. Otherwise a warning will be displayed:

![](./_images/146.png)

If this warning appears, go to the scene’s hierarchy, select a Spline Computer and click on the tools window again. This will update it and the warning will disappear.
## Mass Baking Spline-generated Meshes
If a lot of Mesh Generators need to be baked simultaneously, the Bake Meshes tool can help with that. It does not need a Spline Computer to be selected in order to work and can be opened right away.

![](./_images/147.png)

There are three bake modes:

- All - Bakes all Mesh Generators in the scene
- Selected - Bakes only the selected Generators
- All Excluding - Bakes all Generators except for the selected ones

![](./_images/148.png)

If “Selected” or “All Excluding” is selected, the Bake Tool will provide two panels – Available and Selected:

Inside the Available panel, a list of all Mesh Generators which are available to bake is displayed. Clicking the “+” button will add the given mesh generator to the selected panel. When “Bake” is clicked, all Mesh Generators inside the “Selected” panel will be baked or excluded from baking depending on the Bake Mode.
## Leveling terrain
The Level Terrain tool is an editor-only tool meant for sculpting terrains using splines.

For the Level Terrain tool to work, the scene needs to have at least one terrain and a Spline Computer needs to be selected. If the scene has more than one terrain, the terrain that should be leveled should also be selected along with the Spline Computer. 

![](./_images/149.png)

Once open, the Level Terrain tool will display a preview of the terrain’s heightmap, the path heightmap which will be black and a preview of the brush feather. 

The tool has only three settings:

- Brush Radius – the radius of the brush in world units
- Feather – the feather of the brush
- Height Offset – height offset

To level the terrain using the selected spline(s), click the “Level” button. The path heightmap will be drawn and the terrain will be sculpted.

![](./_images/150.png)

An “Apply” and “Revert” buttons will appear under the heightmap previews.  If the result is not satisfying, edit the spline and the settings and click Level again – the previous level will get replaced with the new one. To save the terrain, click “Apply” – this will write the level data to the terrain permanently.

The brush radius is affected by the sizes of the points.
## Import/Export
Splines can be imported and exported from and to other software using the Import/Export tool. The currently supported formats are SVG and CSV datasets. 

![](./_images/151.png)
### Importing
To import an SVG or a CSV file click the “Import” button. This will open a new file browser. Navigating to the desired SVG or CSV file and clicking “Open” will instantly import the splines from the file in the Unity scene.

**NOTE: Splines from SVG files are usually very big, about the size of a Canvas or even bigger. If the imported SVG spline isn’t visible after import, make sure to zoom-out further in the scene.**

After the spline has been imported, a new object with the name of the file will be created in the scene and the import tool’s panel will display some importing options:

![](./_images/152.png)

- Scale Factor – This is the scale factor of the imported graphic. A scale factor of 1 will retain the size of the graphic.
- Facing axis – The orientation of the imported graphic. By default it is Z which means no rotation
- Always draw – Should the splines be drawn in the scene constantly after closing the import tool?

### Exporting
To export one or more splines, the splines first have to be selected in the scene. If no splines are selected in the scene, the export button will be unavailable.

When the export mode is toggled, a Format dropdown menu will be available, allowing the user to select between SVG and CSV.
#### ***Exporting SVG***
Since the SVG is a 2D vector format, when a spline is exported to SVG it is flattened.

![](./_images/153.png)

The projection axis dropdown menu defines the axis along which the spline will be flattened and projected onto the SVG canvas.
#### ***Exporting CSV***
The CSV export creates a new Excel file with each spline point’s data written on each row.

![](./_images/154.png)

The Dataset columns field allows for customization of the columns that will be written in the CSV file. For example, if only the points’ positions are needed, all columns except the position column should be removed.

CSV Export also supports flattening. If “Flat” is toggled, a projection axis dropdown menu is presented.