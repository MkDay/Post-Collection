# Measure Distance Between Two Points In An Image Using JavsScript
 

This is not about complex astronomy, but we all love the sky, right? Also, we like images of stars, nebulae, galaxies etc. 

So that's why I came up with this idea, how about if we can measure the distance (only for fun) between two stars which appear in an image?

Well, it might not be about the stars in an image but could be anything such as, two cities in the map, we can measure the distance between them with this simple **_beginner-friendly_** project.
 
In my opinion, this is a good project to practice basic knowledge of HTML, CSS and, JavaScript. Most importantly, you don't want to use *HTML canvas* here.

Okay, what does this project all about? 
 
There is an image of the *starry night sky* on the page, so the user can measure the distance between two or more stars that appear on the image by clicking on them.

So let's break down this project into smaller parts.


### 1. HTML: 

Create `div` elements for, 
  * image container: `<div id="container">`
  * result container to display calculated distance: `<div id="results">`

```html

<body>
  <h1>Measure Distance</h1>

  <div id="container"></div>

  <h3>Results</h3>

  <div id="results">

    <div id="current-result"></div>

    <div id="total-result"></div>

  </div>

  <input id="reset" type="button" value="Reset" onclick="document.location.reload()">

</body>

```
 
### 2. CSS:
 
* set our *starry night sky* image as `background-image` of the image container.

```css

#container {
 width: 500px;
 height: 600px;
 border: 2px solid BurlyWood;
 
 background-image: url(https://images.unsplash.com/photo-1515651571008-95427bed8e0b?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxNDU4OXwwfDF8cmFuZG9tfHx8fHx8fHx8MTYzOTQ0NDQ1NA&ixlib=rb-1.2.1&q=85); 
 background-position: center; 
 background-repeat: no-repeat; 
 background-size: cover; 
}

```

* create and style two class selectors `.points` and `.lines` which we're going to create in the JavaScript later.
* make the `position: absolute;` for the both `.points` and `.lines`.

```css

.points {
 position: absolute;
 border: 2px solid red;
 border-radius: 50%;
 opacity: 1;
}

.lines {
 position: absolute;
 border: 1px solid white;
 height: 0;
 opacity: 1;
}

```

* add additional styling as your taste.


### 3. JavaScript:

#### (1) create variables

* select `#container` and `#results` using DOM.
* create an object with two arrays to store **X** and **Y** coordinates of the user's click points.
* create two arrays to store created `div` elements `points` and `lines`.


```javascript

let container = document.querySelector("#container");

let results = document.querySelector("#results");
let currentResult = document.querySelector("#current-result");
let totalResult = document.querySelector("#total-result");

let coordinates = {
  coordX: [],
  coordY: []
};

let points = [];
let lines = [];

```

#### (2) *addEventListener()* to the container

* store **X** and **Y** coordinates in the `coordinates` object.
* call the functions `createPoints()` then `createLines()` within the callback function.

```javascript

container.addEventListener("click", (e) => {
  coordinates.coordX.push(e.x);
  coordinates.coordY.push(e.y);

  createPoints(e.x, e.y);

  let prevX = coordinates.coordX[coordinates.coordX.length - 2];

  let prevY = coordinates.coordY[coordinates.coordY.length - 2];

  if (coordinates.coordX.length > 1) {
    createLines(prevX, prevY, e.x, e.y);
  }
});

```
#### (3) *createPoints()* function

* create a `for-loop` which runs from 0 to number of clicked points.
* in each time create a `div` inside the loop.
* set its `className` as `points`.
* set left and top coordinates of the `div`. (`e.x` and `e.y`)
* append it to the `#container`.


```javascript

function createPoints(posX, posY) {
  for (let i = 0; i < coordinates.coordX.length; i++) {
    points[i] = document.createElement("div");
    points[i].className = "points";
    points[i].style.left = `${coordinates.coordX[i]}px`;
    points[i].style.top = `${coordinates.coordY[i]}px`;

    container.appendChild(points[i]);
  }
}

```

#### (4) *createLines()* function

Okay, we reach to the most complex and most important part in this project. Because some of math are involved here. I don't suppose to be your math teacher, but these are the steps we're going to follow.

* get the distance between the two points.

```javascript

  let distance = Math.sqrt(((x2-x1) * (x2-x1)) + ((y2-y1) * (y2-y1)));

```

* find the middle point of the two points.

```javascript

// X and Y coordinates of the middle point

    let midX = (x1+x2)/2;
    let midY = (y1+y2)/2;

```

* draw a horizontal line of that distance across the midpoint.

```javascript

 lines[i].style.width = `${distance}px`;
 lines[i].style.left = `${(midX - (distance/2))}px`;
 lines[i].style.top = `${midY}px`;

```

* calculate the angle to rotate it around the middle point to fit it into the actual two points.

```javascript

/* get the inclination in radian unit and then convert it into degree */

 let inclinationInRadian = Math.atan2(y1-y2, x1-x2);
 let inclinationInDegree = (inclinationInRadian * 180)/ Math.PI;

```

* rotate the line.

```javascript

  lines[i].style.transform = 'rotate('+inclinationInDegree +'deg)';

```

Here is the complete `createLines()` function.

```javascript

function createLines(x1, y1, x2, y2) {
  let distance = Math.sqrt((x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1));

  let midX = (x1 + x2) / 2;
  let midY = (y1 + y2) / 2;

  let inclinationInRadian = Math.atan2(y1 - y2, x1 - x2);
  let inclinationInDegree = (inclinationInRadian * 180) / Math.PI;

  for (let i = 0; i < coordinates.coordX.length; i++) {
    lines[i] = document.createElement("div");
    lines[i].className = "lines";

    lines[i].style.width = `${distance}px`;
    lines[i].style.left = `${midX - distance / 2}px`;
    lines[i].style.top = `${midY}px`;
    lines[i].style.transform = "rotate(" + inclinationInDegree + "deg)";

    container.appendChild(lines[i]);
  }

  currentResult.innerHTML = `<strong>Current Result:-</strong> <br>`;

  totalResult.innerHTML = `<strong>Total Result:-</strong> <br>`;

  getDistance(distance);
}

```


#### (5) *getDistance()* function

Now we have the distance which is in pixels, so we should convert it to the centimeter. 

* 1 pixels = 0.0264583333cm

* distance in centimeter = distance in pixels × 0.0264583333

```javascript

function getDistance(distance) {
  let pixelToCm = distance * 0.0264583333;
  pixelToCm = Number(pixelToCm.toFixed(2));

  totalDistance += pixelToCm;
  totalDistance = Number(totalDistance.toFixed(2));

  currentPath += `Line ${++lineCounter}:- ${pixelToCm}cm<br>`;

  totalPath += `${totalDistance}cm<br>`;

  currentResult.innerHTML += currentPath;

  totalResult.innerHTML += totalPath;

  results.scrollTop = results.scrollHeight;
}

```

This is the live demo @CodePen.

{}

**NOTE:** **_This won't work correctly within the smaller viewports._**

### Conclusion

Congrats! we're done with the project, now only remains is measure the distance between your favorite stars! 

So count stars, measure distance and have fun.

PS: if you really enjoy this article, you can support me at [ko-fi](https://ko-fi.com/mkdaycode). I always appreciate your support, it really encourages me to keep going.

Happy Coding!

**_Image Credit: Nathan Anderson on Unsplash_**
