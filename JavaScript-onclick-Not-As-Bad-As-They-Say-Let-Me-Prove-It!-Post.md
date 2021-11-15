# JavaScript-onclick-Not-As-Bad-As-They-Say-Let-Me-Prove-It!-Post
Post Pending...

# JavaScript onclick Not As Bad As They Say Let Me Prove It!
 
__*Warning!*__

MDN docs recommands `addEventListener` instead of `onclick` as follows.

[It says](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener),

> **The addEventListener() method is the recommended way to register an event listener. The benefits are as follows:**
>
> * It allows adding more than one handler for an event. This is particularly useful for libraries, JavaScript modules, or any other kind of code that needs to work well with other libraries or extensions.
>
> * In contrast to using an *onXYZ* property, it gives you finer-grained control of the phase when the listener is activated (capturing vs. bubbling).
>
> * It works on any event target, not just HTML or SVG elements.

It sounds like a discouraging statement about the use of `onclick`. However, `onclick` can compete with `addEventListener` for the most part. 

To prove that, let's consider the following simple program.
 
There are a child `button` element and its parent `div` element. And there is a function named *calculate* to use as the event handler when the `button` gets clicked.

###### HTML 
```html
<div id="container">
  
 <button id="btn-add">Add</button>
    
</div>

```

### __*onclick*__ Works Well For The Following Use-cases
#### 1. Event Delegation (Multiple elements - single handler)

