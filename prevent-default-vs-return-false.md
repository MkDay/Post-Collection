# JavaScript preventDefault - Does it work the way its name suggests? 

Often, almost everyone don't like to use **_default behaviors_** which are provided by default on the web (or even maybe in every case in this world). For instance, as users we have our own preferences about the color or font-size of the text, themes of a web page (eg: dark or light theme), in the meantime, as developers we like or need to do different things with some events, such as, navigating through a link, submitting forms, etc. 

It seems in both cases, we all need the web as much as flexible and customizable more than ever. As a developer, you're going to have much hard time working on it! 


So, no one on the web needs default, means they needs to prevent from default. That's where the JavaScript [Event.preventDefault()]() comes in to the play.

Okay then, let's *prevent* from wasting extra time and dive into the topic. 

At this point, you may be wondering about these two words [*Event*](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events) and [*.preventDefault()*](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault) in the JavaScript at the first place.


In brief, *Events* are...

And preventDefault() is... 




So, here is the summary we're going to discuss in this article.

* What to consider before using *Event.preventDefault()*.
* Where does we cannot use it.
* Real-world usage.
* How about callback functions with custom parameters?
  1. use of *preventDefault()*
  2. use of *return false*
  3. working ways for callback functions with custom parameters 

### What to consider before using *Event.preventDefault()*.

Okay, now you may think you can prevent from everything that you want using this method. If so you're wrong. 

  * target the right event (eg: &keyup event doesn't& stop typing)

  * first check if the event is cancelable

  * it doesn't stop the event propagation




### Where does we cannot use it? 

(eg: scroll event, wheel event)



### Real-world usage

* stop navigating through a link
* submitting forms



/*========= ### 1. using preventDefault() ========== */


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

    container.innerHTML +=
      "Please check the agreement before submitting the form";

    console.log("Default submit behavior is prevented");
  }
}

```

**(i) addEventListener()**

```javascript

// using addEventListener

link.addEventListener("click", preventLink);

btn.addEventListener("click", preventSubmit);

```

**(ii) onclick inside <script>**

```javascript

// using onclick inside <script>

link.onclick = preventLink;

btn.onclick = preventSubmit;

```

**(iii) inline onclick attribute**

```html

<div id="container">

  <a id="link" href="https://www.google.com" onclick="preventLink(event)">Go to the link</a>

  <form>

    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>

    <br>

    <input id="submit-btn" type="submit" value="Submit" onclick="preventSubmit(event)">

  </form>

</div>


```

/* ================== extra-start ============================== */

**Is it same the both preventDefault() & return false (no jquery)**

### 2. using return false

```javascript

 // prevent link using return false

function returnFalseLink(e) {
  link.textContent = "Link is prevented";

  console.log("Default link behavior is prevented");

  return false;
}

// prevent submit using return false

function returnFalseSubmit(e) {
  if (!checkbox.checked) {

    container.innerHTML +=
      "Please check the agreement before submitting the form";

    console.log("Default submit behavior is prevented");

    return false;
  }

  return true;
}

```

**(i) addEventListener()**

```javascript

 // using addEventListener

link.addEventListener("click", returnFalseLink); // not working (at first code executed then go to the link)

btn.addEventListener("click", returnFalseSubmit); // working

```

**(ii) onclick inside <script>**

```javascript

 // using onclick inside <script>

link.onclick = returnFalseLink; // working

btn.onclick = returnFalseSubmit; // working

```

**(iii) inline onclick attribute**

```html

<div id="container">

  <a id="link" href="https://www.google.com" onclick="returnFalseLink(event)">Go to the link</a>

  <form>

    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>

    <br>

    <input id="submit-btn" type="submit" value="Submit" onclick="returnFalseSubmit(event)">

  </form>

</div>

```


**there are 2 not working results in the code examples so explain why is it.**


/* ================== extra-end ============================== */


### How about callback functions with custom parameters?



### 1. Use of *preventDefault()*

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

    container.innerHTML +=
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

/* output (immediately, invoked got errors, btn isnt executed)

24

Uncaught TypeError: Cannot read property 'preventDefault' of undefined 

*/

```

**(ii) onclick inside <script>**

```javascript

 // using onclick inside <script>

 /* these cannot be used, why because they will be immediately invoked when the page is loaded. */

 link.onclick = preventLinkCustom(event, 12);

btn.onclick = preventSubmitCustom(event, 12);


/* output (immediately, invoked got errors, btn isnt executed)

24

Uncaught TypeError: Cannot read property 'preventDefault' of undefined 

*/

```


### 2. Use of *return false*

```javascript

 // prevent link using return false

function returnFalseLinkCustom(e, num) {
  console.log(num * 2);

  link.textContent = "Link is prevented";

  console.log("Default link behavior is prevented");

  return false;
}

// prevent submit using return false

function returnFalseSubmitCustom(e, num) {
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

**(i) addEventListener()**

```javascript

 // using addEventListener()

link.addEventListener("click", returnFalseLinkCustom(event, 12));

btn.addEventListener("click", returnFalseSubmitCustom(event, 12));

/* output: (immediately invoked & got errors)

24
"Default link behavior is prevented"
Uncaught TypeError: Failed to execute 'addEventListener' on 'EventTarget': The callback provided as parameter 2 is not an object. 

*/

```

**(ii) onclick inside <script>**

```javascript

// using onclick inside <script>


link.onclick = returnFalseLinkCustom(event, 12);

btn.onclick = returnFalseSubmitCustom(event, 12); 

/* output (immediately invoked & no errors)

24

"Default behavior is prevented"

36

"Default behavior is prevented"

*/

```

**(iii) inline onclick attribute**

```html

 <div id="container">

  <a id="link" href="https://www.google.com" onclick="returnFalseLinkCustom(event, 12)">Go to the link</a>

  <form>

    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>

    <br>

    <input id="submit-btn" type="submit" value="Submit" onclick="returnFalseSubmitCustom(event, 12)">

  </form>

</div>

```
```
 /* output : for link 
 * doesn't execute until press link
 * after pressing the link executed but redirected

24

"Default behavior is prevented"

*/

/* output : for submit working

36

"Default behavior is prevented"

*/
 
 ```

### 3. Working ways for callback functions with custom parameters

**Method 1:** 

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

// using inline onclick 

 <div id="container">

  <a id="link" href="https://www.google.com" onclick="preventLinkCustom(event, 12)">Go to the link</a>

  <form>

    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>

    <br>

    <input id="submit-btn" type="submit" value="Submit" onclick="preventSubmitCustom(event, 12)">

  </form>

</div>

```

**Method 2:**

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

  <a id="link" href="https://www.google.com" onclick="linkCustom(12); return false;">Go to the link</a>

  <form>

    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>

    <br>

    <input id="submit-btn" type="submit" value="Submit" onclick="submitCustom(12); return false;">

  </form>

</div>

```

**Method 3:**

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

  <a id="link" href="https://www.google.com" onclick="return linkCustom(12);">Go to the link</a>

  <form>

    <input id="agreement-checkbox" type="checkbox">

    <label for="agreement-checkbox">I agree</label>

    <br>

    <input id="submit-btn" type="submit" value="Submit" onclick="return submitCustom(12);">

  </form>

</div>

```

```
/* output
24
"Default link behavior is prevented"
36
"Default submit behavior is prevented"
*/
```
