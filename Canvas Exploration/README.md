## Canvas Notes

Canvas Size Management

* Resizing with CSS (100% width and height) does not accomplish full size because HTML does not take up the entire screen
* Instead we will set the w/h with JS

```js
var canvas = document.querySelector("canvas");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

//Event Listener for resizing browser
window.addEventListener("resize", function(e) {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
});
```

Drawing a Rectangle

```js
//Super object for drawing in a 2D space
var c = canvas.getContext("2d");

//Params: x, y (relative to top left corner) position
// Width, Height Sizes for the Rect
c.fillRect(100, 100, width, height);
```

Adding Color and Constructing a Line:

```js
//Line:
c.beginPath();
c.moveTo(50, 300);
c.lineTo(400, 20);
c.lineTo(450, 200);
c.strokeStyle = "blue";
c.stroke();
```

Drawing Arcs:

```js
//Arc: (x, y, radius, startAngle, endAngle, clockwise)
c.beginPath();
c.arc(300, 300, 50, 0, Math.PI * 2, false);
c.strokeStyle = "purple";
c.stroke();
```

Creating Animations:

* Construct recursive loop with request animation frame
* Use clearRect to remove prior frames (x,y, w,h)
* Define velocity and use (x,y) for screen collisions

```js
var x = 100;
var y = 100;
var dx = 2;
var dy = 2;
var radius = 40;
var wallBooster = 1.05;

//Animations
function animate() {
  requestAnimationFrame(animate);
  c.clearRect(0, 0, window.innerWidth, window.innerHeight);
  c.beginPath();
  c.arc(x, y, radius, 0, Math.PI * 2, false);
  c.stroke();

  //Reflection on edges of screen
  if (x + radius > window.innerWidth || x - radius < 0) {
    dx = -(dx * wallBooster);
  }

  if (y + radius > window.innerHeight || y - radius < 0) {
    dy = -(dy * wallBooster);
  }

  x += dx;
  y += dy;
  console.log("Animate");
}

animate();
```

Creating self-contained shapes (prototypal objects js)

* Function with draw and update functions

```js
//Circle Object
function Circle(x, y, radius, dx, dy) {
  this.x = x;
  this.y = y;
  this.radius = radius;
  this.dx = dx;
  this.dy = dy;

  this.draw = function() {
    c.beginPath();
    c.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
    c.stroke();
  };

  this.update = function() {
    //Handle screen edge collisions
    if (this.x + this.radius > window.innerWidth || this.x - this.radius < 0) {
      this.dx = -(this.dx * wallBooster);
    }

    if (this.y + radius > window.innerHeight || this.y - this.radius < 0) {
      this.dy = -(this.dy * wallBooster);
    }

    this.x += this.dx;
    this.y += this.dy;

    this.draw();
  };
}

var circle = new Circle(100, 100, 50, 2, 3);
```

Animating Multiple Circles:

* Iterate through a list of circle's update functions

```js
for (var i = 0; i < 50; i++) {
  var radius = 30;
  var x = Math.random() * (innerWidth - radius * 2) + radius;
  var y = Math.random() * (innerHeight - radius * 2) + radius;
  var dx = Math.random() - 0.5;
  var dy = Math.random() - 0.5;

  circleList.push(new Circle(x, y, radius, dx * 3, dy * 3));
}

//Animations
function animate() {
  requestAnimationFrame(animate);
  c.clearRect(0, 0, innerWidth, innerHeight);

  for (var i = 0; i < 50; i++) {
    circleList[i].update();
  }
}
```

Canvas Interaction:

* Use an event listener to capture the mouse position
* Use the x,y to detect proximity

```js
var mousePos = {
  x: 100,
  y: 100
};

window.addEventListener("mousemove", function(e) {
  mousePos.x = e.x;
  mousePos.y = e.y;
  console.log(mousePos);
});

this.mouseProximity = function(range) {
  xDiff = Math.abs(mousePos.x - this.x) <= range;
  yDiff = Math.abs(mousePos.y - this.y) <= range;
  this.highlighted = xDiff && yDiff;
  this.draw();
};
```
