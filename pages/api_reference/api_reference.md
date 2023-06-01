# API Reference
This chapter lists the main classes in Dreamteck Splines along with their public and protected properties and methods.
## SplinePoint
A spline control point used by the Spline class.
### Enums

|**Type { SmoothMirrored, Broken, SmoothFree };**|<p>- **SmoothMirrored – Mirrors both tangents**</p><p>- **Broken – Each tangent moves independently**</p><p>- **SmoothFree – Tangent direction is mirrored but length is independent**</p>|
| :- | :- |

1. ### Public Properties

|**SplinePoint.Type type**|**The type of the spline point**|
| :- | :- |
|**Vector3 position**|**The position of the point**|
|**Vector3 normal**|**The normal of the point**|
|**Vector3 tangent**|**The first tangent of the point**|
|**Vector3 tangent2**|**The second tangent of the point**|
|**Color color**|**The color of the point**|
|**float size**|**The size of the point**|

### Public Methods

|**void SetPosition(Vector3 pos)**|**Set the point’s position (updates the tangents automatically)**|
| :- | :- |
|**void SetTangentPosition(Vector3 pos)**|**Set the position of the first tangent (updates the other tangent automatically based on the point type)**|
|**void SetTangent2Position(Vector3 pos)**|**Set the position of the second tangent (updates the other tangent automatically based on the point type)**|

### Static Methods

|**SplinePoint Lerp(SplinePoint a, SplinePoint b, float t)**|**Interpolates between two spline points**|
| :- | :- |
|**bool AreDifferent(ref SplinePoint a, ref SplinePoint b)**|**Returns true if any of the points’ values are different**|



## SplineSample

### Public Properties

|**Vector3 position**|**The position of the sample**|
| :- | :- |
|**Vector3 up**|**The up direction of the sample (based on the spline normal)**|
|**Vector3 forward**|**The forward direction of the sample (based on the spline direction)**|
|**Vector3 right**|**A perpendicular vector to up and forward**|
|**Color color**|**The color of the sample**|
|**float size**|**The size of the sample**|
|**double percent**|**The percent along the spline of the sample**|
|**Quaternion rotation**|**A quaternion rotation, calculated from the direction vectors of the sample**|

### Public Methods

|**void Lerp(ref SplineSample b, double t)**|**Interpolates the sample towards sample “b” with a percent t**|
| :- | :- |
|**void Lerp(ref SplineSample b, float t)**|**Interpolates the sample towards sample “b” with a percent t**|
|**void FastCopy(ref SplineSample input)**|**Copies the properties of another sample into the current sample, faster than the = operator**|

### Static Methods

|**SplineSample Lerp(ref SplineSample a, ref SplineSample b, float t)**|**Interpolates between two spline results with time t and returns the result**|
| :- | :- |
|**SplineSample Lerp(ref SplineSample a, ref SplineSample b, double t)**|**Interpolates between two spline results with time t and returns the result**|
|**void Lerp(ref SplineSample a, ref SplineSample b, float t, ref SplineSample target)**|**Interpolates between a and b and writes the result to target**|
|**void Lerp(ref SplineSample a, ref SplineSample b, double t, ref SplineSample target)**|**Interpolates between a and b and writes the result to target**|


## Spline
### Enums

|**Direction { Forward = 1, Backward = -1 }**|**Traversing direction of the spline.**|
| :- | :- |
|**Type { Hermite, BSpline, Bezier, Linear };**|**Type of the spline as described in the documentation**|

### Public Properties

|**Spline.Type type**|**The type of the spline**|
| :- | :- |
|**SplinePoint[] points**|**The control points array which make up the spline**|
|**int sampleRate**|**The sample rate of the spline – the bigger, the smoother**|
|**AnimationCurve customValueInterpolation**|**Custom curve for size and color interpolation**|
|**AnimationCurve customNormalInterpolation**|**Custom curve for normal interpolation**|
|**bool linearAverageDirection**|**Should sample directions be averaged in linear mode?**|
|**bool isClosed**|**Is the spline closed (read only)?**|
|**double moveStep**|**The average percent distance between spline samples based on the sampleRate**|
|**int iterations**|**The total count of samples the spline is expected to have based on sampleRate and point count**|
|**float knotParametrization**|**Value between 0 and 1 which governs the shape of the spline when the type is CatmullRom**|

