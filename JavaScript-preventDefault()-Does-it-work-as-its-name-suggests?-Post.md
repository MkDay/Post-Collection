# JavaScript preventDefault() - Does it work as its name suggests? 

 

Often, almost everyone on the web doesn't like to use **_default behaviors_** which are provided by default. 

For instance, as users, we have preferences about the color or font size of the text, themes of a web page (dark or light theme), etc. 

In the meantime, as developers, we prefer or need to do different things with some events, like stop navigating through a link, stop submitting forms, etc. 

It seems in both cases, we all need the web as much flexible and customizable more than ever!  

You, as a developer, might have a much hard time working on it! 


Well, if no one on the web needs default behaviors, how do we cancel them out and do something else? 

That's the place where the JavaScript *Event.preventDefault()* comes into play.

**_Note:_** the `Event.preventDefault()` method is not limited to pure JavaScript language. You can also find it in languages like React and Jquery. But for now, let's stick only with pure JavaScript. 

Okay, let's *prevent* from wasting extra time and dive into the topic. 


At this point, you may be wondering about these two terms [*Event*](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events) and [*Event.preventDefault()*](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault) at the first place.

In brief, 

**_Event:_** *it can be something the browser does or something the user does.*

**_preventDefault():_** *it cancels out the default behavior that belongs to a particular event.*


Now you may think you can use it to prevent everything in this world that you want. 

Well, you couldn't. 


To realize *why* let's break it down to the following parts,

* Real-world usage - where we can use *Event.preventDefault()*.
* How to check whether the *Event.preventDefault()* has been executed or not - *Event.defaultPrevented*.
* Where does we cannot use *Event.preventDefault()* and some alternatives for it.
 
  * with events that cannot be canceled
  * to stop event propagation
  * to customize callback functions
     1. customize parameters of callbacks
     1. customize return value of callbacks
* Where does the *preventDefault()* becomes a headache?  - *passive true/false*.


### Real-world usage - where we can use *Event.preventDefault()*.


Here is the most common use case where you can use it. 

Let's say you have a form to fill and, before submitting it, you might be asked to read and agree to some policy statement. 

At that point, the form submission might not happen if you didn't check the checkbox followed by some text like *I agree...*. 
 
And here are some more real-world use cases for the `.preventDefault()`:

* to prevent the user from refreshing the page. 
* to prevent the user from navigating through a link (anchor element with some URL to another page).
* to prevent the user from copying content from a web page, etc.   

Now let's see some examples to understand how it works. 

For instance, the following code shows you how to prevent the user from,
* navigating through a link 
* submitting a form.

In our main HTML code has the following elements.
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

Here we select them within JavaScript.

```javascript

const container = document.querySelector("#container");

const link = document.querySelector("#link");

const form = document.querySelector('#form-submission');

const input = document.querySelector("#input-text");

const checkbox = document.querySelector("#agreement-checkbox");

const btn = document.querySelector("#submit-btn");


```

And, here is the JavaScript callback functions where we use the `preventDefault()` method.

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

Now we're going to use those callbacks with,
* `.addEventListener()`
* `onclick` within JavaScript
* inline `onclick` attribute 
to give you some more clarity.

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

Let's say you have already used the `.preventDefalt()` method in your code and, you may want to make sure whether it works or not at some point. 
 
