# Stop Scrolling, Do Something Else Instead - JS Scroll Event
 

You've probably scrolled down all the way through your DEV feed, found this article, stopped and immediately realised you were scrolling for a while without even thinking about it. Right?

So welcome here then!

We're not here to fight upon **_scrolling all day is a good or bad habit_** but, as developers it is our job to provide a good experience with scrolling itself. Because there is no doubt it is one of the main part of the web.

So let's scrolled down to the topic.

The followings are the parts we're going to cover in this article:

1. Set scrolling.
2. Check the page/element has a scrollbar.
3. Detect scrolling event.
4. Manipulate scrolling event.
   1. Keep the focus to the bottom of the page/ element everytime.
   2. Scroll to the top/bottom using *scroll()* and *scrollTo()*
   3. Scroll the page/element in specific amount at a time using *scrollBy()*
   4. Align the page/element to the start, center or end using *scrollIntoView()*
5. Get the scrolling direction.
6. *window* vs. *documentElement* vs. *Element*

Also, the followings are the concepts that we're going to discuss in breif along with the examples:

1. scrollHeight
2. scrollTop
3. clientHeight
4. scroll() & scrollTo()
5. scrollBy()
6. scrollIntoView()
7. scrollY
 

Okay. Here is a brief about the HTML and CSS which we use to demonstrate the magic of the scroll event.

In the HTML, we have four `<div>` with the same content and, each `<div>` has the same structure as follows. The page and the content of each `<div>` element is long enough to make them scrollable.

```html

<div class="container">
  
  <div class="content">
   
   ... Some long dummy paragraphs

  </div>
</div> 

```


Also, there's a `<button id="btn">` to manipulate some scroll events that we're going to discuss later.

In the CSS, when we specify some `width` and `height` you can see that the contents of each `<div>` are not fit in their containers. 


### 1. Set Scrolling

To fix the above problem, we can set the below line of code in the CSS that will get all the content into its container and will allow the user to scroll down to see overflowed content.

```css

/* to scroll the page vertically */
body {
 overflow-y: scroll;
}

/* to scroll an element vertically */
#container {
 overflow-y: scroll;
}

```

