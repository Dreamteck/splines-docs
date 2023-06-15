# Length Calculator
A basic component which calculates the length of the spline using the approximation rate defined by the Spline Computer’s precision and the Resolution of the Length Calculator component.

![](./_images/109.png)

The Length Calculator can have length events added to it which fire when the spline has reached a certain length.

To add an event, click the “Add Length Event” button. This will create a new empty event. Set the “target length” value to the value that should be listened for, select the target object (an object with a certain behavior), select the method and assign the argument’s value. The Dropdown menu tells the Length Calculator when to listen for the specific event.

- Growing: will invoke the event when the target length has been reached by growing
- Shrinking: will invoke the event when the target length has been reached by shrinking
- Both: will invoke the event when this value has been reached or passed in both directions

This component has one additional module that controls its behavior. Refer to [Triggers](/pages/spline_triggers/spline_triggers#spline-triggers) and [Modules](/pages/sample_modifiers/sample_modifiers#sample-modifiers) for more information.