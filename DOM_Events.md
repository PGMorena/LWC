
# HTML DOM allows JavaScript to react to HTML events:
        When a user clicks the mouse
        When a web page has loaded
        When an image has been loaded
        When the mouse moves over an element
        When an input field is changed
        When an HTML form is submitted
        When a user strokes a key
 ```javascript
 <button id="myBtn">Try it</button>
 <p id="demo"></p>
 <script>
 document.getElementById("myBtn").addEventListener("click", displayDate);

  function displayDate() {
        document.getElementById("demo").innerHTML = Date();
  }
 </script>
 ```

Throughout the web platform events are dispatched to objects to signal an occurrence, such as network activity or user interaction. These objects implement the EventTarget interface and can therefore add event listeners to observe events by calling addEventListener():
```javascript
obj.addEventListener("load", imgFetched)

function imgFetched(ev) {
  // great success
  …
}
```
Event listeners can be removed by utilizing the removeEventListener() method, passing the same arguments.
Event listeners can be removed by utilizing the removeEventListener() method, passing the same arguments.

Events are objects too and implement the Event interface (or a derived interface). In the example above ev is the event. ev is passed as an argument to the event listener’s callback (typically a JavaScript Function as shown above). Event listeners key off the event’s type attribute value ("load" in the above example). The event’s target attribute value returns the object to which the event was dispatched (obj above).

Although events are typically dispatched by the user agent as the result of user interaction or the completion of some task, applications can dispatch events themselves by using what are commonly known as synthetic events:
```javascript
// add an appropriate event listener
obj.addEventListener("cat", function(e) { process(e.detail) })

// create and dispatch the event
var event = new CustomEvent("cat", {"detail":{"hazcheeseburger":true}})
obj.dispatchEvent(event)
```
Apart from signaling, events are sometimes also used to let an application control what happens next in an operation. For instance as part of form submission an event whose type attribute value is "submit" is dispatched. If this event’s preventDefault() method is invoked, form submission will be terminated. Applications who wish to make use of this functionality through events dispatched by the application (synthetic events) can make use of the return value of the dispatchEvent() method:
```javascript
if(obj.dispatchEvent(event)) {
  // event was not canceled, time for some magic
  …
}
```
When an event is dispatched to an object that participates in a tree (e.g. an element), it can reach event listeners on that object’s ancestors too. First all object’s ancestor event listeners whose capture variable is set to true are invoked, in tree order. Second, object’s own event listeners are invoked. And finally, and only if event’s bubbles attribute value is true, object’s ancestor event listeners are invoked again, but now in reverse tree order.

Let’s look at an example of how events work in a tree:
```javascript
<!doctype html>
<html>
 <head>
  <title>Boring example</title>
 </head>
 <body>
  <p>Hello <span id=x>world</span>!</p>
  <script>
   function test(e) {
     debug(e.target, e.currentTarget, e.eventPhase)
   }
   document.addEventListener("hey", test, {capture: true})
   document.body.addEventListener("hey", test)
   var ev = new Event("hey", {bubbles:true})
   document.getElementById("x").dispatchEvent(ev)
  </script>
 </body>
</html>
```
The debug function will be invoked twice. Each time the event’s target attribute value will be the span element. The first time currentTarget attribute’s value will be the document, the second time the body element. eventPhase attribute’s value switches from CAPTURING_PHASE to BUBBLING_PHASE. If an event listener was registered for the span element, eventPhase attribute’s value would have been AT_TARGET.

Resource: https://dom.spec.whatwg.org/#events
