# JavaScript Decorators
Decorators add functionality to a property or function. The ability to create decorators is part of ECMAScript, but these decorators are unique to Lightning Web Components.

# @api
To expose a public property, decorate it with @api. Public properties define the API for a component. To communicate down the component hierarchy, an owner component can access and set a child component’s public properties.

Public properties are reactive. If the value of a reactive property changes, the component’s template renders any content that references the property. For more information, see Public Properties.

To expose a public method, decorate it with @api. Public methods also define the API for a component. To communicate down the component hierarchy, owner and parent components can call JavaScript methods on child components.

Tip

To communicate down the containment hierarchy, use properties. To communicate up the containment hierarchy, fire an event in a child component and handle it in an owner or container component. See Events.

# @track
Private properties that contain a primitive value are reactive. If a reactive property is used in a template and its value changes, the component re-renders.

If a private property contains an object, to mark is as reactive, annotate it with @track. LWC tracks changes on the object's properties. If a property is used in a template and its value changes, the component re-renders.
