# Spline Triggers
The spline triggers are a way to call events when an object reaches a certain point along the spline. Spline Triggers are not physical collider triggers but rather a percent along the spline [0-1] which when reached or passed, will call an event.

Currently, triggers are supported only by the Spline Tracer components (Follower, Positioner and Projector) but trigger-checking functionality can be easily implemented by calling **SplineComputer.CheckTriggers** and providing the start percent along the spline and the end percent. If there are any triggers that fall between these two values, they will get invoked.
## Creating Triggers
![](./_images/115.png)

The trigger panel is located below the “Spline Computer” panel of the Spline Computer component. Expanding it reveals a single button, called “New Group”. Clicking the button will create a new trigger group with index 0.

After a group is created, triggers can be added inside it by pressing the “Add Trigger” button. 

When a trigger is created, it is added to the trigger list under the trigger group. Clicking on the trigger’s name will open the trigger’s panel:

![](./_images/116.png)

![](./_images/117.png)

Inside the trigger panel, several properties can be set:

- Enabled – is the trigger active?
- Color – Color of the trigger for rendering it in the scene view
- Position – the position in percent [0-1] of the trigger along the spline
- Type – The direction the trigger works in: forward, backward or double (works in both directions).
- Work once – The trigger will work only the first time it is crossed

The On Cross event is the event that gets fired when the trigger is crossed.
## Managing and Editing Triggers
When a trigger group is expanded in the inspector, its triggers will be drawn in the scene.

![](./_images/118.png) ![](./_images/119.png)

Dragging the trigger handles along the spline in the scene view will set their position. 

Triggers and trigger groups can be renamed and deleted by right clicking on their names in the inspector and choosing the corresponding option.
## Using Triggers
The Spline Tracer components like the Spline Follower for example, all have the “Use Triggers” option inside the inspector. When “Use Triggers” is checked, an additional field for the trigger group is presented. When the tracer moves along the spline, it will check all triggers from the given group and if a trigger is crossed, its event will be invoked.


![](./_images/120.png)

Currently, Spline Tracers can use only one trigger group but in future updates using multiple groups will be added.
### Using Triggers from Code
To use the triggers from code, first a reference of the Spline Computer should be present:

```csharp
SplineComputer spline = GetComponent();
```

From here, all triggers from all trigger groups can be checked like this:

```csharp
spline.CheckTriggers(0.0, 0.5);
```

This will invoke all triggers which are in the range [0-0.5] and their type is set to either Forward or Double. To check triggers of type Backward, switch the position of the values:


```csharp
spline.CheckTriggers(0.5, 0.0);
```

If there are triggers, set to work once, they can be reset by calling:

```csharp
spline.ResetTriggers();
```

To check and reset triggers only from a given trigger group, the “triggerGroups” array needs to be used:

```csharp
spline.triggerGroups[0].Check(0.0, 0.5);

spline.triggerGroups[0].Reset();
```

This will check and reset all triggers in group 0.

Individual triggers can also be changed through code by modifying the same properties, exposed in the inspector:

```csharp
spline.triggerGroups[0].triggers[0].name = "Explosion";

spline.triggerGroups[0].triggers[0].position = 0.75;
```