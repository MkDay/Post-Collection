# How To Find Out If An Element Has Children In JavaScript

**_Published @DEV 22/11/2021_**

  
If you are still getting familiar with JavaScript [**DOM**](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction), definitely you're going to have this problem, now or later.

There are a couple of properties and methods to check for children of a parent element in JavaScript. 

However, you get completely different results depending on how they treat the parent element as *Node* or *Element*.


### Bring it to the life

Here we have a simple program to check for the child elements within a parent element. 

In the HTML, there is a parent `div` element with the `id="parent"`. It has no child elements but with some whitespaces and comments. 

Also, there is a `button` with the `id="btn"`

```html

<div id="parent">
  <!-- 
 here this div has whitespaces and comments..
 -->
</div>

<button id="btn">Add Children</button>

```

In JavaScript, there is a created *paragraph* element with some text in it. 

When the user clicks the button, the `function addChildren() {}` will be getting called. If there isn't any existing child element in the *parent*, the function will append the paragraph to the *parent* at the run time. Otherwise, it won't append anything to the *parent* at all.

```javascript

let parent = document.querySelector("#parent");
let btn = document.querySelector("#btn");
let child = document.createElement("p");
child.textContent = "this is a new child element..";

btn.addEventListener("click", addChildren);

function addChildren(e) {
  if (!hasChildElements(parent)) {
    parent.appendChild(child);
    console.log("So a new paragraph has been added");
  }
}

```
To check for the child elements we are going to create another function called `function hasChildElements(parent) {}`

### 1. The quicker way to check for children

First, we try the easier way to do the job.

#### 1.1. Find children using *innerHTML*

The [`parent.innerHTML`](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML) property can be used to examine the current HTML source of an element. 

It returns the HTML source of the element as a string if there is any.

The following function checks if there any HTML in the parent element including *elements*, *comments* and *whitespaces* etc. 

So our function returns true, which means the element has children, in this case, *comments* and *whitespaces*.


```javascript
 function hasChildElements(parent) {
  if (parent.innerHTML !== "") {
    console.log(parent.innerHTML);
    return true;
  }
  console.log('no children');
  return false;
}

// output
/*
" <!-- here this div has whitespaces and comments.. --> "
*/

```

So when checking for children of an element, we must be careful not to get *non-element* children that appear within a parent element.

### 2. Comments and whitespaces as children

`#comment` and *whitespaces* belong to `#text` are both considered as the *nodes* of the DOM tree.  