That is the place where [`Event.defaultPrevented`](https://developer.mozilla.org/en-US/docs/Web/API/Event/defaultPrevented) comes into the play.

* It is a read-only property.
 * It returns true if the default behavior of the *event* has been prevented.
* Otherwise, it returns false. 

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

Congrats! we stopped things provided by default.

It's cool, right?

But hold on, can we use it anywhere we want to stop default behaviors of an event?

Actually, we couldn't!

We couldn't use it,
  1. with events that cannot be canceled
  1. to stop event propagation
  1. to customize callback functions 
     1. customize parameters of callbacks
     1. customize return value of callbacks
 


### 1. With events that cannot be canceled


For example, if you suppose to prevent the user from typing in a text input field, unfortunately, you cannot use `.preventDefault()` with the `input` event since it is not cancelable. 

In other words `input` event is a read-only property. As an alternative, you can use the `readonly` attribute like below.


```html

<input type="text" id="input-text" placeholder="Don't type anything here" readonly>

```

So it seems before using `.preventDefault()` to cancel any event, you should find out whether the *event* is `cancelable` or not. If it is not cancelable, you cannot use `.preventDefault()` to cancel it.

Okay, how do we check if it is cancelable or not?

#### Check whether an event is cancelable or not - *Event.cancelable*

The `Event.cancelable` property returns true if the *event* is cancelable otherwise returns false.


```javascript

// check whether an event is cancelable or not

// for input event

input.addEventListener("input", (e) => {
  console.log(e.cancelable); // false
});


// for click event

btn.addEventListener('click', (e) => {
  if (e.cancelable) {
    console.log(e.cancelable); // true
    e.preventDefault();
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
 
In the code above, we attach `Event.preventDefault()` to the *submit button*. So it stops submitting the form if the checkbox is unchecked. But, it doesn't stop changing the `border-color` of the `form` into the red. 

It seems the code works as follows,
* checks for the `click` event of the `btn` element.
* executes the `preventDefault()` 
* hence, it won't submit the form.
* then bubbles up to the parent element (in this case, `form`).
* finds `click` event there and executes it too.

In simple words, `preventDefault()` cannot stop *event propagation* even it is one of the default behaviors of events.

If our requirement is only to stop the event propagation we can use [Event.stopPropagation()](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation) instead using `.preventDefault()`

### 3. To customize callback functions

Now at this point, we know a bit about things that the `.preventDefault()` method can do or cannot do. 

Okay, how about customizing things, like, the number of parameters, types of parameters, and return value of the callback function using `.preventDefault()`?

For instance,

* callback function of the event listener only can have a single parameter which is an object, based on *Event* that has occurred.
* Also, it returns nothing. 
* If there is some return value, it will be ignored.

So, what if we need more parameters or another type of parameter instead of an object, based on *Event*?

Also, what if we need our callback function to return something?

Well, the answer is, you cannot customize both behaviors using `Event.preventDefault()` unless you tend to use the inline `onclick` attribute. 

Let's see it practically. 


#### 3.1. Customize parameters of callbacks

**(1) Not working ways - callbacks with custom parameters:**

If we use callback functions followed by parentheses, it will be immediately invoked even without the *event* has been occurred. So we cannot use parentheses, which means we cannot have custom parameters other than its default one.

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

/* these callbacks are invalid because they will be immediately invoked when the page is loaded. */

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

 /* these callbacks are invalid because they will be immediately invoked when the page is loaded. */

 link.onclick = preventLinkCustom(event, 12);

btn.onclick = preventSubmitCustom(event, 12);


/* output (immediately invoked, got errors, btn isn't executed)

24

Uncaught TypeError: Cannot read property 'preventDefault' of undefined 

*/

```




**(2) Working ways - callbacks with custom parameters:**

However, there are some alternatives to customize *parameters* of the callback functions.

In each method, we only use the *inline-onclick* attribute to handle the *click* event.

**_Note:_** *it is not recommended to use *inline-onclick* attribute to handle an event as a good practice.*

**Method 1:** 

Using `inline-onclick` attribute in the HTML and `.preventDefault()` in its callback function, we can achieve this requirement like below.

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

In this method, we use the *inline-onclick* in the HTML and add there,

* a regular function without *Event* object parameter but your custom parameters.
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

As shown above, we cannot modify the number of parameters or type of parameters using `.preventDefault()` unless we use the *inline-onclick* to handle the click event. Now let's check whether it can modify the return value that it inherits by default.

By default, the callback function returns nothing. If there is some return value, it will be ignored.

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

This code makes no sense and has no effect when it adds to an event listener. So it seems our `.preventDefault()` cannot stop that default behavior too.

However, you can still use inline onclick to handle the click event. In this way, it will work.

```html

<!-- check the return type using typeof keyword -->

<input id="submit-btn" type="submit" value="Submit" onclick="console.log(typeof preventSubmitCustom(event));">

<!--output: "boolean"-->

```


### 4. Where does the *preventDefault()* becomes a headache?  - *passive true/false*

The `Event.addEventListener()` has two optional parameters and one of them is **_options_**.


**Syntax**

```
addEventListener(type, listener);
addEventListener(type, listener, options);
addEventListener(type, listener, useCapture);

```

The *passive* is one of the available options.

> **passive:** *boolean value that, if true, indicates that the function specified by the listener will never call preventDefault(). If a passive listener does call preventDefault(), the user agent will do nothing other than generate a console warning.* **- MDN**

It is more useful with touch events on mobile browsers. 

For example, suppose we perform a `touchstart` event on the mobile. 

When a `touchstart` event has occurred, 
* The browser will check whether there is an `Event.preventDefault()` method to execute within the callback function. 
* This checking will happen even you don't use the `Event.preventDefault()` within the callback.

Also, there will be a small time gap between each two `touchstart` events. Maybe it is smaller than the execution time of a single `touchstart` event. It will cause to slow down the web page running.

So this is the place where the `.preventDefault()` becomes a headache since the *event* will slow down the page running, just by only checking for the `preventDefault()` method.

To prevent it, we can use the `{passive: true}` option.

It tells the browser not to check for the `Event.preventDefault()` method and prevents the page from slowing down.

**_Note:_** *Most of the modern browsers use passive listeners by default for mobile touch events. So you don't need to use `{passive: true}` there. However, it's still good to use while considering all browsers. 



```javascript

document.addEventListener('touchstart', (e) => {
  console.log('before: ' + e.defaultPrevented); // "before: false"
  e.preventDefault();
  console.log('after: ' + e.defaultPrevented); // "after: false"
}, { passive: true});

```

### Conclusion

 At this point, I hope you have some sense of preventing default behaviors of an event. To wrap this out, let me summarize what we discussed in this article.

* to use `preventDefault()` method, the event should be cancelable - `Event.cancelable=true`
* it cannot stop the event propagation.
* it cannot modify default parameters and return values of a callback function.
* sometimes it becomes a headache even where we haven't used it. To overcome it, use `{passive: true}` as an option within the event listener.
* use the `Event.defaultPrevented` property to check whether the default behavior of an event is prevented or not.


**_Photo Credits: Jon Tyson on Unsplash_**


If you enjoyed this article, you can support me at [ko-fi](https://ko-fi.com/mkdaycode). I always appreciate your support, it really encourages me to keep going.

**_Happy Coding!_**

[![image_description](https://cdn.ko-fi.com/cdn/kofi2.png?v=3)](https://ko-fi.com/mkdaycode)
