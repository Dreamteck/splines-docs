# Getting Started
Dreamteck Splines is a Spline system and extension for Unity which comes with a collection of tools and components for mesh generation, particle control, object spawning and much more. For a full list of features, refer to [Spline Users](/pages/using_splines/using_splines#using-splines-spline-users) and [Editor Tools](/pages/editor_tools/editor_tools#editor-tools). The tool was initially created to aid our company (Dreamteck Ltd.) in the process of level editing and creating game mechanics and it has been growing ever since it was first created in 2013. Over the years it has been used for many different purposes in many different projects (action games, TCG games, racing games, endless runners, children’s games and more).

Our support team is always at your disposal to help resolve issues at <https://dreamteck.io/support/contact.php>
## Key Features
- Rapid spline [creation](#creating-a-new-spline) and [editing](/pages/editing_splines/editing_splines#editing-splines) via custom editor in Unity.
- Four types of spline: [Hermite](/pages/spline_computer_settings/spline_computer_settings#type), [Bezier](/pages/spline_computer_settings/spline_computer_settings#type), [B-Spline](/pages/spline_computer_settings/spline_computer_settings#type) and [Linear](/pages/spline_computer_settings/spline_computer_settings#type)
- [Following splines with uniform speed](/pages/tracing_splines/tracing_splines#tracing-splines)
- [Extruding geometry along splines](/pages/spline_mesh_generation/spline_mesh_generation#spline-mesh)
- [Creating splines with code](/pages/basic_api_usage/basic_api_usage#creating-and-editing-splines-in-runtime)
- [Procedural primitives](/pages/editing_splines/editing_splines#using-primitives) [and saving presets](/pages/editing_splines/editing_splines#using-presets) for later use
- [Junctions](/pages/nodes/nodes#junctions)
- [Morph states](/pages/morphing_splines/morphing_splines#morphing-splines) 
- Multithreading
- Open source


## Install
![](./pages/getting_started/_images/006.png)

After the Dreamteck Splines package is downloaded from the Asset Store and imported into the project, a welcome window will pop up with information about the current version and additional actions. 

- **What’s new?** – Changelog 
- **Get Started** – Video tutorials, Examples and User Manual
- **Community & Support** – Contact support and Discord links.
- **Rate** – We would really appreciate it if you rate our efforts on the Asset Store.
- **Playmaker Actions**  Install Playmaker actions for Dreamteck Splines
- **Text Mesh Pro Support** – Install components for working with TextMeshPro

**To get the example scenes, go to Get Started and click the Examples tab. The examples package will be imported in the project under Dreamteck/Splines/Examples**
## Updating Dreamteck Splines
Always make a backup of the project before updating to a newer version of any plugin.

If a previous version of Dreamteck Splines is installed, it is recommended that the previous version is removed before updating. Before deleting the current Dreamteck Splines version, make sure that the Dreamteck/Splines/Presets folder doesn’t contain any custom presets and if it does, make a backup of them.
## Preferences configuration
To access the Preferences screen, go to **Edit/Preferences/Dreamteck/Splines**:

![](./pages/getting_started/_images/007.png)

The options under “Newly created splines” define the default settings for each newly created spline. For example, if the game scene is full of light colors, then the default spline color could be set to black and that way each newly created spline will be black upon creation without the need of setting the color every time.

The options under “Newly created points” define the size and color of newly created points.

The options under the “Editor” section let the user customize the UI and scene view of the editor.

## Creating a new Spline
To create a new spline object in the scene, go to **GameObject/3D Object/Spline Computer**. This will create and select a new Game Object called “Spline” with a **Spline Computer** component. If the “Start in Creation Mode” setting is toggled in the preferences, the spline will be automatically put in point creation mode and a green grid will follow the mouse inside the scene. Clicking inside the scene view will create new spline points. To exit point creation mode, go to the Spline Computer’s inspector and click the plus button inside the Edit tab.



## Adding control points
![](./pages/getting_started/_images/008.png)

A spline needs at least two control points in order to visualize. To add control points, click the Plus button inside the Edit tab in the Spline Computer’s inspector. This will enter point creation mode. While in point creation mode, clicking inside the scene creates spline points.

The point creation mode has a couple of properties which define how points are created and added to the spline.
### Placement Mode
Defines where the point will be created. 

- Y Plane – Creates points on the World-Y plane

![](./pages/getting_started/_images/009.png)

- X Plane – Creates points on the World-X plane

![](./pages/getting_started/_images/010.png)

- Z Plane – Creates points on the World-Z plane

![](./pages/getting_started/_images/011.png)



- Camera Plane - Y Plane – Creates points on the editor camera’s plane.

![](./pages/getting_started/_images/012.png)

- Surface – Creates points on any surface which has a 3D Collider (uses Raycasting)

![](./pages/getting_started/_images/013.png)

- Insert – Inserts a point in between two other spline points

All placement modes have an accompanying offset property. For the world plane methods, it is called the “Grid Offset” and for the camera plane it’s called “Far Plane”. In surface mode it is called “Surface offset”. This property adds offset along the given creation method’s normal.
### Normal Mode
Defines where the new points’ normals will point at.

- Default – Will use the default normal for the selected placement method. For example, Y Plane will make the normal point upwards.
- Look at Camera – The normal will look towards the editor camera’s position
- Align with Camera – The normal will align with the editor camera’s direction
- Calculate – The normal will automatically be calculated based on the location of the previous control points
- Left – World Negative X
- Right – World Positive X
- Up – World Positive Y
- Down – World Negative Y
- Forward – World Positive Z
- Back – World Negative Z

### Append To
Defines to which end of the spline the new points will beaded.

- Beginning – Will add the point before the first spline point
- End – Will add the point after the last spline point

### Add Node
When toggled, a new Node object will be created and bound to each new created point. Nodes are game objects which are linked to spline points and update the points when they are moved in the scene. A more detailed explanation is given in the Nodes chapter.

## Selecting points
To select a point either click on it in the scene view or select it from the inspector using the “Select” drop down menu inside the Edit tab.

When a point is selected, its parameters will be displayed in the inspector:

![](./pages/getting_started/_images/014.png)

Selected points are highlighted in blue by default and are drawn with an additional hollow circle surrounding them:

![](./pages/getting_started/_images/015.png)
### Selecting multiple points
![](./pages/getting_started/_images/016.png)

To select multiple points either drag-select them in the scene view or select them one-by-one while holding Ctrl on the keyboard.

- Pressing Ctrl + A while in the scene view, will select all points inside the spline.





## Deleting points
Points can be deleted by selecting them and pressing Delete key on the keyboard or by entering Delete point mode by clicking on the Minus button inside the Edit panel.

![](./pages/getting_started/_images/017.png)

![](./pages/getting_started/_images/018.png)

The Delete Mode toggles the delete brush tool. The brush tool has a radius in which it looks for points. Clicking and holding while dragging along the scene view will delete any spline points that fall under the delete brush’ range.
## Undo
Any action can be undone using Unity’s undo (Ctrl+Z) or re-done (Ctrl+Y).