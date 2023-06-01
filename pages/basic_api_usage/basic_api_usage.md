# Basic API Usage
To use the Dreamteck Splines API, add a “**using Dreamteck.Splines;”** to the start of the script. From there on, all components, classes and public methods/properties can be accessed.
## Accessing Spline Components in Code
All Spline components derive from the **MonoBehaviour** class and therefore act as regular components which can be get using the GetComponent method.

Once a reference to the given component is obtained, all properties exposed in the editor are available in code. For example, here is how getting a reference to a SplineFollower and setting its follow speed looks like:

```csharp
SplineFollower follower = GetComponent();

follower.followSpeed = 10f;
```

In order to change the spline used by the current user, set the user’s “spline” property to the desired spline:

```csharp
follower.spline = mySplineComputer; //Where mySplineComputer is an object of type SplineComputer
```

Setting the spline through code like that will immediately tell the user to move all of its functionality over to the new spline.
## Creating and Editing Splines in Runtime
Splines are defined by a collection of control points so creating and altering them in runtime comes down to getting and setting points. There are a couple of methods for this:

- **GetPoint** – Gets a point by index
- **GetPoints** – Gets an array of all points in the spline
- **SetPoint** – Sets a point by index
- **SetPoints** – Sets the entire points array of the spline

Here is an example of creating a new spline from scratch:

```csharp
//Add a Spline Computer component to this object
SplineComputer spline = gameObject.AddComponent();

//Create a new array of spline points
SplinePoint[] points = new SplinePoint[5];

//Set each point's properties
for (int i = 0; i < points.Length; i++)
{
    points[i] = new SplinePoint();
    points[i].position = Vector3.forward * i;
    points[i].normal = Vector3.up;
    points[i].size = 1 f;
    points[i].color = Color.white;
}

//Write the points to the spline
spline.SetPoints(points);
```

This is an example of editing the points of an existing spline:

```csharp
SplineComputer spline = gameObject.GetComponent();

//Get the spline's points
SplinePoint[] points = spline.GetPoints();

for (int i = 0; i < points.Length; i++)
{
    //Add random vertical offset to each point
    points[i].position += Vector3.up \ Random.Range(-1f, 1f);
}

spline.SetPoints(points);
```

And this is an example of removing a point:

```csharp
int pointToRemove = 2;
SplineComputer spline = gameObject.GetComponent();
SplinePoint[] points = spline.GetPoints();
SplinePoint[] newPoints = new SplinePoint[points.Length - 1];

for (int i = 0; i < points.Length; i++)
{
    if (i < pointToRemove) newPoints[i] = points[i];
    else if (i > pointToRemove) newPoints[i - 1] = points[i];
}

spline.SetPoints(newPoints);
```

## Evaluating Splines
There are two methods for evaluating a spline at a percent – **Evaluate** and **EvaluatePosition**. EvaluatePosition is lightweight but it only returns a Vector3 position along the spline while Evaluate returns all values that the spline can provide and writes them to a special object called a **SplineSample.** 

For EvaluatePosition, the usage is as simple as calling the method:

```csharp
SplineComputer spline = gameObject.GetComponent();
Vector3 position = spline.EvaluatePosition(0.5);
```

