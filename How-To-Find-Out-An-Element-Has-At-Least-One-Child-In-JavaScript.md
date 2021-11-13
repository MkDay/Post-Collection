# How To Find Out An Element Has At Least One Child In JavaScript
 

If you just begin with JavaScript [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction), definitely you're going to have this problem, now or later.

Let's say,

we create child elements and append them to the parent element dynamically at the run time using following methods, 

1. create elements using [`document.createElement()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement)
2. append them to the parent using [`parent.appendChild()`](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild)

Also before appending new child elements to the parent element, we should remove or replace existing child elements. 

In this case there are a couple of choices,

1. remove all the existing child elements of the parent element using [`parent.innerHTML = '';`](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)
2. remove specfic existing child element using [`parent.removeChild(oldChild)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild)
3. replace existing child element with new one using [`parent.replaceChild(newChild, oldChild)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/replaceChild) or [`oldChild.replaceWith(newChild)`](https://developer.mozilla.org/en-US/docs/Web/API/Element/replaceWith)

However, if we want to remove or replace child elements from the parent element first we have to find out,  if there are any kind of child elements,

1. including [*comments*](https://developer.mozilla.org/en-US/docs/Web/API/Comment) and *whitespaces* which considers as [*Text*](https://developer.mozilla.org/en-US/docs/Web/API/Text).
2. excluding comments and whitespaces. (this will be more helpful when we are going to replace existing specific child element).


### Bring it to the life

Here is the actual program. 

In the HTML, there is a parent `div` element with the `id` *container* without any child elements but with whitespaces and comments.

Also there is a `select` element with the `id` *input-type* to switch between two `option` with values,
* *number*
* *text*

```html

<select id="input-type">
  <option value="">Select Name Or Age</option>
  <option value="text">Name</option>
  <option value="number">Age</option>
</select>

<div id="container">
  <!-- 
 here this div has whitespaces and comments..
 -->
</div>

```

In the JavaScript, we have two created `input` elements,
 
1. with the `type="number"` and the `id="number-input"` 
2. with the `type="text"` and the `id="text-input"`.

And append them to the parent at the run time based on the user selection.

So the user can choose to enter either his *Name* or *Age* as a single input.

```javascript

let inputType = document.querySelector("#input-type");
let container = document.querySelector("#container");

let inputText = document.createElement("input");
inputText.setAttribute("id", "text-input");
inputText.setAttribute("type", "text");
inputText.setAttribute("placeholder", "Enter your name");

let inputNumber = document.createElement("input");
inputNumber.setAttribute("id", "number-input");
inputNumber.setAttribute("type", "number");
inputNumber.setAttribute("placeholder", "Enter your age");

inputType.addEventListener("change", generateInputs);

function generateInputs() {
  if (inputType.value === "number") {
    generateNumber();
  } else if (inputType.value === "text") {
    generateText();
  }
}

```

Also there are three functions within JavaScript.

1. *generateNumber()*
2. *generateText()*
3. *hasChildElements(parent)*

And we're going to use first and second functions in two different ways based on the removing or replacing purpose.

##### 1. functions with *innerHTML = '';* or *removeChild()* methods.

```javascript

function generateNumber() {
  if (hasChildElements(container)) {
    // container.innerHTML = '';
    container.removeChild(inputText);
  }
  container.appendChild(inputNumber);
}

function generateText() {
  if (hasChildElements(container)) {
    // container.innerHTML = '';
    container.removeChild(inputNumber);
  }
  container.appendChild(inputText);
}

```

##### 2. functions with *replaceChild()* or *replaceWith()* methods.

 ```javascript

function generateNumber() {
  if (hasChildElements(container)) {
    container.replaceChild(inputNumber, inputText);
   // inputText.replaceWith(inputNumber);
  } else {
    container.appendChild(inputNumber);
  }
}

function generateText() {
  if (hasChildElements(container)) {
    container.replaceChild(inputText, inputNumber);
   // inputNumber.replaceWith(inputText);
  } else {
    container.appendChild(inputText);
  }
}

```
### Check for child elements : *NodeList* and *HTMLCollection* 

Let's assume, the user choose to enter his *Name* and instantly change his mind and choose to enter his *Age* as the input. In this case if we didn't remove existing `input` then, both the `input` *Name* and *Age* might be appearing on the page.

To remove existing child elements we can check for them,

1. including comments and whitespaces.
2. excluding comments and whitespaces. 

Fortunately, [*NodeList*](https://developer.mozilla.org/en-US/docs/Web/API/NodeList) object considers *comments* and *whitespces* as the parts of it, in the mean time, [*HTMLCollection*](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection) doesn't.

#### 1. Check including comments and whitespaces

This is good if we just want to clear out all the children from the parent element using `parent.innerHTML = ''; `.

However, if we use `removeChild()`, `replaceChild()` methods we might get errors like below, when there is no any actual child, other than comments and whitespaces.

*Uncaught NotFoundError: Failed to execute 'replaceChild' on 'Node': The node to be replaced is not a child of this node*

With the `replaceWith()` we might not get errors but also it won't append the new child elements either.

Alternatively, we can iterate through the parent element and use those methods to remove or replace each child from the parent.

##### 1.1. Find children using [*innerHTML*](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)

```javascript
 function hasChildElements(parent) {
  if (parent.innerHTML !== "") {
    console.log(parent.innerHTML);
    return true;
  }
console.log('no children');
  return false;
}

// or you can use innerHTML.trim() to remove whitespaces first

// output : with .innerHTML = ''; 
// after choosing 3 input type: text > number > text

/*
" <!-- here this div has whitespaces and comments.. --> "

"<input id='text-input' type='text' placeholder='Enter your name'>"

"<input id='number-input' type='number' placeholder='Enter your age'>"
*/
```

##### 1.2. Find children using [*hasChildNodes()*](https://developer.mozilla.org/en-US/docs/Web/API/Node/hasChildNodes)

```javascript

function hasChildElements(parent) {
  if (parent?.hasChildNodes()) {
    console.log(parent?.childNodes);
    return true;
  }
  console.log("no chidren");
  return false;
}

 
// output : with .innerHTML = ''; 
// after choosing 3 input type: text > number > text

 /*
// [object NodeList] (3)
["#text","#comment","#text"]

// [object NodeList] (1)
["<input/>"]

// [object NodeList] (1)
["<input/>"]
*/

```

 ##### 1.3. Find children using [*firstChild*](https://developer.mozilla.org/en-US/docs/Web/API/Node/firstChild)

```javascript

function hasChildElements(parent) {
  if (parent?.firstChild) {
    console.log(parent?.firstChild);
    return true;
  }
  console.log("no chidren");
  return false;
}

// output : with .innerHTML = ''; 
// after choosing 3 input type: text > number > text

/*
// [object Text] 
{}

<input id="text-input" type="text" placeholder="Enter your name">

<input id="number-input" type="number" placeholder="Enter your age">
*/

```

##### 1.4. Find children using [*childNodes.length*](https://developer.mozilla.org/en-US/docs/Web/API/Node/childNodes)

```javascript

function hasChildElements(parent) {
  if (parent?.childNodes.length > 0) {
    console.log(`number of childNodes: ${parent?.childNodes.length}`);
    console.log(parent?.childNodes);
    return true;
  }
  console.log("no chidren");
  return false;
}

// output : with .innerHTML = ''; 
// after choosing 3 input type: text > number > text

/*
"number of childNodes: 3"
// [object NodeList] (3) 
["#text","#comment","#text"]

"number of childNodes: 1"
// [object NodeList] (1) 
["<input/>"]

"number of childNodes: 1"
// [object NodeList] (1) 
["<input/>"]
*/

```

#### 2. Check excluding comments and whitespaces.

Following ways will work with all `innerHTML = ''; `, `removeChild()`, `replaceChild()` & `replaceWith()` methods which mentioned before.


 ##### 2.1. Find children using [*childNodes*](https://developer.mozilla.org/en-US/docs/Web/API/Node/childNodes) & [*nodeType*](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType)
 
```javascript

function hasChildElements(parent) {
  for (let i = 0; parent?.childNodes[i]; i++) {
    if (parent?.childNodes[i]?.nodeType === 1) {
      console.log(parent.childNodes);
      return true;
    }
  }
  console.log("no chidren");
  return false;
}

// output : after choosing 3 input type: text > number > text
 /*
"no chidren"

// [object NodeList] (4) 
["#text","#comment","#text","<input/>"]

// [object NodeList] (4) 
["#text","#comment","#text","<input/>"]
*/

```

##### 2.2. Find children using [*firstChild*](https://developer.mozilla.org/en-US/docs/Web/API/Node/firstChild), [*nextSibling*](https://developer.mozilla.org/en-US/docs/Web/API/Node/nextSibling) & [*nodeType*](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType)

```javascript

function hasChildElements(parent) {
  let child;
  for (child = parent?.firstChild; child; child = child.nextSibling) {
    if (child.nodeType == 1) {
      console.log(child);
      return true;
    }
  }
  console.log("no children");
  return false;
}

// output : after choosing 3 input type: text > number > text
/*
"no children"

<input id="text-input" type="text" placeholder="Enter your name">

<input id="number-input" type="number" placeholder="Enter your age">
*/

```

##### 2.3. Find children using [*firstElementChild*](https://developer.mozilla.org/en-US/docs/Web/API/Element/firstElementChild)

```javascript

function hasChildElements(parent) {
  if (parent?.firstElementChild) {
    console.log(parent?.firstElementChild);
    return true;
  }
  console.log("no chidren");
  return false;
}

// output : after choosing 3 input type: text > number > text
/*
"no chidren"

<input id="text-input" type="text" placeholder="Enter your name">

<input id="number-input" type="number" placeholder="Enter your age">
*/

```

##### 2.4. Find children using [*children.length*](https://developer.mozilla.org/en-US/docs/Web/API/Element/children)

```javascript

function hasChildElements(parent) {
  if (parent?.children.length > 0) {
    console.log(`number of children: ${parent?.children.length}`);
    console.log(parent?.children);
    return true;
  }
  console.log("no chidren");
  return false;
}

// output : after choosing 3 input type: text > number > text
/*
"no chidren"

"number of children: 1"
// [object HTMLCollection] 
{
 "0": {}
}

"number of children: 1"
// [object HTMLCollection] 
{
 "0": {}
}
*/

```

##### 2.5. Find children using [*childElementCount*](https://developer.mozilla.org/en-US/docs/Web/API/Element/childElementCount)

```javascript

function hasChildElements(parent) {
  if (parent?.childElementCount > 0) {
    console.log("childElementCount: " + parent?.childElementCount);
    return true;
  }
  console.log("no chidren");
  return false;
}

// output : after choosing 3 input type: text > number > text
/*
"no chidren"
"childElementCount: 1"
"childElementCount: 1"
*/

```

### Choose the best out of all

Now we have two sets of ways to do our job. However, more importantly, before using any kind of code in your project, it is the best practice to check browser compatibility of it. 

Also there is no *best of all* that mentioning here, you are the one who always qualify to mention your best choice based on your experience. See you in the comment section.

Happy Coding!