**Note:** Here we use `overflow-y` for only demonstrate vertical scrolling. And you can learn more about CSS `overflow` [here](https://developer.mozilla.org/en-US/docs/Web/CSS/overflow).

 {% codepen https://codepen.io/MkDay/pen/abLVxao %}

Well, now we are all set to take control of the scroll event. So let's *scroll down* to the JavaScript code.

In the JavaScript we select all the `<div>` elements using their class name and select the `<button>` using its id.


```javascript

const container = document.querySelectorAll(".container");
const btn = document.querySelector("#btn");

```
### 2. Check the page/element has a scrollbar

As we have already seen, there is a scrollbar that appears in the browser window and in each `<div>`.

Also, we can prove it in our code using the following two properties as well.

**1) [Element.scrollHeight](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollHeight):**

* which gives the full height of the element (visible + overflowed content of the element).
* includes padding, height of pseudo elements (if any).
* doesn't include margins, borders, horizontal scrollbar.
* a read-only property (cannot set values to it). 
* returns an integer value.


**2) [Element.clientHeight](https://developer.mozilla.org/en-US/docs/Web/API/Element/clientHeight):** 

* which gives the visible height of the element.
* includes padding, height of pseudo elements (if any).
* doesn't include margins, borders, horizontal scrollbar.
* a read-only property  (cannot set values to it).
* returns an integer value.

So if there's no vertical scrollbar, `Element.scrollHeight` equal to `Element.clientHeight`.

```javascript

// check the page has a scrollbar

btn.addEventListener("click", (e) => {
  if (document.documentElement.scrollHeight >
    document.documentElement.clientHeight) {
    console.log("page has a scrollbar!");
  } else {
    console.log("page does not have a scrollbar!");
  }
});

// check the first container has a scrollbar

btn.addEventListener("click", (e) => {
  if (container[0].scrollHeight > container[0].clientHeight) {
    console.log("1st container has a scrollbar!");
  } else {
    console.log("1st container does not have a scrollbar!");
  }
});

```
### 3. Detect scrolling event 

By attaching an *EventListener* to the page/ element we can detect scroll event like below.

```javascript

// detect page scrolling

window.addEventListener('scroll', (e) => {
 console.log('page is scrolled!');
});


// detect scrolling of the first container

container[0].addEventListener('scroll', (e) => {
 console.log('1st container is scrolled!');
});

```
### 4. Manipulate scrolling event

Now we know how to check if the page/ element has a scrollbar and how to detect scroll event using `EventListener`. 

But that isn't the end of the world. We can manipulate it as well. Let's see how.

#### (4.1) Keep the focus to the bottom of the page/ element everytime

We can always display the bottom of the page/element even when we add new content to the page/ element dynamically using the following method.

`Element.scrollTop = Element.scrollHeight`

```javascript

window.addEventListener("load", (e) => {

  // show bottom of the page when the page is loading

  document.documentElement.scrollTop = document.documentElement.scrollHeight;


  // show bottom of the 1st container when the page is loading

  container[0].scrollTop = container[0].scrollHeight;
});

```

Here is the brief of the `Element.scrollTop`.

**[Element.scrollTop](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollTop):**

* it gives number of pixels that the element's content is scrolled vertically.
* Element.scrollTop = distance between element's top and top of the visible content. 
* if there's no vertical scrollbar, *Element.scrollTop = 0*.
* it could be a value from 0 to maximum of the element's height inclusively.


#### (4.2) Scroll to the top/bottom/center using *scroll()* or *scrollTo()*

The two methods that we can use here are `scroll()` & `scrollTo()`. 

**[Element.scroll()](https://developer.mozilla.org/en-US/docs/Web/API/Element/scroll) and [Element.scrollTo()](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollTo):**
* scrolls the document/ element to a particular set of coordinates.
* both are effectively same.

```
Syntax: 

Element.scroll(x-coord, y-coord)
Element.scroll(options)

Element.scrollTo(x-coord, y-coord)
Element.scrollTo(options)

```

* `x-coord`: the pixel along the X axis of the document/ element that you want displayed in the upper-left.
* `y-coord`: the pixel along the Y axis of the document/ element that you want displayed in the upper-left.
* options:
   * `top`: number of pixels to scroll along the Y axis.
   * `left`: number of pixels to scroll along the X axis.
   * `behavior`: smooth/ auto/ instant

The code below demonstrates how to manipulate scroll event using the `scrollTo()` method.

In this case we only talk about how to scroll to the top and center.

Here is how the code works.

**scroll to the top:**

* if the user clicks the button, it checks if the user has scrolled the page/ element (so the `scrollTop` won't be zero)
* if so, it will scroll the page/element back to the top.

```javascript

 /* ======= The page ======= */


btn.addEventListener("click", (e) => {

  // scroll to top 
  
  if (document.documentElement.scrollTop !== 0) {
    
    document.documentElement.scrollTo({
      top: 0,
      left: 0,
      behavior: "smooth"
    });
  }
  
});



/* ======The 1st container ======== */

 
btn.addEventListener("click", (e) => {

  // scroll to top 
  
  if (container[0].scrollTop !== 0) {

    container[0].scrollTo({
      top: 0,
      left: 0,
      behavior: "smooth"
    });
 }
  
});

```

**scroll to the center:**

* if the user clicks the button, it always scrolls to the center of the page/ element.

```javascript

 /* ======= The page ======= */


btn.addEventListener("click", (e) => {
  
  // scroll to the center

  document.documentElement.scrollTo({
    top: document.documentElement.scrollHeight / 2,
    left: 0,
    behavior: "smooth"
  });

});



/* ======The 1st container ======== */

 
btn.addEventListener("click", (e) => {

  
  // scroll to the center
  
  container[0].scrollTo({
    top: container[0].scrollHeight / 2,
    left: 0,
    behavior: "smooth"
  });
  
});

```

#### (4.3) Scroll the page/element by specific amount at a time using *scrollBy()*

Using the [scrollBy()](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollBy) method we can scroll the document/ element by specific amount at a time. 

```
Syntax:

Element.scrollBy(x-coord, y-coord)
Element.scrollBy(options)

```

* `x-coord`: pixel value you want to scroll by horizontally.
* `y-coord`: pixel value you want to scroll by vertically.
* options:
   * `top`: number of pixels along the Y axis to scroll by.
   * `left`: number of pixels along the X axis to scroll by.
   * `behavior`: smooth/ auto/ instant.


In the following code, the document/ element will be scrolled down by 100px at a time when the button is clicked.

```javascript

btn.addEventListener("click", (e) => {

  // scroll the page to the bottom by 100px at a time
  
  document.documentElement.scrollBy({
    top: 100,
    left: 0,
    behavior: 'smooth'
  });
  

  // scroll the 1st container to the bottom by 100px at a time

  container[0].scrollBy({
    top: 100,
    left: 0,
    behavior: "smooth"
  });
});

```

#### (4.4) Align the page/element to the start, center or end using *scrollIntoView()*

The [scrollIntoView()](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView) method can take two types of parameters and both are optional.

```
Syntax:

Element.scrollIntoView();

Element.scrollIntoView(alignToTop); // Boolean parameter

Element.scrollIntoView(scrollIntoViewOptions); // Object parameter 

```


**alignToTop:** 

It's a boolean value.

* if `true`, the top of the element will be aligned to the top of the visible area of the scrollable ancestor (Default)
* if `false`, the bottom of the element will be aligned to the bottom of the visible area of the scrollable acestor.
 
**scrollIntoViewOptions:** 

It's an object with three optional properties.

* `behavior`: smooth/ auto (Default: auto)
* `block`: defines vertical alignment (start/ center/ end/ nearest) Default: start.
* `inline`: defines horizontal alignment (start/ center/ end/ nearest) Default: nearest.

Also,

`scrollIntoView({block: "start", inline: "nearest"})` corresponds to `true` value.

`scrollIntoView({block: "end", inline: "nearest"})` corresponds to `false` value.

```javascript

btn.addEventListener('click', (e) => {

  // align the page to the end 
  
  document.documentElement.scrollIntoView({
    behavior: 'smooth',
    block: 'end'
  });
  
  // align the 1st container to the center
  
  container[0].scrollIntoView({
    behavior: 'smooth',
    block: 'center'
  });
  
});

```


### 5. Get the scrolling direction

Here, we get the direction of the page/ element that the user is scrolling.

The variable `prevScrollY` store the previous number of pixels that the page/ element was scrolled vertically.

If the current number of pixels is greater than the previous number of pixels then the page/ element has scrolled downward. Otherwise upwards.


```javascript

/* The page: */

let prevScrollY = window.scrollY;

window.addEventListener('scroll', (e) => {
  if(window.scrollY > prevScrollY) {
    console.log('page is scrolling downwards');
  }
  else {
    console.log('page is scrolling upwards');
  }
  prevScrollY = window.scrollY;
});


/* The 1st container: */

let prevScrollY = container[0].scrollTop;

container[0].addEventListener('scroll', (e) => {
  if(container[0].scrollTop > prevScrollY) {
    console.log('1st container is scrolling downwards');
  }
  else {
    console.log('1st container is scrolling upwards');
  }
  prevScrollY = container[0].scrollTop;
});

```

### 6. *window* vs. *documentElement* vs. *Element*

* [Window](https://developer.mozilla.org/en-US/docs/Web/API/Window)

* [documentElement](https://developer.mozilla.org/en-US/docs/Web/API/Document/documentElement)

* [Element](https://developer.mozilla.org/en-US/docs/Web/API/Element)


While you reading through this article you may wonder about some weirdnesses of the above keywords. For instance, we attached the `EventListener` to the `window` but we don't use something like, `window.scrollHeight` or `window.scrollTop`. 

So then, this is the right section to clear them out. Here is some cases where they have some differences from each other.

 **(i) Getting the distance that the document is currently scrolled vertically for the window and the Element**

To do this, `Window` interface has two properties, one is newer than the other.
   * `pageYOffset` (older)
   * `scrollY` (newer)

In the meantime `Element` has `scrollTop`.

**(ii) *window.onscroll* and *document.body.onscroll***

These two are interchangeable, that means if we do some changes to one of them the other one is also inherits those changes as well.

```javascript

window.onscroll = () => {
  console.log('scrolled!!');
};

console.log(window.onscroll === document.body.onscroll); // true

```

**(iii) manipulate scroll event of the window**

To scroll an `element` by a given amount of pixels vertically, we can use,

```javascript

element.scrollTop = 2500;

```

But to scroll the `window` we cannot use `window.scrollY` since it is a read-only property. 

As an alternative, if we use `document.body.scrollTop`. However, this even doesn't work. Because the scrollbar that the browser renders for the document belongs to the `<html>` element, NOT to the `<body>` element.

```javascript

// these both ways aren't valid

window.addEventListener('load', () => {
  
  // window.scrollY = 2500;
  // document.body.scrollTop = 2500;
});

```

At this point `documentElement` comes in to the scene. It returns the `Element` that is the root element of the document in this case it is `<html>` element.

```javascript

// right way to manipulate scroll event for the window

 window.addEventListener('load', () => {
  
  document.documentElement.scrollTop = 2500;
  
});

```

**(iv) *window.innerHeight* and *document.documentElement.clientHeight***

(a) *window.innerHeight*:
* it returns interior height of the window in pixels.
* it includes height of the horizontal scrollbar (if present)

(b) *document.documentElement.clientHeight*:
* it returns visible height of the element.
* includes padding, height of pseudo elements (if any).
* doesn't include margins, borders, horizontal scrollbar.
* a read-only property  (cannot set values to it).
* returns an integer value.

So it is a good idea to choose `document.documentElement` to get the visible height of the window.


**(v) why *window.onscroll* not *document.documentElement.onscroll*?**

Another weird thing is we cannot attach `onscroll` event to the `documentElement` despite it inherits the scrollbar. We just have to use `window.onscroll` in this case.

### Conclusion

Okay, we reached the end of the article. Now we know bit about scroll event and how to manipulate it. So you may feel like it isn't bad as they say, and yes, it is one of the smartest features that the web makes awsome.

Happy Coding!

PS: Thanks for scrolling down so far to the end and, if you feel you really enjoyed this article, you can support me [@ko-fi](https://ko-fi.com/mkdaycode). I always appreciate your support, it really encourages me to keep going.

[![image_description](https://cdn.ko-fi.com/cdn/kofi2.png?v=3)](https://ko-fi.com/mkdaycode)