Note that, 0.5 does not necessarily mean that the position will be physically in the middle of the spline, especially if the spline’s sample mode is not set to Uniform. Refer to [Sample Mode ](#_sample_mode)to get a better understanding on how percentages translate to world positions. 6

Evaluate requires a SplineSample object to be created and passed to it in order to work. It is usually a good practice to define the SplineSample in the initial variables and re-use in order to reduce GC. However, in this example, the SplineSample is defined just before Evaluate is called so that it can work when copied.

```csharp
SplineComputer spline = gameObject.GetComponent();
SplineSample sample = new SplineSample();
spline.Evaluate(0.5, sample);
Debug.DrawRay(sample.position, sample.forward, Color.blue, 10f);
Debug.DrawRay(sample.position, sample.up, Color.green, 10f);
Debug.DrawRay(sample.position, sample.right, Color.red, 10f);
```

This example draws the spline result with its forward, up and right vectors.

Note that the Evaluate methods use parameters of type double and not float so when hardcoding percentages, do not add the “f” behind the number. For example **spline.Evaluate(0.5f, sample)** is wrong.

Both methods also have an override where instead of a double percent (0.5 for example), an integer value defining a point index can be provided and this will return the evaluation at the position of that control point. For example **spline.EvaluatePosition(0); spline.EvaluatePosition(1); spline.EvaluatePosition(2);** Will return the positions of points 1, 2 and 3.
## Converting World Units to Spline Percentages
It is possible to find the length of the whole spline or a certain region in world units by using the CalculateLength method. The method uses two parameters, from and to, which are set by default to 0 and 1.

```csharp
Debug.Log(spline.CalculateLength()); //Will return the entire spline’s length
Debug.Log(spline.CalculateLength(0.25, 0.75)); //Will return the length of the given region
```

To find the percent of a spline that corresponds to a certain distance in world units along it, the **Travel** method can be used.

```csharp
double distancePercentFromStart = spline.Travel(0.0, 15f, Spline.Direction.Forward);
double distancePercentFromEnd = spline.Travel(1.0, 15f, Spline.Direction.Backward);
```

The first argument is the start percent, the second holds the distance in world units which will be travelled and the last argument is a direction – forward or backward since Travel can work in both directions.

With all that said, here is an example of how to find the true percent that lies in the physical middle of the spline:

```csharp
SplineComputer spline = gameObject.GetComponent();
float splineLength = spline.CalculateLength();
double travel = spline.Travel(0.0, splineLength / 2f, Spline.Direction.Forward);
Vector3 middle = spline.EvaluatePosition(travel);

Debug.DrawRay(middle, Vector3.up, Color.red, 10f);
```

## Getting Output from Spline Tracers
The Spine Tracer components cave a public property called “result” and it can be used to get information about their current location along the spline. The result property is a read-only SplineSample object that automatically gets updated by all tracers.

For example, in order to get the percentage of a Spline Follower along a spline, it can be done like this:

```csharp
SplineFollower follower = GetComponent();
Debug.Log(follower.result.percent);
```

And this works for the Spline Positioner and Spline Projector as well.
## Writing a custom SplineUser class
Dreamteck Splines’ functionality can be expanded easily by creating new SplineUser components.  For a SplineUser class to be created it needs to derive from the SplineUser base class.  

An empty SplineUser example is provided in the Components folder. It can be used as a template for creating new custom ones. 
### Protected and virtual Methods
Each SplineUser class inherits a set of methods which are automatically called. These methods are automatically called and should be overridden in order to create custom functionality.

SplineUser classes are not supposed to have Update, FixedUpdate and LateUpdate methods. They can have a Start method and an Awake method through overriding however:

```csharp
protected override void Awake()
{
    base.Awake();
    //Code here
}
```

In order to access the generated spline samples inside the custom SplineUser class the protected property “sampleCount” should be used to determine the count of the spline samples and then the GetSample(int index) method to get the sample at any given index. Note that these are the samples within the clip range of the Spline User and changing the clipFrom or clipTo properties will also change the sampleCount and the returned samples by GetSample.

The [Build](#protected-virtual-void-build) method can either run on the main thread or if Multithreading is enabled, it can run on a worker thread and therefore it is important to only have thread-safe code inside. For example, if the spline user generates a mesh, all vertex calculations should happen inside the Build method, using sampleCount and GetSample(), and all mesh operations like setting the vertices for the mesh should happen in [PostBuild](#protected-virtual-void-postbuild).

Here is an example of a simple custom SplineUser which draws all spline samples within its clip range:

```csharp
public class DebugUser : SplineUser
{
    bool isPlaying = false;

    protected override void Awake()
    {
        base.Awake();
        isPlaying = true;
    }

    protected override void Build()
    {
        if (isPlaying) return; //Cannot use Application.isPlaying as it is not thread-safe

        //Should run only in edit mode as Debug.DrawRay is not thread-safe
        for (int i = 0; i < sampleCount; i++)
        {
            SplineSample sample = GetSample(i);
            Debug.DrawRay(sample.position, sample.up * sample.size, sample.color);
        }
    }
}
```

Look at how existing SplineUser classes are implemented in order to get a better understanding of how SplineUser inheritance works.
### Protected virtual void Run()
Run is called automatically on Update, FixedUpdate or LateUpdate based on the Update Method setting of the SplineUser. It’s mainly used for game logic that will run on each update cycle.
### Protected virtual void Build()
Build is called when there has been a change in the spline user. For example if the spline has changed or a property of the SplineUser has been set. Build can either be called from the main thread or from a separate one if multithreading is enabled, therefore it is advised that only thread-safe code is put inside. Make calculations inside Build and then apply them in PostBuild.
### Protected virtual void PostBuild()
PostBuild is always called on the main thread after Build has finished even if multithreading is enabled. It’s used to apply the results, calculated in Build(). For example, if the custom SplineUser will position objects along path. The objects’ positions should be calculated in Build and then applied to the objects’ transforms in PostBuild().
### Rebuilding
The SplineUser class has the Rebuild(bool sampleComputer) method which forces the user to recalculate on the next update cycle.

SplineUsers rebuild automatically when their spline is modified or their properties are changed. That however does not happen when the properties of custom SplineUser classes are modified. In order to make the SplineUser class rebuild, custom setters should be implemented that call the Rebuild method. 

Example:

```csharp
private bool _state = false;

public bool state
{
    get { return _state; }
    set
    {
        if (value != _state) //Only rebuild if the value is different
        {
            _state = value;

            Rebuild(); //Rebuild but don’t resample the spline
        }
    }
}
```

Rebuild is a public method so it can also be called from another object. Rebuilding happens once per update cycle no matter how many times it’s called prior to that. 
### RebuildImmediate
Rebuild waits for the next update cycle so calling it will not yield any changes immediately. In some cases, rebuilding should happen instantly and that’s what **RebuildImmediate** is for. It calls **t**he rebuild sequence regardless of update cycles and will yield results immediately.

Calling **RebuildImmediate** multiple times per update cycle will cause the SplineUser to rebuild as many times as RebuildImmediate is called.