### Public Methods

|**float CalculateLength(double from = 0.0, double to = 1.0, double resolution = 1.0)**|**Calculates the length in world units between two points along the spline**|
| :- | :- |
|**double Project(Vector3 position, int subdivide = 4, double from = 0.0, double to = 1.0)**|**Finds the closest point along the spline to a world point and returns its percent value**|
|**bool Raycast(out RaycastHit hit, out double hitPercent, LayerMask layerMask, double resolution = 1.0, double from = 0.0, double to = 1.0, QueryTriggerInteraction hitTriggers = QueryTriggerInteraction.UseGlobal)**|**Raycasts along the spline and returns the first hit**|
|**bool RaycastAll(out RaycastHit[] hits, out double[] hitPercents, LayerMask layerMask, double resolution = 1.0, double from = 0.0, double to = 1.0, QueryTriggerInteraction hitTriggers = QueryTriggerInteraction.UseGlobal)**|**Raycasts along the spline and returns all hits**|
|**double GetPointPercent(int pointIndex)**|**Returns the percent along a spline of a point based on its index**|
|**Vector3 EvaluatePosition(double percent)**|**Evaluates the spline at the provided percent and returns the world position at that percent**|
|**SplineSample Evaluate(double percent)**|**Evaluates the spline at the provided percent and returns the spline sample at that percent**|
|**SplineSample Evaluate(int pointIndex)**|**Evaluates the spline at a given control point and returns the spline sample at that point**|
|**void Evaluate(int pointIndex , ref SplineSample result)**|**A version of Evaluate which does not allocate new objects**|
|**void Evaluate(double percent , ref SplineSample result)**|**A version of Evaluate which does not allocate new objects**|
|**void Evaluate(ref SplineSample[] samples, double from = 0.0, double to = 1.0)**|**Evaluates the spline throughout the provided range and writes all samples to the provided sample array**|
|**void EvaluateUniform(ref SplineSample[] samples, ref double[] originalSamplePercents, double from = 0.0, double to = 1.0)**|**Evaluates the spline throughout the provided range and writes all samples to the provided sample array. The spline is sampled at uniform distances.**|
|**void EvaluatePositions(ref Vector3[] positions, double from = 0.0, double to = 1.0)**|**Evaluates the spline throughout the provided range and writes all positions to the provided sample array**|
|**double Travel(double start, float distance, out float moved, Direction direction)**|**Moves along a spline using world units and returns the new percent**|
|**double Travel(double start, float distance, Spline.Direction direction = Spline.Direction.Forward)**|**Moves along a spline using world units and returns the new percent**|
|**void EvaluatePosition(double percent, ref Vector3 point)**|**Evaluates the spline at the provided percent and writes the world position at that percent to the referenced field**|
|**void Close()**|**Closes the spline**|
|**void Break()**|**Breaks (opens) the spline at the closed point**|
|**Break(int at)**|**Breaks (opens)  the spline at a specific point**|
|**void HermiteToBezierTangents()**|**Converts a Hermite spline into a Bezier spline**|

### Static Methods

|**void FormatFromTo(ref double from, ref double to, bool preventInvert = true)**|**Used for formatting a from and to percent so that they can be safely used in internal spline methods**|
| :- | :- |


## SampleCollection
### Public Properties

|**SplineSample[] samples**|**The array of spline samples this collection holds**|
| :- | :- |
|**SplineComputer.SampleMode sampleMode**|**The sample mode these samples have been calculated with**|
|**int Count**|**The count of the samples**|
|**int[] optimizedIndices**|**Index mapping array for optimized samples only**|
|**double clipFrom = 0.0**|**The start region the samples are clipped from**|
|**double clipTo = 0.0**|**The end region the samples are clipped to**|
|**bool loopSamples**|**Should the samples looped (closed)?**|
|**bool samplesAreLooped**|**Are the samples successfully looped?**|
|**double span**|**The total spline range [0-1] of the samples.**|

### Public Methods

