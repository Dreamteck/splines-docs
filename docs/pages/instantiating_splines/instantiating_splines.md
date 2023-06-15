# Instantiating Splines
Spline Computers can safely be put into prefabs and instantiated during runtime. However, since they come from prefabs, instantiating a spline at a different position than the one that is written in the prefab will cause the spline to revert to its original prefab location.

**To fix this issue, make sure that all Spline Computers inside any prefab are set to rebuild on awake.**