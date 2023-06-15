# Object Controller
The Object Controller component instantiates and positions Game Objects along a spline. This could be done both during runtime and in the editor. 

![](./_images/105.png)

The Object Controller has two Object Methods which define how the objects it controls are created. 

- Instantiate – This will instantiate new objects
- Get Children – This will not instantiate new objects but instead will take the existing children of the Object Controller’s Transform and use them.

![](./_images/106.png)

In Instantiate mode, a Game Object array is presented. For Instantiate to work there must be at least one added Game Object reference.

A Spawn Count property is presented. Modifying it will create or destroy objects.

The Iteration dropdown defines in what order the Game Objects are instantiated. If Ordered is selected, the Game Objects will be spawned in the order they are added. To change this order, use the up and down arrows next to the objects. In Random mode, objects will be instantiated in a random order.

If Delayed spawn is turned on, the objects will spawn over a certain period of time. This is mostly used to prevent spike lags caused by spawning many objects at once. Delayed spawn does not work in the editor.

In Get Children mode, the child objects of the Object Controller’s Transform will be used. Adding or removing objects from the hierarchy will include or exclude them from the controller.
## Custom Object Controller Rules
Version 2.12 introduces a way to expand the Object Controller’s functionality in the form of custom rules. These are scriptable objects that change how the objects within an Object Controller are handled. 

Custom rules can be assigned to the offset, the rotation or the scale property of the Object Controller. Once a custom rule is assigned, the inspector GUI for the given section will be replaced by the GUI of the custom rule and the Object Controller will immediately start to draw the placement, rotation or scaling logic from the rule. 



By default, Dreamteck Splines comes with two types of custom rules:

- Sine Rule – orders the objects in a sine wave
- Spiral Rule – orders the objects in a spiral

To create rule objects, go to **Assets/Create/Dreamteck/Splines/Object Controller Rules**

![](./_images/107.png) ![](./_images/108.png)

**Note: Editing the values of the custom rule from the Object Controller’s inspector will change the values inside the custom rule object. If multiple object controllers require different values, separate rule objects need to be created. To make sure rule object values are saved, the project needs to be saved (File/Save Project).**

More custom rules can be easily created by inheriting the **ObjectControllerCustomRuleBase** and overriding the methods. The **ObjectControllerSineRule** and **ObjectControllerSpiralRule** can be used as a reference. When creating new rules, use the MenuItem attribute to make the scriptable object available through the Create menu.