|**int GetClippedSampleCount(out int startIndex, out int endIndex)**|**Returns the number of samples which fall inside the clip range. Based on the clip range and the samples array.**|
| :- | :- |
|**double ClipPercent(double percent)**|**Converts a global percent [0-1] into a clip range percent [0-1]**|
|**void ClipPercent(ref double percent)**|**Converts a global percent [0-1] into a clip range percent [0-1]**|
|**double UnclipPercent(double percent)**|**Converts a clip range percent into a global percent**|
|**void UnclipPercent(ref double percent)**|**Converts a clip range percent into a global percent**|
|**void GetSamplingValues(double percent, out int sampleIndex, out double lerp)**|**Returns the sample index and the interpolation value towards the next sample based on the provided percent**|
|**Vector3 EvaluatePosition(double percent)**|**Evaluates the samples at the provided percent and returns the world position at that percent**|
|**SplineSample Evaluate(double percent)**|**Evaluates the samples at the provided percent and returns a spline sample at that percent**|
|**void Evaluate(double percent, ref SplineSample result)**|**A version of Evaluate which does not allocate new objects**|
|**void Evaluate(ref SplineSample[] results, double from = 0.0, double to = 1.0)**|**Evaluates the samples throughout the provided range and writes all samples to the provided sample array**|
|**void EvaluatePositions(ref Vector3[] positions, double from = 0.0, double to = 1.0)**|**Evaluates the samples throughout the provided range and writes all positions to the provided sample array**|
|**double Travel(double start, float distance, Spline.Direction direction, out float moved)**|**Moves along a sample collection using world units and returns the new percent**|
|**double Travel(double start, float distance, Spline.Direction direction = Spline.Direction.Forward)**|**Moves along a sample collection using world units and returns the new percent**|
|**void Project(Vector3 position, int controlPointCount, SplineSample result, double from = 0.0, double to = 1.0)**|**Finds the closest point along the collection to a world point and returns its percent value**|
|**float CalculateLength(double from = 0.0, double to = 1.0)**|**Calculates the length in world units between two points along the spline**|


## SplineComputer
### Enums

|**Space { World, Local };**|<p>- **World space does not take the Spline’s transform into account**</p><p>- **Local transforms the spline using the transform**</p>|
| :- | :- |
|**EvaluateMode { Cached, Calculate }**|<p>- **Cached uses the cached spline samples to operate**</p><p>- **Calculate recalculates the spline for better precision at the cost of performance**</p>|
|**SampleMode { Default, Uniform, Optimized }**|` `**Sample mode as described in the documentation**|
|**UpdateMode { Update, FixedUpdate, LateUpdate, AllUpdate, None }**|` `**Which update method should the Spline Computer use? None does not update the spline and waits for manual update using the Rebuild method**|

### Public Properties

|**SplineComputer.Space space**|**The transform space of the Spline Computer**|
| :- | :- |
|**Spline.Type type**|**The type of the spline**|
|**bool linearAverageDirection**|**Should sample directions be averaged in linear mode?**|
|**bool is2D**|**Toggle 2D mode for the spline**|
|**int sampleRate**|**The sample rate of the spline – the bigger, the smoother**|
|**float optimizeAngleThreshold**|**The angle below which samples get deleted when using Optimized sample mode**|
|**SampleMode sampleMode**|**The sample mode of this Spline Computer as described in the documentation**|
|**bool multithreaded**|**Toggle multithreading for the Spline Computer**|
|**bool rebuildOnAwake**|**Should the spline rebuild as soon as it is awake in the scene?**|
|**SplineComputer.UpdateMode updateMode**|**The update mode of the Spline Computer**|
|**TriggerGroup[] triggerGroups**|**A collection of triggers used by other components to invoke events**|
|**AnimationCurve customValueInterpolation**|**Custom curve for size and color interpolation**|
|**AnimationCurve customNormalInterpolation**|**Custom curve for normal interpolation**|
|**int iterations**|**The total count of samples the spline is expected to have based on sampleRate and point count**|
|**double moveStep**|**The average percent distance between spline samples based on the sampleRate**|
|**bool isClosed**|**Is the spline closed?**|
|**int pointCount**|**Returns the control point count of the spline (read only)**|
|**SplineSample[] samples**|**Returns the computed and transformed samples along the whole spline**|
|**int sampleCount**|**Returns the count of the computed samples (read only)**|
|**SplineSample[] rawSamples**|**Returns the raw samples before transformation**|
|**Vector3 position**|**Thread-safe position of the Spline Computer’s transform component**|
|**Quaternion rotation**|**Thread-safe rotation of the Spline Computer’s transform component**|
|**Vector3 scale**|**Thread-safe scale of the Spline Computer’s transform component**|
|**int subscriberCount**|**Returns the number of SplineUsers that are currently using this Spline Computer**|
|**float knotParametrization**|**Value between 0 and 1 which governs the shape of the spline when the type is CatmullRom**|

