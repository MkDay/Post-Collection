# JavaScript preventDefault() - Does it work the way its name suggests? 

# JavaScript preventDefault() - It won't work everywhere as its name suggest

# JavaScript preventDefault() - It won't work with these 5 use cases

Often, almost everyone on the web don't like to use **_default behaviors_** which are provided by default. For instance, as users we have our own preferences about the color or font-size of the text, themes of a web page (eg: dark or light theme) and, in the meantime, as developers we prefer or need to do different things with some events, such as, stop navigating through a link, stop submitting forms, etc. 

It seems in both cases, we all need the web as much as flexible and customizable more than ever. As a developer, you're going to have much hard time working on it! 


So, no one on the web needs default, means they needs to prevent from default. That's where the JavaScript Event.preventDefault() comes in to the play.

**_Note:_** this `Event.preventDefault()` method is not limited to pure JavaScript language, it can be found in languages like React and Jquery. But in this article, let's just stick with pure JavaScript. 

Okay then, let's *prevent* from wasting extra time and dive into the topic. 


At this point, you may be wondering about these two terms [*Event*](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events) and [*Event.preventDefault()*](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault) at the first place.

In brief, *Events* are...

And preventDefault() is... 


Now you may think you can use it to prevent from everything in this world that you want. Actually, you couldn't. 


To realize *why* let's break it down to the following parts,

* Real-world usage - where we can use *Event.preventDefault()*.
* How to check whether the *Event.preventDefault()* has been executed or not - *Event.defaultPrevented*.
* Where does we cannot use *Event.preventDefault()* and some alternatives for it.
 
  * with events that couldn't be canceled
  * to stop event propagation
  * to customize callback functions
     1. customize parameters of callbacks
     1. customize return value of callbacks
* Where does it become a headache - How to prevent the system by checking for *preventDefault()* while executing heavy JavaScript - *passive: true/false*.


### Real-world usage - where we can use *Event.preventDefault()*.


Here is the most common use case where you can use it. Let's say you have a form to fill and, before submitting it you might be asked to read and agree to some policy statement. At that point the form submission might not happend if you didn't check the checkbox followed by some text like *I agree...*. 
 
And here are some more real-world use cases for the `.preventDefault()`

* to prevent the user from refreshing the page. 
* to prevent the user from navigating through a link (anchor element with some URL to another page) after clicking on it.
* to prevent the user from copying content from a web page, etc.   

Now let's see some examples to understand how it works. For instance the following code shows you how to prevent the user from,
* navigating through a link 
* submitting a form.

In our main HTML code has the following important elements.
* `<a>` element with a URL.
* `<form id="form-submission">` element with three types of input elements as its children.
   * `<input type="text" id="input-text">`
   * `<input type="checkbox" id="agreement-checkbox">`
   * `<input type="submit" id="submit-btn">`


**HTML:**

```html

<div id="container">
  <h1>JS Event - preventDefault()</h1>
  
  <a id="link" href="https://www.google.com">Go to the link</a>
  <br><br>
  
  <form id="form-submission">
  
   <label for="input-text">username: </label>
    <input type="text" id="input-text" placeholder="Don't type anything here">
    <br><br>
    
    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>
    <br><br>

    <input id="submit-btn" type="submit" value="Submit">
    <br><br>
    
  </form>

</div>

```

These are the selected HTML elements we're going to use specially within JavaScript.

```javascript

const container = document.querySelector("#container");

const link = document.querySelector("#link");

const form = document.querySelector('#form-submission');

const input = document.querySelector("#input-text");

const checkbox = document.querySelector("#agreement-checkbox");

const btn = document.querySelector("#submit-btn");


```

And, The JavaScript callback functions that use `preventDefault()`.

**JS:**

```javascript

// prevent link

function preventLink(e) {
  e.preventDefault();

  link.textContent = "Link is prevented";

  console.log("Default link behavior is prevented");
}

// prevent submit

function preventSubmit(e) {
  if (!checkbox.checked) {
    e.preventDefault();

    form.innerHTML +=
      "Please check the agreement before submitting the form";

    console.log("Default submit behavior is prevented");
  }
}

```

Now we're going to use those callbacks with `.addEventListener()`, `onclick` within JavaScript and, inline `onclick` attribute to give you a more clarity.

**(i) addEventListener()**