So if we use some of the properties or methods that belong to the [**Node**](https://developer.mozilla.org/en-US/docs/Web/API/Node) interface to get the child elements of a parent element, we might get *comment* and *whitespace* as child elements if there any.

In short, we get all sorts of *nodes* in the *DOM* tree instead of specifically getting *elements*.

Let's see some examples.
#### 2.1. Find children using *hasChildNodes()*

The [*parent.hasChildNodes()*](https://developer.mozilla.org/en-US/docs/Web/API/Node/hasChildNodes) method of the *Node* interface returns *true*, if the given *Node* has child nodes, otherwise it returns *false*.

Also, [*childNodes*](https://developer.mozilla.org/en-US/docs/Web/API/Node/childNodes) property of the *Node* gives a list of child nodes that appears within the parent node. 

In the output, we get three child nodes representing *comments* and *whitespaces*.


```javascript

function hasChildElements(parent) {
  if (parent?.hasChildNodes()) {
    console.log(parent?.childNodes);
    return true;
  }
  console.log("no children");
  return false;
}

 
// output 
/*
// [object NodeList] (3) 
["#text","#comment","#text"]
*/

```

 #### 2.2. Find children using *firstChild*

The [*parent.firstChild*](https://developer.mozilla.org/en-US/docs/Web/API/Node/firstChild) property returns first child of the parent node.

In this case, it returns the `#text` node representing *whitespace* as the first child of the parent node.

```javascript

function hasChildElements(parent) {
  if (parent?.firstChild) {
    console.log(parent?.firstChild);
    return true;
  }
  console.log("no children");
  return false;
}

// output 
/*
 // [object Text] 
 {}
*/

```

#### 2.3. Find children using *childNodes.length*

The [*parent.childNodes*](https://developer.mozilla.org/en-US/docs/Web/API/Node/childNodes) is an object of the parent node. So we can check the number of children it has, using the `length` property.

Here we get three child nodes, only representing *comments* and *whitespaces*.

```javascript

function hasChildElements(parent) {
  if (parent?.childNodes.length > 0) {
    console.log(`number of childNodes: ${parent?.childNodes.length}`);
    console.log(parent?.childNodes);
    return true;
  }
  console.log("no children");
  return false;
}

// output 
/*
"number of childNodes: 3"
// [object NodeList] (3) 
["#text","#comment","#text"]
*/

```

### 3. Ignore comments and whitespaces

As we saw before, using properties or methods of the *Node* interface, we cannot get the actual child elements separately. 

However, the *Node* interface has another property called [*nodeType*](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType) to check the type of the child node.

After checking for child nodes, we can extract the specific node types using the *nodeType* property.

Here, we need to get only, 
* Node.ELEMENT_NODE (1)

And we want to ignore,
* Node.TEXT_NODE (3)
* Node.COMMENT_NODE (8)

The following two examples will show you how to use *nodeType* to get the actual element child nodes.

#### 3.1. Find children using *childNodes* and *nodeType*


```javascript

function hasChildElements(parent) {
  for (let i = 0; parent?.childNodes[i]; i++) {
    if (parent?.childNodes[i]?.nodeType === 1) {
      console.log(parent.childNodes);
      return true;
    }
  }
  console.log("no children");
  return false;
}

// output
/*
"no children"
"So a new paragraph has been added"
*/

```

#### 3.2. Find children using *firstChild*, [*nextSibling*](https://developer.mozilla.org/en-US/docs/Web/API/Node/nextSibling) & *nodeType*

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

// output 
/*
"no children"
"So a new paragraph has been added"
*/

```

### 4. Capture only elements

Using some properties of the *Element* class, we can check for child elements as well.

[*Element*](https://developer.mozilla.org/en-US/docs/Web/API/Element) is the most general base class of all element objects.

For example it has following properties:
* children
* firstElementChild
* childElementCount

By using these properties of the *Element*, we can only obtain the element nodes without obtaining other types of nodes.


#### 4.1 Find children using *firstElementChild* 

The [*parent.firstElementChild*](https://developer.mozilla.org/en-US/docs/Web/API/Element/firstElementChild) gives first child element of the parent element. It only looks for element nodes to grab the first element child. Especially, it does not provide non-element nodes.

```javascript

function hasChildElements(parent) {
  if (parent?.firstElementChild) {
    console.log(parent?.firstElementChild);
    return true;
  }
  console.log("no children");
  return false;
}

// output
/*
"no children"
"So a new paragraph has been added"
*/

```

#### 4.2. Find children using *children.length*

The [*parent.children*](https://developer.mozilla.org/en-US/docs/Web/API/Element/children) includes only element nodes in a given element. 

Here we check for the number of child elements that given parent has, using the `length` property.

```javascript

function hasChildElements(parent) {
  if (parent?.children.length > 0) {
    console.log(`number of children: ${parent?.children.length}`);
    console.log(parent?.children);
    return true;
  }
  console.log("no children");
  return false;
}

// output 
/*
"no children"
"So a new paragraph has been added"
*/

```

#### 4.3. Find children using *childElementCount*

The [*parent.childElementCount*](https://developer.mozilla.org/en-US/docs/Web/API/Element/childElementCount) returns the number of child elements of the given element.

```javascript

function hasChildElements(parent) {
  if (parent?.childElementCount > 0) {
    console.log("childElementCount: " + parent?.childElementCount);
    return true;
  }
  console.log("no children");
  return false;
}

// output 
/*
"no children"
"So a new paragraph has been added"
*/

```

### Choose the best out of all

Now we know how to check for:
* any HTML source in an element.
* any types of child nodes in an element.
* specific node type of child nodes in an element.
* only child elements in an element.

However, more importantly, before using any line of code in your project, it is the best practice to check the browser compatibility of the code. 

Also, you may have something else to add here, so feel free to add them.  It will make this article more useful.

Happy Coding!

**_Image Credit: Tanja Cotoaga on Unsplash_**
  
