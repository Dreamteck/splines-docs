# Object Bender
The purpose of the Object Bender is to bend a game object and all of its children along a spline. The Object Bender will position and orient all of the objects in the hierarchy along the spline. The difference between the Object Bender and the Object Controller however is that the Object Bender will also bend any Meshes, MeshColliders and SplineComputers in the hierarchy.

When an Object Bender is added in the editor or in runtime, it will be in edit mode by default. This means that the object bender will not work. The edit mode allows the developer to edit the children in the hierarchy.

![](./_images/110.png)

In edit mode, the ObjectBender editor will visualize the bounds of the parent object and all of the children in the hierarchy. These bounds include the Transform components, meshes, colliders and SplineComputers. 

When in edit mode, there is a “Bend” button at the bottom of the ObjectBender’s inspector. Clicking it will exit edit mode and will start bending the objects along the spline.

![](./_images/111.png)

When in Bend mode, all objects will be controlled by the spline. Each object from the hierarchy will be editable after it has been bent but if the spline updates, it will get recalculated and repositioned.

In Bend mode, the “Bend” button will be replaced with an “Enter edit mode” button which will revert the bend and enable editing.

The ObjectBender bends all the objects along one of the three local axes. The axes are local to the parent object. The bend axis can be selected through the ObjectBender’s inspector and will be applied immediately even if the ObjectBender is in edit mode.

The ObjectBender component gives complete control over what is bent and how it’s bent. A bend property is created for each object from the hierarchy. Each bend property allows the developer to toggle certain aspects of the bending process on and off for each object individually or toggle off bending for the object completely. 

![](./_images/112.png)

All bend properties are automatically exposed in the inspector. Clicking on a property will highlight it in blue and will open the property editor. Several properties can be edited simultaneously. To select multiple properties, hold down Ctrl and click on properties to add them to the selection. Shift + click will select multiple properties in a row.