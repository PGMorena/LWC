# property is a value stored in the hash key, whereas method is a function stored in hash key.
```javascript
var person = {
  name: "John Doe",
  sayHello: function() {
        console.log("hello");
  }
}
```

In this code sample.

name is the property of object person, it stored String "John Doe", you can access it via dot notation person.name.
sayHello is the method of object, and it is a function. You can also access it using dot notation person.sayHello(). Notice that sayHello is just a function, and function invocation requires you to use () here.