### Public Methods

|**void GetSamples(SampleCollection collection)**|**Writes the calculated samples into the passed SampleCollection object**|
| :- | :- |
|**void ResampleTransform()**|**Resamples the transform so that samples can be properly transformed**|
|**bool IsSubscribed(SplineUser user)**|**Checks if a Spline User is subscribed to the given computer**|
|**SplineUser[] GetSubscribers()**|**Returns all subscribed Spline Users**|
|**SplinePoint[] GetPoints(Space getSpace = Space.World)**|**Returns all spline points of the Spline Computer**|
|**SplinePoint GetPoint(int index, Space getSpace = Space.World)**|**Returns the point at the given index**|
|**Vector3 GetPointPosition(int index, Space getSpace = Space.World)**|**Returns the position of the point at the given index**|
|**Vector3 GetPointNormal(int index, Space getSpace = Space.World)**|**Returns the normal of the point at the given index**|
|**Vector3 GetPointTangent(int index, Space getSpace = Space.World)**|**Returns the tangent of the point at the given index**|
|**Vector3 GetPointTangent2(int index, Space getSpace = Space.World)**|**Returns the second tanget of the point at the given index**|
|**float GetPointSize(int index, Space getSpace = Space.World)**|**Returns the size of the point at the given index**|
|**Color GetPointColor (int index, Space getSpace = Space.World)**|**Returns the color of the point at the given index**|
|**void SetPoints(SplinePoint[] points, Space setSpace = Space.World)**|**Sets the points of the Spline Computer to the ones from the array**|
|**void SetPointPosition(int index, Vector3 pos, Space setSpace = Space.World)**|**Sets the position of the point at the given index**|
|**void SetPointTangents(int index, Vector3 tan1, Vector3 tan2, Space setSpace = Space.World)**|**Sets the tangents of the point at the given index**|
|**void SetPointNormal(int index, Vector3 nrm, Space setSpace = Space.World)**|**Sets the normal of the point at the given index**|
|**void SetPointSize(int index, float size)**|**Sets the size of the point at the given index**|
|**void SetPointColor(int index, Color color)**|**Sets the color of the point at the given index**|
|**void SetPoint(int index, SplinePoint point, Space setSpace = Space.World)**|**Sets the point at the given index**|
|**double GetPointPercent(int pointIndex)**|**Returns the percent of a point along the spline based on its index**|
|**int PercentToPointIndex(double percent, Spline.Direction direction = Spline.Direction.Forward)**|**Finds the point index that corresponds to the given percent along the spline**|
|**Vector3 EvaluatePosition(double percent)**|**Evaluates the spline at the provided percent and returns the world position at that percent**|
|**Vector3 EvaluatePosition(double percent, EvaluateMode mode = EvaluateMode.Cached)**|**Evaluates the spline at the provided percent and returns the transformed position at that percent**|
|**Vector3 EvaluatePosition(int pointIndex, EvaluateMode mode = EvaluateMode.Cached)**|**Evaluates the spline at the given control point and returns the transformed position at that percent**|
|**SplineSample Evaluate(double percent)**|**Evaluates the spline at the provided percent and returns the transformed sample at that percent**|
|**SplineSample Evaluate(double percent, EvaluateMode mode = EvaluateMode.Cached)**|**Evaluates the spline at the provided percent and returns the transformed sample at that percent**|
|**SplineSample Evaluate(int pointIndex)**|**Evaluates the spline at the given control point and returns the transformed sample at that percent**|
|**void Evaluate(int pointIndex, ref SplineSample result)**|**Evaluates the spline at the given control point and writes the result to the provided sample**|
|**void Evaluate(double percent, ref SplineSample result)**|**Evaluates the spline at the provided percent and writes the result to the provided sample**|
|**void Evaluate(double percent, ref SplineSample result, EvaluateMode mode = EvaluateMode.Cached)**|**Evaluates the spline at the provided percent and writes the result to the provided sample**|
|**void Evaluate(ref SplineSample[] results, double from = 0.0, double to = 1.0)**|**Evaluates the spline throughout the provided range and writes all samples to the provided sample array**|
|**void EvaluatePositions(ref Vector3[] positions, double from = 0.0, double to = 1.0)**|**Evaluates the spline throughout the provided range and writes all sampled positions to the provided vector array**|
|**double Travel(double start, float distance, out float moved, Spline.Direction direction = Spline.Direction.Forward)**|**Moves along the spline based on a start percent and a distance in world units and returns the new percent at the reached position as well as the moved distance**|
|**double Travel(double start, float distance, Spline.Direction direction = Spline.Direction.Forward)**|**Moves along the spline based on a start percent and a distance in world units and returns the new percent at the reached position**|
|**void Project(Vector3 worldPoint , ref SplineSample result, double from = 0.0, double to = 1.0, EvaluateMode mode = EvaluateMode.Cached, int subdivisions = 4)**|**Projects a point in world space onto the spline and returns the percent  along the spline that resembles the closest sample to that point**|
|**float CalculateLength(double from = 0.0, double to = 1.0)**|**Calculates the distance in world units of the provided range**|
|**void Rebuild(bool forceUpdateAll = false)**|**Recalculates the Spline Computer and all of its subscribers (Spline Users) on the next frame**|
|**void RebuildImmediate(bool calculateSamples = true, bool forceUpdateAll = false)**|**Recalculates the Spline Computer and all of its subscribers (Spline Users) immediately**|
|**void Close()**|**Closes the spline (at least 4 control points required)**|
|**void Break()**|**Breaks the spline**|
|**void Break(int at)**|**Breaks the spline at a specific control point**|
|**void HermiteToBezierTangents()**|**Converts a Hermite spline into a Bezier spline**|
|**bool Raycast(out RaycastHit hit, out double hitPercent, LayerMask layerMask, double resolution = 1.0, double from = 0.0, double to = 1.0 , QueryTriggerInteraction hitTriggers = QueryTriggerInteraction.UseGlobal)**|**Raycasts along the spline and returns hit information**|
|**bool RaycastAll(out RaycastHit[] hits, out double[] hitPercents, LayerMask layerMask, double resolution = 1.0, double from = 0.0, double to = 1.0, QueryTriggerInteraction hitTriggers = QueryTriggerInteraction.UseGlobal)**|**Raycasts along the spline and returns all objects that have been hit**|
|**void CheckTriggers(double start, double end)**|**Checks and invokes any triggers inside the start – end range**|
|**void CheckTriggers(int group, double start, double end)**|**Checks and invokes only the triggers inside the given trigger group inside the start – end range**|
|**void ResetTriggers()**|**Resets all triggers that have been set to work once**|
|**void ResetTriggers(int group)**|**Resets only the triggers inside the given trigger group that have been set to work once**|
|**List<Node.Connection> GetJunctions(int pointIndex)**|**Returns the available junctions for the given point**|
|**Dictionary<int, List<Node.Connection>> GetJunctions(double start = 0.0, double end = 1.0)**|**Returns all junctions for the points in the given range**|
|**void ConnectNode(Node node, int pointIndex)**|**Connect the referenced Node to the spline point**|
|**void DisconnectNode(int pointIndex)**|**Disconnects the connected node at the given point**|