Using [event delegation](https://davidwalsh.name/event-delegate), we can add one event handler only for the parent element and recognize the current child element that the click event is occurred on, using [`event.target.matches()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/matches).

```javascript
let container = document.querySelector('#container');
let addBtn = document.querySelector('#btn-add');

let num1 = 6;
let num2 = 2;
let result = 0;

function calculate(e) {  
    if(e.target && e.target.matches('#btn-add')) {
      result += num1 + num2;
      console.log(`result: ${result}`);
    }    
}

```

##### Event delegation - addEventListener
```javascript

//addEventListener
container.addEventListener('click', calculate);

// output after clicking the button 3 times.
/*
"result: 8"
"result: 16"
"result: 24"
*/

```

##### Event delegation - onclick
```javascript

//onclick
container.onclick = calculate;

// output after clicking the button 3 times.
/*
"result: 8"
"result: 16"
"result: 24"
*/

```
#### 2. Event Bubbling & Capture

I don't suppose to explain [*bubbling and capturing*](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_bubbling_and_capture) here. However, it is good to mention that the bubbling event is the default behavior of almost every modern browser.

The `addEventListener` has an option to use event bubbling or capture and, it is pretty clear that there is no such option with `onclick` for the capturing phase. 

```javascript

let container = document.querySelector('#container');
let addBtn = document.querySelector('#btn-add');

let num1 = 6;
let num2 = 2;
let result = 0;

function calculate(e) {
  result += num1 + num2;
  console.log(`calculated result: ${result}`);
}

```

First, we retrieve the *calculated result* using the event handler of the `button`.

And then display the result on the `div` as the *current result*. 

Bubbling works well in this case for both `onclick` and `addEventListener`. 

##### Bubbling - addEventListener
```javascript

// addEventListener - bubbling
// display current result after calculating 

container.addEventListener('click', function() {
  console.log(`current result: ${result}`);
});

addBtn.addEventListener('click', calculate);

// output after clicking the button 3 times.
/*
"calculated result: 8"
"current result: 8"
"calculated result: 16"
"current result: 16"
"calculated result: 24"
"current result: 24"
*/

```
##### Bubbling - onclick
```javascript

// onclick - bubbling
// display current result after calculating 

container.onclick = function() {
  console.log(`current result: ${result}`);
}

addBtn.onclick = calculate;

// output after clicking the button 3 times.
/*
"calculated result: 8"
"current result: 8"
"calculated result: 16"
"current result: 16"
"calculated result: 24"
"current result: 24"
*/

```

Now we first display the result as *previous result* on the `div` and then retrieve the *calculated result* using the event handler of the `button`. 

Here we specify the optional argument of the `addEventListener` which is, [*useCapture*](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#syntax) as *true* for the parent element.

##### Capture - addEventListener
```javascript

// addEventListener - capturing 
// display previous result before calculating 

container.addEventListener('click', function() {
  console.log(`previous result: ${result}`);
}, true);

addBtn.addEventListener('click', calculate);

// output after clicking the button 3 times.
/*
"previous result: 0"
"calculated result: 8"
"previous result: 8"
"calculated result: 16"
"previous result: 16"
"calculated result: 24"
*/

```
We cannot use the event capture with the `onclick`. However, this is kind of achievable using [event delegation](https://davidwalsh.name/event-delegate).

##### Capture - onclick (using event delegation)
```javascript

// onclick - capturing 
// display previous result before calculating 

container.onclick = function(e) {
  console.log(`previous result: ${result}`);
  if(e.target && e.target.matches('#btn-add')) {
    calculate();
  }
}

// output after clicking the button 3 times.
/*
"previous result: 0"
"calculated result: 8"
"previous result: 8"
"calculated result: 16"
"previous result: 16"
"calculated result: 24"
*/

```

#### 3. Remove event listeners

Here we add the `num1 + num2` to the `result` only once and stop listening to the event after the first click has occurred.

There is a method called [`removeEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener), which accepts the same arguments that assigned to the `addEventListener` previously. It removes the previously-added *event listener* from the element.

```javascript
let container = document.querySelector('#container');
let addBtn = document.querySelector('#btn-add');

let num1 = 6;
let num2 = 2;
let result = 0;

function calculate(e) {
  result += num1 + num2;
  console.log(`element: button - result: ${result}`);
}

```

##### addEventListener - before removing listener

```javascript

container.addEventListener('click', function(e) {
  console.log(`element: div - result: ${result}`);
});
addBtn.addEventListener('click', calculate);
 
// output after clicking the button 3 times.
/*
"element: button - result: 8"
"element: div - result: 8"
"element: button - result: 16"
"element: div - result: 16"
"element: button - result: 24"
"element: div - result: 24"
*/

```
##### addEventListener - after removing listener

```javascript

container.addEventListener('click', function(e) {
  addBtn.removeEventListener('click', calculate);
  console.log(`element: div - result: ${result}`);
});
addBtn.addEventListener('click', calculate);

// output after clicking the button 3 times.
/*
"element: button - result: 8"
"element: div - result: 8"
"element: div - result: 8"
"element: div - result: 8"
*/

```

There is no obvious way to remove the `onclick` event, but if we make the `onclick` attribute as `null` it will do the job as we supposed.

 
##### onclick - before removing listener

```javascript

container.onclick = function(e) {
  console.log(`element: div - result: ${result}`);
}
addBtn.onclick = calculate;
 
// output after clicking the button 3 times.
/*
"element: button - result: 8"
"element: div - result: 8"
"element: button - result: 16"
"element: div - result: 16"
"element: button - result: 24"
"element: div - result: 24"
*/

```
##### onclick - after removing listener 
 
```javascript

container.onclick = function(e) {
  addBtn.onclick = null;
  console.log(`element: div - result: ${result}`);
}
addBtn.onclick = calculate;
 
// output after clicking the button 3 times.
/*
"element: button - result: 8"
"element: div - result: 8"
"element: div - result: 8"
"element: div - result: 8"
*/

```

### No Options Other Than __*addEventListener*__

#### 1. Event Overwriting (Single element - multiple handlers)

The `addEventListener` works well with the following two handlers. 
* First handler - *calculate*: calculates the result.
* Second handler - *showResult*: shows the result.

If we use `onclick` in this case, the second handler will overwrite the first one, so we will never get the calculated result. 

```javascript
let container = document.querySelector('#container');
let addBtn = document.querySelector('#btn-add');

let num1 = 6;
let num2 = 2;
let result = 0;

function calculate(e) {
  if(e.target) {
    result += num1 + num2;
  }
}

function showResult(e) {
  if(e.target) {
    console.log(`result: ${result}`); 
  }
}

```

##### Using __*addEventListener*__

```javascript
// addEventListener

addBtn.addEventListener('click', calculate);
addBtn.addEventListener('click', showResult); 

// output after clicking the button 3 times.
/*
"result: 8"
"result: 16"
"result: 24"
*/

```

##### Using __*onclick*__

```javascript
// onclick

addBtn.onclick = calculate;
addBtn.onclick = showResult; 

// output after clicking the button 3 times.
/*
"result: 0"
"result: 0"
"result: 0"
*/

```

### Let's Summarize

Now you can see `onclick` can do almost everything except registering multiple handlers to a single element. However, it is good to mention that there are many things to consider before selecting the right one for your specific needs. This post is just here to prove that there are still some cases where we can use `onclick`.

*Image Credit: Artem Bryzgalov on Unsplash* 
