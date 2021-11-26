# Weird Looking For-Loops In JavaScript

I bet you all use [*for-loop*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) like the below one, at least once in your lifetime.

```javascript

for (let i = 1; i <= 5; i++) {
  console.log(i);
}

/* output

1
2
3
4
5

*/

```
The `for` loop comes in handy when we want to,

* execute some statements multiple times,
* by updating the value of a variable, 
* while evaluating a condition.

Here is the syntax for the `for` loop.

### Syntax

> *for ([initialization]; [condition]; [final-expression]) {*
> 
>   *//statements*
>   
> *}*

Normally what does this do is, 

1. **initialization:** Initialize a variable and evaluate only once before the loop start.

2. **condition:** Check the condition.
   * if it is true, execute the *statements*.
   * if it is false, terminate the loop.

3. **final-expression:** If the condition is true, evaluate the *final-expression*.

4. go back again to check the condition.

Anyway, instead of this deadly usual, let me show you some of the rare things that come with *for-loop*.

### Optional three expressions

By looking at the syntax of the `for` loop, we can see that it has three expressions inside the parentheses. But they all are optional, which means we can leave them as blank as well. 

So let's try to leave all or some of these expressions and see the effect.


#### 1. The *for-loop* without *initialization* expression

Here we keep the *initialization* expression as empty.

Alternatively, we can initialize the variable outside of the loop before the loop begin. But remember to put a *semi-colon* to represent the empty initialization block.

```javascript

let i = 1;

for (; i <= 5; i++) {
  console.log(i);
}

/* output

1
2
3
4
5

*/

```

#### 2. The *for-loop* without *condition* expression

Also, we can omit the *condition* part as well. 

By doing this, you have to break the loop at some point. Otherwise, it will run infinitely.


```javascript

for (let i = 1; ; i++) {
  if (i > 5) {
    break;
  }
  console.log(i);
}

/* output

1
2
3
4
5

*/

```

#### 3. The *for-loop* without *final-expression* expression 
This loop omits the *final expression*. So we have to modify the variable inside the loop body to keep the loop running.


```javascript

for (let i = 1; i <= 5; ) {
  console.log(i);
  i++;
}

/* output

1
2
3
4
5

*/

```



#### 4. The *for-loop* without any expressions

Even we omit all the expressions still have to put two *semi-colons* inside the parentheses to represent all the three expression blocks. Otherwise, it gives us an error. 

Also, do not forget to use the *break-statement*, to terminate the loop at some point and modify the variable to keep the loop running.

```javascript

let i = 1;

for (;;) {
  if (i > 5) {
    break;
  }

  console.log(i);
  i++;
}

/* output

1
2
3
4
5

*/

```

#### 5. The *for-loop* with multiple variables

Of cause! Multiple variables are allowed to use inside the parentheses. Using *commas*, we can separate them from each other in each expression block.

In the example below, we use two separate variables called `i` and `j`. 

* *i* represents *odd* numbers between 1 to 5 inclusively.
* *j* represents *even* numbers between 1 to 5 inclusively.


```javascript

for (let i = 1, j = 2; i <= 5, j <= 5; i += 2, j += 2) {
  console.log(i + " - odd");
  console.log(j + " - even");
}

/* output

"1 - odd"
"2 - even"
"3 - odd"
"4 - even"

*/

```

The cool thing is you can see that we did not get `5 - odd` in the output! 

The reason is that the *for-loop* checks both the conditions in each iteration and only executes the statements if both of them are true. 

After the fourth iteration, 
* i = 5, so i <= 5 is true
* j = 6, so j <= 5 is false

So the loop stop at this point.


#### 6. The *for-loop* without the loop body

Interestingly enough, we can omit the *loop body* as well. Here we put *semi-colon* immediately after the parentheses instead of the loop body.

In this example, `i` increments until *10* and in each iteration adds its value to the `sum`.

```javascript

let sum = 0;

for (let i = 1; i <= 10; i++, sum += i);

console.log(sum); // 65

```

### The *for-loop* with the keywords *var* and *let* 

A variable initialized with the `var` keyword inside the loop, can also access outside the loop.

The reason is that the variable initialized with the `var` and the *for-loop* both belong to the same scope.

If we initialize the variable with the `let` keyword, we cannot access it outside the loop.

Because the scope of the variable initialized with `let` is local to the loop.

Try this example.

**with *var* keyword:**

```javascript

// with var keyword
for (var i = 1; i <= 5; i++) {
  console.log(`inside counter = ${i}`);
}

console.log(`outside counter = ${i}`); // doesn't throw errors

/* output

"inside counter = 1"
"inside counter = 2"
"inside counter = 3"
"inside counter = 4"
"inside counter = 5"
"outside counter = 6"

*/

```

**with *let* keyword:**

```javascript

// with let keyword
for (let i = 1; i <= 5; i++) {
  console.log(`inside counter = ${i}`);
}

console.log(`outside counter = ${i}`); // throws an error

/* output

"inside counter = 1"
"inside counter = 2"
"inside counter = 3"
"inside counter = 4"
"inside counter = 5
"Uncaught ReferenceError: i is not defined 

*/

```

### Labeled *for-loop*

Using [label](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label#description), we can [break](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break#description) the *outer loop* within the *inner loop*. Because the `break` statement can be labeled.

Here is the syntax of the `break` statement:

> *break [label];*


```javascript

outer_loop: for (let i = 1; i <= 3; i++) {
  console.log("round: " + i);
  inner_loop: for (let j = 1; j <= 5; j++) {
    if (i > 1) {
      console.log("do not allow to run more than one round");
      break outer_loop;
    }

    console.log(j);
  }
}

/* output

"round: 1"
1
2
3
4
5
"round: 2"
"do not allow to run more than one round"

*/

```

### The *for-loop* iterates through the HTML element

Here is another rare way to use *for-loop*. 

This loop iterates through the parent node by checking whether it has any child element using the [*nodeType*](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType) of the child node. 

`child.nodeType == 1` means it checks for only the *ELEMENT_NODE*.

If it found a child element then, it gets `textContent` of the child.

**HTML:**

```html

<div id="parent">
  <p>child para 1</p>
  <p>child para 2</p>
  <p>child para 3</p>
  <p>child para 4</p>
  <p>child para 5</p>
</div>

<button id="btn">Get Text</button>

```

**JavaScript:**

```javascript

// loop through child nodes in a parent node and get the text content of them.

let parent = document.querySelector("#parent");
let btn = document.querySelector("#btn");
let counter = 1;

btn.addEventListener("click", getText);

function getText() {
  let child;

  for (child = parent.firstChild; child; child = child.nextSibling) {
    if (child.nodeType == 1) {
      console.log(child.textContent);
    }
  }
}

/* output

"child para 1"
"child para 2"
"child para 3"
"child para 4"
"child para 5"

*/

```

These are only a few of the rare cases that come with *for-loop*. If you know more, we would like to hear from you. 

**_Happy Coding!_**