## SplineUser
### Enums

|**UpdateMethod { Update, FixedUpdate, LateUpdate }**|
| :- |

### Public Properties

|**SplineComputer spline**|**The spline computer that this user is using**|
| :- | :- |
|**double clipFrom**|**The start range of the user**|
|**double clipTo**|**The end range of the user**|
|**bool autoUpdate**|**Should this user update automatically? If not, Rebuild must be called manually**|
|**bool loopSamples**|**Should samples be looped (only works for closed splines)?**|
|**double span**|**The span of the user’s clip range (read-only)**|
|**bool samplesAreLooped**|**Returns true if the spline is closed, loopSamples is true and the clipFrom is bigger than clipTo**|
|**RotationModifier rotationModifier**|**The rotation modifier module of the spline user**|
|**OffsetModifier offsetModifier**|**The offset modifier module of the spline user**|
|**ColorModifier colorModifier**|**The color modifier module of the spline user**|
|**SizeModifier sizeModifier**|**The size modifier module of the spline user**|
|**bool multithreaded**|**Should multithreading be used?**|
|**bool buildOnAwake**|**Should the user rebuild on Awake?**|
|**bool buildOnEnable**|**Should the user rebuild on Enable?**|

### Public Methods

|**SplineSample GetSample(int index)**|**Returns the spline sample at the given index**|
| :- | :- |
|**void Rebuild()**|**Causes the user to rebuild on the next frame**|
|**void RebuildImmediate()**|**Causes the user to rebuild immediately**|
|**void ModifySample(ref SplineSample source, SplineSample destination)**|**Modifies a sample using the user’s modifiers**|
|**void SetClipRange(double from, double to)**|**Sets the clip range of the user (same as setting clipFrom and clipTo)**|
|**double ClipPercent(double percent)**|**Converts a global spline percent [0-1] to a local percent within [clipFrom-clipTo]**|
|**void ClipPercent(ref double percent)**|**Converts a global spline percent [0-1] to a local percent within [clipFrom-clipTo]**|
|**double UnclipPercent(double percent)**|**Converts a local [clipFrom-clipTo] percent to a global [0-1] percent**|
|**void UnclipPercent(ref double percent)**|**Converts a local [clipFrom-clipTo] percent to a global [0-1] percent**|
|**Vector3 EvaluatePosition(double percent)**|**Evaluates the clip range and returns the position at the given percent**|
|**void Evaluate(double percent, ref SplineSample result)**|**Evaluates the clip range and writes the spline sample at the given percent to the given result object**|
|**SplineSample Evaluate(double percent)**|**Evaluates the clip range and returns a spline sample at the given percent**|
|**void Evaluate(ref SplineSample[] results, double from = 0.0, double to = 1.0)**|**Evaluates the entire clip range within the given range and writes them to an array of spline samples**|
|**void EvaluatePositions(ref Vector3[] positions, double from = 0.0, double to = 1.0)**|**Evaluates the entire clip range within the given range and writes them to the and writes them to the provide Vector3 array**|
|**double Travel(double start, float distance, Spline.Direction direction, out float moved)**|**Moves along the clip range, based on a start percent and a distance in world units and returns the new percent at the reached position**|
|**double Travel(double start, float distance, Spline.Direction direction = Spline.Direction.Forward)**|**Moves along the clip range, based on a start percent and a distance in world units and returns the new percent at the reached position as well as the moved distance**|
|**void Project(Vector3 position, ref  SplineSample result, double from = 0.0, double to = 1.0)**|**Projects a point in world space onto the clip range and returns the percent  along the spline that resembles the closest sample to that point**|
|**float CalculateLength(double from = 0.0, double to = 1.0)**|**Calculates the distance along the clip range within the provided range**|
## SplineTracer : SplineUser
Base class for behaviors that use the spline to position themselves. Spline Follower, Spline Projector, Spline Positioner.
### Enums

