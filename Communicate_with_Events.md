# Communicate with Events 1
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
Create and dispatch events in a component’s JavaScript class. To create an event, use the CustomEvent() constructor. To dispatch an event, call the EventTarget.dispatchEvent() method.

The c-paginator component contains Previous and Next buttons. When a user clicks the buttons, the component creates and dispatches previous and next events. You can drop the paginator component into any component that needs Previous and Next buttons. That parent component listens for the events and handles them.

```Javascript
<!-- paginator.html -->
<template>
    <lightning-layout>
        <lightning-layout-item>
            <lightning-button label="Previous" icon-name="utility:chevronleft" onclick={previousHandler}></lightning-button>
        </lightning-layout-item>
        <lightning-layout-item flexibility="grow"></lightning-layout-item>
        <lightning-layout-item>
            <lightning-button label="Next" icon-name="utility:chevronright" icon-position="right" onclick={nextHandler}></lightning-button>
        </lightning-layout-item>
    </lightning-layout>
</template>
```
When a user clicks a button, the previousHandler or nextHandler function executes. These functions create and dispatch the previous and next events.

```javascript
// paginator.js
import { LightningElement } from 'lwc';

export default class Paginator extends LightningElement {
    previousHandler() {
        this.dispatchEvent(new CustomEvent('previous'));
    }

    nextHandler() {
        this.dispatchEvent(new CustomEvent('next'));
    }
}
```
These events are simple “something happened” events. They don’t pass a data payload up the DOM tree, they simply announce that a user clicked a button.

Let’s drop paginator into a component called c-event-simple, which listens for and handles the previous and next events.

To listen for events, use an HTML attribute with the syntax oneventtype. Since our event types are previous and next, the listeners are onprevious and onnext.
```javascript
<!-- eventSimple.html -->
<template>
    <lightning-card title="EventSimple" icon-name="custom:custom9">
        <div class="slds-m-around_medium">
            <p class="slds-m-vertical_medium content">Page {page}</p>
            <c-paginator onprevious={previousHandler} onnext={nextHandler}></c-paginator>
        </div>
    </lightning-card>
</template>
```
When c-event-simple receives the previous and next events, previousHandler and nextHandler increase and decrease the page number. Because the page property is tracked, when its value changes it causes the template to rerender.
```javascript
// eventSimple.js
import { LightningElement, track } from 'lwc';

export default class EventSimple extends LightningElement {
    @track page = 1;

    previousHandler() {
        if (this.page > 1) {
            this.page = this.page - 1;
        }
    }

    nextHandler() {
        this.page = this.page + 1;
    }
}
```
#Pass Data in an Event
https://developer.salesforce.com/docs/component-library/documentation/lwc/lwc.events_create_dispatch


