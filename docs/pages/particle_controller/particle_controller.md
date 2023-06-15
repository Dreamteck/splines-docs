# Particle Controller
The Particle Controller uses a Shuriken particle system and places its particles along a spline. 

![](./_images/103.png)

This component offers a preview right in the editor if it’s attached to the particle system that it controls. 

The Particle Controller has an Emit point which controls where new particles will be spawned. The modes are:

- Beginning – Emits particles from the beginning of the spline
- Ending – Emits particles from the final point of the spline
- Ordered – Emits particles from the beginning of the spline towards it’s end based on particle index
- Random – Emits particles between the beginning and the end of the spline on a random basis

Another very important option of the Particle Controller is the Motion type. This controls the particle behavior over its lifetime. The available motion types are:

- Use Particle System – Uses the motion, defined by the Particle System component
- Follow Forward – Particles move forward along the spline based on their lifetime
- Follow Backward – Particles move backward along the spline based on their lifetime
- By Normal – Particles get their initial velocity set in the direction of the spline sample’s normal
- By Normal Randomized – Same as above but the direction vector is randomized

![](./_images/104.png)