|**PhysicsMode { Transform, Rigidbody, Rigidbody2D }**|**Denotes an approach to the motion application of the tracer. Transform will use the position and rotation of the Transform component. Rigidbody will use MovePosition and MoveRotation of the Rigidbody  component. Rigidbody2D will use a 2D rigidbody.**|
| :- | :- |

### Public Properties

|**TransformModule motion**|**The transform module of the tracer that is used to apply the position, rotation, and scale of the object. It contains rules how to apply these parameters.**|
| :- | :- |
|**PhysicsMode physicsMode**|**Which physics mode the tracer should use**|
|**SplineSample result**|**The result of the last spline evaluation.**|
|**SplineSample modifiedResult**|**The result of the last spline evaluation but modified using the sample modifiers.**|
|**Bool dontLerpDirection**|**If true, the Tracer’s direction will snap between each spline sample instead of interpolating smoothly. Bool dontLerpDirection**|
|**Spline.Direction direction**|**The direction in which the tracer is moving and/or looking along the spline.**|
|**Bool applyDirectionRotation**|**If true, the tracer’s rotation will be affected by the direction property. If false, the tracer will always look forward along the spline.**|
|**Bool useTriggers**|**If true, the tracer will utilize the SplineComputer’s triggers.**|
|**Int triggerGroup**|**Which trigger group to use**|

### Events

|**JunctionHandler onNode**|` `**Called when the tracer passes a node or multiple nodes. Returns the list of passed nodes in the last frame.** |
| :- | :- |
|**EmptySplineHandler onMotionApplied**|**Called when the TransformModule applies the motion – useful to apply additional offsets via script directly to the tracer’s transform or rigidbody.**|