```javascript

// using addEventListener

link.addEventListener("click", preventLink);

btn.addEventListener("click", preventSubmit);

```

**(ii) onclick within JS**

```javascript

// using onclick within JS

link.onclick = preventLink;

btn.onclick = preventSubmit;

```

**(iii) inline onclick attribute**

```html

<div id="container">
  <h1>JS Event - preventDefault()</h1>
  
  <a id="link" href="https://www.google.com" onclick="preventLink(event)">Go to the link</a>
  <br><br>
  
  <form id="form-submission">
  
   <label for="input-text">username: </label>
    <input type="text" id="input-text" placeholder="Don't type anything here">
    <br><br>
    
    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>
    <br><br>

    <input id="submit-btn" type="submit" value="Submit" onclick="preventSubmit(event)">
    <br><br>
    
  </form>

</div>


```

### How to check whether the *Event.preventDefault()* has been executed or not - *Event.defaultPrevented*

Congrats! now you know how to use *preventDefault()* to stop default behaviors. But when you use it in practical code, at some point, you may want to make sure whether it actually works or not. This is the place where [`Event.defaultPrevented`](https://developer.mozilla.org/en-US/docs/Web/API/Event/defaultPrevented) comes into the play.

It is a read-only property that checks whether the `.preventDefault()` of the event has been executed or not. If it has been executed then it returns true, otherwise false. 

```javascript

function preventSubmit(e) {
  if (!checkbox.checked) {
    e.preventDefault();

    form.innerHTML +=
      "Please check the agreement before submitting the form";

    console.log("Default submit behavior is prevented");
  }
  
  console.log(e.defaultPrevented); // true
}

btn.addEventListener('click', preventSubmit);

```

### Where does we cannot use *Event.preventDefault()* and some alternatives for it.

Okay, now you know how to use `.preventDefault()` practically. But hold on, can we use it anywhere we want to stop default behaior?

Actually we couldn't!

Followings are some cases we couldn't use it with.

  1. with events that couldn't be canceled
  1. to stop event propagation
  1. to customize callback functions 
     1. customize parameters of callbacks
     1. customize return value of callbacks
 


### 1. With events that couldn't be canceled


For example, if you suppose to prevent the user from typing in a text input field, unfortunately, you cannot use `.preventDefault()` with the `input` event since it is not cancelable. In other word `input` event is a read-only property.  As an alternative, you can simply use, 


```html

<input type="text" id="input-text" placeholder="Don't type anything here" readonly>

```

So it seems before use `.preventDefault()` for any event you need to know whether the event is `cancelable` (or not a read-only property) or not. If it is not cancelable you cannot use `.preventDefault()` with it.

Okay, how do we check if it is cancelable or not?

#### Check whether an event is cancelable or not - *Event.cancelable*

```javascript

// check whether an event is cancelable or not

// for input event

input.addEventListener("input", (e) => {
  console.log(e.cancelable); // false
});


// for click event

btn.addEventListener('click', (e) => {
  if (e.cancelable) {
    e.preventDefault();
    console.log(e.cancelable); // true
    form.innerHTML += "Please check the agreement before submitting the form";
  }
});

``` 

### 2. To stop event propagation

It is another misunderstanding about the `Event.preventDefault()` method. You cannot stop [Event propagation](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Examples#example_5_event_propagation) using just only `.preventDefault()`. 

See this example:

```javascript

form.addEventListener('click', (e) => {
  form.style.borderColor = 'red';
});


function preventSubmit(e) {
  if (!checkbox.checked) {
    e.preventDefault();

form.innerHTML +=
      "Please check the agreement before submitting the form";

    console.log("Default submit behavior is prevented");
  }
}

btn.addEventListener('click', preventSubmit); 


```
 
In the code above, we attach `Event.preventDefault()` to the *submit button*. So it stops submitting form if the checkbox is unchecked. But, it doesn't stop changing the `border-color` of the `form` into red. It seems, the code executes the `.preventDefault()` at first and, bubbling up to the parent element (in this case, `form`) and executes its event if there is any. 

In simple words, `preventDefault()` cannot stop *event propagation* even it is a default behavior of an event.

If our requirement is only to stop the event propagation we can use [Event.stopPropagation()](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation) instead using `.preventDefault()`

### 3. To customize callback functions

Now at this point we know bit about things that the `.preventDefault()` method can do or cannot do. 

Okay, how about customizing stuff like number of parameters, types of parameters and return value of the callback function using `.preventDefault()`?

For instance,

* callback function of the event listener only have a single parameter which is an object based on Event that has occurred.
* Also, it returns nothing. If there is some return value, it will be ignored.

So, what if we need more parameters or another type of parameter instead an object based on Event?

Also, what if we need our callback function to return something?

Well, the answer is, you cannot customize both behaviors using `Event.preventDefault()` unless you tend to use inline `onclick`. 

Let's see it in practically. 


#### 3.1. Customize parameters of callbacks

**(1) Not working ways - callbacks with custom parameters:**

Normally, if we use callback functions followed by parentheses, it will be immediately invoked even without the event has occurred. So we cannot use parentheses means we cannot have custom parameters instead its default one.

The code below will never work as we expected.

```javascript

 // prevent link - custom callback

function preventLinkCustom(e, num) {
  console.log(num * 2);

  e.preventDefault();

  link.textContent = "Link is prevented";

  console.log("Default link behavior is prevented");
}

// prevent submit - custom callbacks

function preventSubmitCustom(e, num) {
  console.log(num * 3);

  if (!checkbox.checked) {
    e.preventDefault();

    form.innerHTML +=
      "Please check the agreement before submitting the form";

    console.log("Default submit behavior is prevented");
  }
}

```

**(i) addEventListener()**

```javascript

 // using addEventListener()

/* these cannot be used, why because they will be immediately invoked when the page is loaded. */

link.addEventListener("click", preventLinkCustom(event, 12));

btn.addEventListener("click", preventSubmitCustom(event, 12)); 

/* output (immediately invoked, got errors, btn isn't executed)

24

Uncaught TypeError: Cannot read property 'preventDefault' of undefined 

*/

```

**(ii) onclick within JS**

```javascript

 // using onclick within JS

 /* these cannot be used, why because they will be immediately invoked when the page is loaded. */

 link.onclick = preventLinkCustom(event, 12);

btn.onclick = preventSubmitCustom(event, 12);


/* output (immediately invoked, got errors, btn isn't executed)

24

Uncaught TypeError: Cannot read property 'preventDefault' of undefined 

*/

```




**Working ways - callbacks with custom parameters:**

However, there are some alternatives to customize parameters of the callback functions.

In every method we only use inline onclick to handle the *click* event.

**_Note:_** *it is not recommended to use inline-onclick attribute to handle an event as a good practice.*

**Method 1:** 

Using inline `onclick` attribute in the HTML and `.preventDefault()` in its callback function we can easily acheive this requirement like below.

```

// inline onclick

<input id="submit-btn" type="submit" value="Submit" onclick="callback(event, customPara)">

// callback with custom parameters and preventDefault()

function callback(e, customPara) {
 // your custom code goes here
 e.preventDefault();
}

```

JS: 

```javascript 

// prevent link - custom callback 

function preventLinkCustom(e, num) { 

  console.log(num * 2); 

  e.preventDefault(); 
  link.textContent = "Link is prevented"; 
  console.log("Default link behavior is prevented");

  } 


// prevent submit - custom callbacks 

function preventSubmitCustom(e, num) { 

  console.log(num * 3);
  
   if (!checkbox.checked) { 
     e.preventDefault(); 
     container.innerHTML += "Please check the agreement before submitting the form"; 
    console.log("Default submit behavior is prevented"); 
  
   } 
  } 

``` 

HTML: 

```html 

<div id="container">
  <h1>JS Event - preventDefault()</h1>
  
  <a id="link" href="https://www.google.com" onclick="preventLinkCustom(event, 12)">Go to the link</a>
  <br><br>
  
  <form id="form-submission">
  
   <label for="input-text">username: </label>
    <input type="text" id="input-text" placeholder="Don't type anything here">
    <br><br>
    
    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>
    <br><br>

    <input id="submit-btn" type="submit" value="Submit" onclick="preventSubmitCustom(event, 12)">
    <br><br>
    
  </form>

</div>

```

**Method 2:** 

In this method, we use inline `onclick` in the HTML and use there,

* a regular funcrion without *Event* object parameter but your custom parameters.
* return false.

```
// inline onclick

<input id="submit-btn" type="submit" value="Submit" onclick="callback(customPara); return false;">

// regular function without Event object as a parameter

function callback(customPara) {
  // your custom code goes here
}

```

JS:

```javascript

 // prevent link - custom callbacks

function linkCustom(num) {
  console.log(num * 2);

  link.textContent = "Link is prevented";

  console.log("Default link behavior is prevented");
}

// prevent submit - custom callbacks

function submitCustom(num) {
  console.log(num * 3);

  if (!checkbox.checked) {
    container.innerHTML +=
      "Please check the agreement before submitting the form";

    console.log("Default submit behavior is prevented");
  }
}

```

HTML:

```html

<div id="container">
  <h1>JS Event - preventDefault()</h1>
  
  <a id="link" href="https://www.google.com" onclick="linkCustom(12); return false;">Go to the link</a>
  <br><br>
  
  <form id="form-submission">
  
   <label for="input-text">username: </label>
    <input type="text" id="input-text" placeholder="Don't type anything here">
    <br><br>
    
    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>
    <br><br>

    <input id="submit-btn" type="submit" value="Submit" onclick="submitCustom(12); return false;">
    <br><br>
    
  </form>

</div>

```

**Method 3:**

```

// inline onclick

<input id="submit-btn" type="submit" value="Submit" onclick="return callback(customPara);">


// regular function without Event object as a parameter but with a boolean return value

function callback(customPara) {

 // your custom code goes here
 return false;
} 

```

JS:

```javascript

 // prevent link - custom callbacks

function linkCustom(num) {
  console.log(num * 2);

  link.textContent = "Link is prevented";

  console.log("Default link behavior is prevented");
  return false;
}

// prevent submit - custom callbacks

function submitCustom(num) {
  console.log(num * 3);

  if (!checkbox.checked) {
    container.innerHTML +=
      "Please check the agreement before submitting the form";

    console.log("Default submit behavior is prevented");
    return false;
  }
  return true;
}

```

HTML:

```html

<div id="container">
  <h1>JS Event - preventDefault()</h1>
  
  <a id="link" href="https://www.google.com" onclick="return linkCustom(12);">Go to the link</a>
  <br><br>
  
  <form id="form-submission">
  
   <label for="input-text">username: </label>
    <input type="text" id="input-text" placeholder="Don't type anything here">
    <br><br>
    
    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>
    <br><br>

    <input id="submit-btn" type="submit" value="Submit" onclick="return submitCustom(12);">
    <br><br>
    
  </form>

</div>

```

You will get this result for all the above methods,

```javascript

/* output
24
"Default link behavior is prevented"
36
"Default submit behavior is prevented"
*/

```

#### 3.2. Customize return value of callbacks

As shown above, we cannot modify number of parameters or type of parameters using `.preventDefault()` unless we use inline onclick to handle the click event. Now let's check whether it can modify at least the return value that it inherits by default.

By default, the callback function returns nothing, if there is some return value, it will be ignored.

```javascript

/* no error and no effect by adding return value even using preventDefault() within the callback function */

function preventSubmitCustom(e) {
  if (!checkbox.checked) {
    e.preventDefault();

    container.innerHTML +=
      "Please check the agreement before submitting the form";

    console.log("Default submit behavior is prevented");
    return false;
  }
  return true;
}

btn.addEventListener('click', preventSubmitCustom); 

```

This code makes no sense and no effect when it adds to an event listener. So it seems our `.preventDefault()` cannot stop that default behavior too.

However, you can still use inline onclick to handle the click event. In this way it will work.

```html

<!-- check the return type using typeof keyword -->

<input id="submit-btn" type="submit" value="Submit" onclick="console.log(typeof preventSubmitCustom(event));">

<!--output: "boolean"-->

```


### 4. Where does it become a headache - How to prevent the system by checking for *preventDefault()* while executing heavy JavaScript - *passive true/false*


```javascript

document.addEventListener('touchstart', (e) => {
  console.log('before: ' + e.defaultPrevented); // "before: false"
  e.preventDefault();
  console.log('after: ' + e.defaultPrevented); // "after: false"
}, { passive: true});

```

### Summary

* the event should be cancelable to use `.preventDefault()` with it.
* it doesn't stop event propagation.
* it cannot customize parameters of the callback functions unless we use inline onclick.
* it cannot customize the return value of the callback functions unless we use inline onclick.
* passive : false
