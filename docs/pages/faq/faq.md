# Frequently Asked Questions
This is a list of questions that our studio gets asked on a daily basis. We highly suggest checking these out to new users as this might save some time in waiting for a response from our support.
## How do I Sample / Evaluate a Spline in Code?
To evaluate a spline, use the SplineComputer.Evaluate or SplineComputer.EvaluatePosition methods. Find more in-depth usage and examples in the [Basic API Usage](/pages/basic_api_usage/basic_api_usage#basic-api-usage) chapter.
## Can I create and edit splines in runtime with code?
Yes! In fact, everything that is possible in the editor, can be done with code as well since the Editor uses the same API that is exposed during runtime. Examples of creating splines with code can be found in the [Basic API Usage](/pages/basic_api_usage/basic_api_usage#basic-api-usage) section.
## How to get the percent of a Spline Follower along a path?
The Spline Follower, Spline Projector and Spline Positioner all provide the **result** property which holds information about the current spline traversal. To get the percent, use:

```csharp
SplineFollower follower = GetComponent();
double percent = follower.result.percent;
```

## How to distribute objects evenly along a spline using the Object Controller
The default behavior of all splines is such that they stretch and squash when control points are further apart or closer together (refer to [Sample Mode](/pages/spline_computer_settings/spline_computer_settings#sample-mode) for a better explanation). To mitigate this effect, either make sure to place all control points at even distances (see [Distribute Evenly](/pages/editing_splines/editing_splines#distribute-evenly)) or use a Uniform sample mode for the Spline Computer.
## How to find the middle of the spline
Calling **Evaluate** with a percent of 0.5 will not work unless all control points are distributed evenly. In order to find the middle of the spline, use **CalculateLength** and **Travel**. Example [here](/pages/basic_api_usage/basic_api_usage#converting-world-units-to-spline-percentages).
## Can I use Dreamteck Splines to create an endless runner game?
Yes, in fact, a lot of endless runner games have already been made using Dreamteck Splines. However, creating an endless runner game with this package will require some additional coding and this is why we have released [Forever](https://assetstore.unity.com/packages/templates/systems/forever-endless-runner-engine-140926) – it uses Dreamteck Splines as a base to create endless runner games effortlessly.
## How to move along a spline in world units instead of percent?
Incrementing a percent with another percent will not move along the spline at even distances and therefore the Travel method should be used in such cases. Example [here](/pages/basic_api_usage/basic_api_usage#converting-world-units-to-spline-percentages).
## I'm using the Spline Mesh component and the extruded mesh does not perfectly follow the spline or is too edgy. What is going on?
The Spline Mesh component extrudes the provided meshes along the spline without creating additional geometry. This means that if you are trying to extrude a single cube mesh along a spline, the start and end vertices will simply stretch at the both ends of the spline because there are no vertices in the middle which can be positioned along the spline:

![](./_images/155.png)

No matter how smooth the spline is, if the mesh does not have enough detail, it will be jaggy.

To fix this, either increase the repetition count of the mesh in the channel or if this isn’t an option, add more vertices in between the two ends of the mesh in the modeling program.
## I’m using a mesh generator to generate a big mesh but the mesh does not generate. What is going on?
Unity has a limit of 64,000 vertices per mesh object. If the mesh exceeds this limit, it cannot exist and therefore the mesh generators will stop updating the geometry. 

In such cases, consider splitting the geometry into two separate objects. Create two game objects and add the same Mesh generator component to both. Have both use the same spline and set the clip range of the first Mesh Generator to [0-0.5] and the clip range of the second Mesh Generator to [0.5-1].
## How can I change the speed of the Spline Follower component during runtime?

```csharp
SplineFollower follower = GetComponent();
follower.followSpeed = 10f;
```

## No matter where I instantiate a Spline Computer, the spline appears always in the same place. What is happening?
Double check if the Spline Computer’s space is not set to World instead of Local and make sure the Spline Computer is set to rebuild on awake from the Spline Computer panel.