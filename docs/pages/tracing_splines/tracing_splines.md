# Tracing Splines

![](./_images/075.png)

The Spline Tracers are a group of Spline Users, meant specifically for positioning a single object along a spline. This class of Spline Users fosters the Spline Follower component which is one of the most commonly used components when it comes to splines. 

The Spline Tracer component expands on the SplineUser class by adding another set of properties that the rest of the tracers share. 
## Use Triggers
Should this tracer use the triggers provided in the spline. Note that this refers to spline triggers (See [Spline Triggers](/pages/spline_triggers/spline_triggers#spline-triggers)) and not Unity colliders set to act as triggers. Collision and physics are not used here.

If “Use Triggers” is toggled, a Trigger Group field is shown. This is the trigger group that this tracer will be using. 

## Direction
Defines the orientation of the object along the spline (Forward or Backward along the spline). In the case of the Spline Follower component, the direction also defines the direction in which the follower will move.


## Physics Mode
![](./_images/076.png)

The Physics mode of a SplineTracer can be either Transform, Rigidbody and Rigidbody2D. By default, the Physics mode is set to Transform which modifies the property of the Transform directly. Using Rigidbody or Rigidbody2D will modify the rotation and position of a Rigidbody component instead of the Transform. Rigidbody mode is preferred when physical interactions are needed.

Using PhysicsMode Rigidbody with a combination of the proper motion application settings can create realistically behaving followers.


## Motion
![](./_images/077.png)

The motion module defines how the transformation from evaluating the spline is applied to the object. There are three axes for each position, rotation, and scale. When an axis is toggled, transformation will be applied along it. The position and rotation components use world axes while the scale axes are local to the object. 

It has two modes – 3D and 2D – which define how the rotation of the object will be applied. In 2D mode, the provided settings are simplified.

Applying scale is disabled by default meaning that all axes are unchecked. When applying scale is enabled, an additional “Base Scale” Vector3 field is presented. The base scale is the scale that is going to be applied to the object. If the spline has various sizes, the base scale will be multiplied by the size value of the current spline evaluation.

When toggled, Retain Local Rotation and Retain Local Position will attempt to preserve the last offset of the spline tracer object in regard to the spline. These options accumulate an error over time.
### Velocity Mode
If the Physics Mode is set to Rigidbody, this property defines  how the velocity of the Rigidbody will be handled. 

- Zero – rigidbody.velocity will always be zero
- Preserve – velocity will not be handled by the spline tracer
- Align – the velocity’s magnitude will be preserved but the velocity direction will be aligned with the spline
- Align Realistic – aligns the velocity with the spline but also reduces the velocity’s magnitude based on the angle between the direction of the velocity and the spline.


## Spline Follower
As the name suggests, the Spline Follower component provides spline following functionality for a single object.

![](./_images/078.png)

![](./_images/079.png)

The Spline Follower has two modes of following:

- Uniform
- Time

In Uniform mode, the Spline Follower will follow the spline with uniform speed, regardless of how the spline is set up. The Follow Speed property defines the distance in world units the object will travel along the spline in one second.

In Time mode, the “Follow Speed” field turns into the “Follow Duration” and the object moves along the spline by incrementing / decrementing its current spline percent [0-1]. This means that for non-uniform splines where points are closer or further apart, the object will follow with variable speed. The Time follow mode, however, will make sure that the follower has finished following the spline in exactly the time I is given to do so. 

The “Follow” property defines whether or not the Spline Follower will be following the spline automatically. If Follow is disabled, the follower can be caused to follow manually each frame by calling it’s public method **Move(float distance).**

Three wrap modes control the Follower’s behavior when it reaches the end of a spline.:

- Default – when the end of the spline is reached, the follower will stop
- Loop – When the end of the spline is reached, the follower will start again from the beginning.
- PingPong – When the end of the spline is reached, the follower will change its direction

If “Face Direction” is toggled, the follower will face the direction of movement along the spline. If unchecked, the follower will always face the forward direction even if it moves backwards.

![](./_images/080.png)

Where the follower starts along the spline is dictated by the Start position settings. If the “Project” checkbox is checked, the start position will be calculated automatically based on the closest point along the spline to the Follower object in world space.  The Set Distance button opens a small window which allows a distance to be input. This is a distance in world units from the start of the spline and it will be converted to a percent.


## Spline Projector
The Spline projector projects a single game object onto a spline. It will find the nearest point on the spline to the object’s current world position and return the result at that point. By default, the Spline Projector is passive, meaning that it doesn’t apply any results to objects but it can be set to automatically apply the projected result to any object by setting the reference of the Apply Target Object field.

![](./_images/081.png)

The Spline Projector has two modes:

- Accurate
- Cached

By default, the mode is set to Cached which means that the cached samples of the spline will be used. However, this in some cases results in jagged movement if the samples are not enough. This is why the Accurate mode is also introduced. In Accurate mode, the projector re-calculates the spline path from scratch using subdivision.
## Spline Positioner

The simplest Spline Tracer, the Positioner simply places a single object along a spline at a given percent.

The positioner has two modes – Percent and Distance

- Percent maps the whole spline to the range [0-1]
- Distance positions the object at the given distance from the spline’s start

![](./_images/082.png)