<!DOCTYPE html>
<html>
<body>
      <button id="myBtn">Try it</button>

      <p id="demo"></p>

      <script>
     # document.getElementById("myBtn").addEventListener("click", displayDate);

      function displayDate() {
        document.getElementById("demo").innerHTML = Date();
      }
      </script>
</body>
</html> 

# HTML DOM allows JavaScript to react to HTML events:
        When a user clicks the mouse
        When a web page has loaded
        When an image has been loaded
        When the mouse moves over an element
        When an input field is changed
        When an HTML form is submitted
        When a user strokes a key