### Public Methods

|**void SetPercent(double percent, bool checkTriggers = false, bool handleJunctions = false)**|**Sets the position of the tracer in percent along the spline. If checkTriggers is true, triggers will be used. If handleJunctions is true, onNode will be called if the tracer passes a node.**|
| :- | :- |
|**void SetDistance(float distance, bool checkTriggers = false, bool handleJunctions = false)**|**Similar to SetPercent but uses distance in world units to position the tracer along the Spline.**|

## TransformModule
Used by the SplineTracer to apply transformation
### Public Properties

|**Vector2 offset**|**The offset from the spline in local coordinates**|
| :- | :- |
|**Vector3 rotationOffset**|**Angular offset from the spline direction in local coordinates**|
|**Vector3 baseScale**|**Base scale to use if scale applying is enabled**|
|**bool is2D**|**If true, the transform module will be set up for 2D**|
|**bool applyPositionX**|**Should position along world X be applied?**|
|**bool applyPositionY**|**Should position along world Y be applied?**|
|**bool applyPositionZ**|**Should position along world Z be applied?**|
|**bool applyPosition2D**|**Should position be applied in 2D?**|
|**bool retainLocalPosition**|**Should the local offset of the object be preserved in relation to the spline?**|
|**bool applyRotationX**|**Should rotation be applied around world X?**|
|**bool applyRotationY**|**Should rotation be applied around world Y?**|
|**bool applyRotationZ**|**Should rotation be applied around world Z?**|
|**bool applyRotation2D**|**Should rotation be applied in 2D?**|
|**bool retainLocalRotation**|**Should the local angular offset of the object be preserved in relation to the spline?**|


## SplineTrigger
### Enums

|**Type { Double, Forward, Backward }**|<p>- **Double works both forwards and backwards**</p><p>- **Forward only works forwards**</p><p>- **Backward only works backwards**</p>|
| :- | :- |

### Public Properties

|**string name**|**The name of the trigger**|
| :- | :- |
|**SplineTrigger.Type type**|**The type of the trigger**|
|**bool workOnce**|**Should this trigger only work once?**|
|**double position**|**The percent along the spline where this trigger is located**|
|**bool enabled**|**Is this trigger enabled and working?**|
|**Color color**|**The color of the trigger**|
|**UnityEvent onCross**|**UnityEvent to invoke when the trigger is triggered**|

### Public Methods

|**void AddListener(UnityAction<SplineUser> action)**|**Adds a listener to be invoked when the trigger is triggered. The spline user that has crossed the trigger is passed as an argument.**|
| :- | :- |
|**void AddListener(UnityAction action)**|**Adds a listener to be invoked when the trigger is triggered. Nothing will be passed as arguments.**|
|**void RemoveListener(UnityAction<SplineUser> action)**|**Removes the given listener from the trigger**|
|**void RemoveAllListeners()**|**Removes all subscribed listeners from the trigger**|
|**void Reset()**|**Resets the trigger if it is set to work once and it has already worked**|
|**bool Check(double previousPercent, double currentPercent)**|**Checks the trigger (called automatically by SplineComputer)**|
|**void Invoke()**|**Invokes the trigger (called automatically by SplineComputer)**|


## SplineTriggerGroup

### Public Properties

|**bool enabled**|**Is the trigger group enabled?**|
| :- | :- |
|**string name**|**The name of the trigger group**|
|**Color color**|**The color of the trigger group**|
|**SplineTrigger[] triggers**|**The triggers in this trigger group**|

### Public Methods

|**void Check(double start, double end)**|**Checks all triggers inside the trigger group (called automatically by SplineComputer)**|
| :- | :- |
|**void Reset()**|**Resets all triggers inside the trigger group**|
|**List<SplineTrigger> GetTriggers(double from, double to)**|**Gets a list of triggers lying on the spline between from and to**|
|**SplineTrigger AddTrigger(double position, SplineTrigger.Type type)**|**Creates a new trigger in the trigger group**|
|**SplineTrigger AddTrigger(double position, SplineTrigger.Type type, string name, Color color)**|**Creates a new trigger in the trigger group**|
|**void RemoveTrigger(int index)**|**Removes the trigger at the given index**|