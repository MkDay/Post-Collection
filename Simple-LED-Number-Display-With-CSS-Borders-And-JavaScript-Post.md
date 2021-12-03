# Simple LED-Number-Display With CSS Borders And JavaScript
 

Have you ever wondered how numbers are displayed on a digital clock or a basic calculator?

You can think about it in this way.

There are seven red lines made up with a bunch of red LED bulbs and, those lines are arranged like two rectangles that appear one on top of the other.

According to [*Wikipedia*](https://en.m.wikipedia.org/wiki/Seven-segment_display), those seven red lines display introduced as **_Seven-Segment Display_** and, each of them will be visible or invisible depending on the digit that needed to be displayed.

However, if you want to make one by yourself, you just need to follow these three simple steps. I promise it won't take more than one hour.

**Note:** Here, I don't suppose to create the whole clock itself, but I will show you how to make a *Seven-Segment-Display* and how to make digits (from 0 to 9) as visible or invisible on it.

## Steps you have to follow:

1. **Step 1. HTML:** Create two rectangles.

2. **Step 2. CSS :** Add borders separately for each side of the two rectangles.

3. **Step 3. JavaScript :** Display digits by changing the border color on each side of the rectangles.

So let's follow the steps.

### 1. HTML: Create two rectangles

* Create the parent `div`: 
`<div id="container">`

* Create the two child `div` inside parent `div`: 
`<div id="top-box">` and `<div id="bottom-box">`

Here is the HTML code:

```html

<body>
  <h1>LED Number Display</h1>
  <div id="container">

    <div id="top-box"></div>
    <div id="bottom-box"></div>

  </div>

</body>

```

### 2. CSS: Add borders separately for each side of the two rectangles.

First of all,

* Give the same `width` and `height` for the both `#top-box` and `#bottom-box`.

* Place them one on top of the other using *CSS flex*. 

And then,

* Create borders for each side of the both elements separately.

**For the `top-box`:**

```css

 border-left: 10px ridge transparent;
 border-right: 10px ridge transparent;
 border-top: 10px ridge transparent;
 border-bottom: 5px inset transparent;
 
```

**For the `bottom-box`:**

```css

 border-left: 10px ridge transparent;
 border-right: 10px ridge transparent;
 border-top: 5px inset transparent; 
 border-bottom: 10px ridge transparent;

```

Here is the full CSS code:

```css

html {
  font-size: 62.5%;
}

@media (max-width: 75em) {
  html {
    font-size: 60%;
  }
}

@media (max-width: 61.25em) {
  html {
    font-size: 58%;
  }
}

@media (max-width: 28.75em) {
  html {
    font-size: 55%;
  }
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  padding: 3rem;
}

#container {
  margin-top: 3rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 550px;
  height: 450px;
  background: #555;
  border: ridge 8px #fff;
}

#top-box {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100px;
  height: 100px;
  background: #555;
  border-left: 10px ridge transparent;
  border-right: 10px ridge transparent;
  border-top: 10px ridge transparent;
  border-bottom: 5px inset transparent;
  border-radius: 10px;
}

#bottom-box {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100px;
  height: 100px;
  background: #555;
  border-left: 10px ridge transparent;
  border-right: 10px ridge transparent;
  border-top: 5px inset transparent;
  border-bottom: 10px ridge transparent;
  border-radius: 10px;
}

```
### 3. JavaScript: Display digits by changing the colors of each border.

* Select the each element.

```javascript

let topBox = document.querySelector('#top-box');
let bottomBox = document.querySelector('#bottom-box');

```

* Create two variables to store colors 'red' and 'transparent'.

**Eg:** ` let on='red' ` and ` let off='transparent' `.


* Create 10 functions to display the each digit.

**Eg:** `let zero = () => {...}` to `let nine = () => {...}`


* Change the color of each single border using the variables, `on` and `off` to display a specific digit.

**Eg:**
`topBox.style.borderTopColor = on;` 
`bottomBox.style.borderTopColor = off;`

* If you want, you can animate those digits repeatedly using `setTimeout ()` and` setInterval ()`.

Here is the full JavaScript code:

```javascript

let topBox = document.querySelector("#top-box");

let bottomBox = document.querySelector("#bottom-box");

let on = "red";
let off = "transparent";

let zero = () => {
  topBox.style.borderTopColor = on;
  topBox.style.borderLeftColor = on;
  topBox.style.borderRightColor = on;
  topBox.style.borderBottomColor = off;

  bottomBox.style.borderTopColor = off;
  bottomBox.style.borderLeftColor = on;
  bottomBox.style.borderRightColor = on;
  bottomBox.style.borderBottomColor = on;
};

let one = () => {
  topBox.style.borderTopColor = off;
  topBox.style.borderLeftColor = off;
  topBox.style.borderRightColor = on;
  topBox.style.borderBottomColor = off;
  topBox.style.borderBottomColor = off;

  bottomBox.style.borderTopColor = off;
  bottomBox.style.borderLeftColor = off;
  bottomBox.style.borderRightColor = on;
  bottomBox.style.borderBottomColor = off;
};

let two = () => {
  topBox.style.borderTopColor = on;
  topBox.style.borderLeftColor = off;
  topBox.style.borderRightColor = on;
  topBox.style.borderBottomColor = on;

  bottomBox.style.borderTopColor = on;
  bottomBox.style.borderLeftColor = on;
  bottomBox.style.borderRightColor = off;
  bottomBox.style.borderBottomColor = on;
};

let three = () => {
  topBox.style.borderTopColor = on;
  topBox.style.borderLeftColor = off;
  topBox.style.borderRightColor = on;
  topBox.style.borderBottomColor = on;

  bottomBox.style.borderTopColor = on;
  bottomBox.style.borderLeftColor = off;
  bottomBox.style.borderRightColor = on;
  bottomBox.style.borderBottomColor = on;
};

let four = () => {
  topBox.style.borderTopColor = off;
  topBox.style.borderLeftColor = on;
  topBox.style.borderRightColor = on;
  topBox.style.borderBottomColor = on;

  bottomBox.style.borderTopColor = on;
  bottomBox.style.borderLeftColor = off;
  bottomBox.style.borderRightColor = on;
  bottomBox.style.borderBottomColor = off;
};

let five = () => {
  topBox.style.borderTopColor = on;
  topBox.style.borderLeftColor = on;
  topBox.style.borderRightColor = off;
  topBox.style.borderBottomColor = on;

  bottomBox.style.borderTopColor = on;
  bottomBox.style.borderLeftColor = off;
  bottomBox.style.borderRightColor = on;
  bottomBox.style.borderBottomColor = on;
};

let six = () => {
  topBox.style.borderTopColor = on;
  topBox.style.borderLeftColor = on;
  topBox.style.borderRightColor = off;
  topBox.style.borderBottomColor = on;

  bottomBox.style.borderTopColor = on;
  bottomBox.style.borderLeftColor = on;
  bottomBox.style.borderRightColor = on;
  bottomBox.style.borderBottomColor = on;
};

let seven = () => {
  topBox.style.borderTopColor = on;
  topBox.style.borderLeftColor = on;
  topBox.style.borderRightColor = on;
  topBox.style.borderBottomColor = off;

  bottomBox.style.borderTopColor = off;
  bottomBox.style.borderLeftColor = off;
  bottomBox.style.borderRightColor = on;
  bottomBox.style.borderBottomColor = off;
};

let eight = () => {
  topBox.style.borderTopColor = on;
  topBox.style.borderLeftColor = on;
  topBox.style.borderRightColor = on;
  topBox.style.borderBottomColor = on;

  bottomBox.style.borderTopColor = on;
  bottomBox.style.borderLeftColor = on;
  bottomBox.style.borderRightColor = on;
  bottomBox.style.borderBottomColor = on;
};

let nine = () => {
  topBox.style.borderTopColor = on;
  topBox.style.borderLeftColor = on;
  topBox.style.borderRightColor = on;
  topBox.style.borderBottomColor = on;

  bottomBox.style.borderTopColor = on;
  bottomBox.style.borderLeftColor = off;
  bottomBox.style.borderRightColor = on;
  bottomBox.style.borderBottomColor = on;
};

function playNumbers() {
  setTimeout(zero, 0);
  setTimeout(one, 1000);
  setTimeout(two, 2000);
  setTimeout(three, 3000);
  setTimeout(four, 4000);
  setTimeout(five, 5000);
  setTimeout(six, 6000);
  setTimeout(seven, 7000);
  setTimeout(eight, 8000);
  setTimeout(nine, 9000);
}

playNumbers();
setInterval(playNumbers, 10000);

```
Now you are done creating your *LED Number Display*.

Here is the *live demo* for the *LED Number Display*.

{% codepen https://codepen.io/MkDay/pen/YzryPwE %}

If you have any ideas to create this in a different way, feel free to mention them below. 

**_Happy Coding!_**

**_Image Credit:_**



