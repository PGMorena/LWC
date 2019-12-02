# Communicate with Events
Lightning web components dispatch standard DOM events.
Components can also create and dispatch custom events. 

Events in Lightning web components are built on DOM Events, a collection of APIs and objects available in every browser.

The DOM events system is a programming design pattern that includes these elements.

An event name, called a type
A configuration to initialize the event
A JavaScript object that emits the event
To create events, we strongly recommend using the CustomEvent interface. In Lightning web components, CustomEvent provides a more consistent experience across browsers, including Internet Explorer. It requires no setup or boilerplate, and it allows you to pass any kind of data via the detail property, which makes it flexible.

Lightning web components implement the EventTarget interface, which allows them to dispatch events, listen for events, and handle events.

# Create and Dispatch Events

Resource to be continued.....
https://developer.salesforce.com/docs/component-library/documentation/lwc/lwc.events_create_dispatch

