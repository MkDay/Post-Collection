# LET'S TRY TO CALL A FUNCTION BY A STRING IN JAVASCRIPT

If you know the basics of JavaScript, then you know how to call a [Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions "Functions-MDN"). However, what if we want to call a function differently?


### FUNCTION NAME AS A STRING

Here is the real-world use case that I have tried.

There is a set of functions and, there is also a set of string variables with values ​​corresponding to each function name.

The user also can select the corresponding string variable that has a value equal to the name of the function he intends to execute at this moment.

```javascript

// set of functions

 function add(num1, num2) {
 console.log(num1 + num2);
}

function subtract(num1, num2) {
 console.log(num1 - num2);
}

function divide(num1, num2) {
 console.log(num1 / num2);
}

function multiply(num1, num2) {
 console.log(num1 * num2);
}

// set of string variables

let operationAdd = 'add';
let operationSubtract = 'subtract';
let operationDivide = 'divide';
let operationMultiply = 'multiply';

// user input

let currentOperation = operationAdd;

```

Now if we call the `function add() {}` using the variable `currentOperation` which currently has the value `add`, it will give us an error.

```javascript

currentOperation(8, 2); // throws an error

```

It seems that we cannot replace a function name directly with a string. However, I found some great ways to do it.

### WAYS TO REPLACE A FUNCTION NAME WITH A STRING

#### 1. Using eval()

The [`eval()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval "eval() - MDN") function evaluates JavaScript code represents as a string. That string can be a JavaScript expression, a statement, or a sequence of statements. The expression can include variables and properties of existing objects.

In the code below, we concatenate the string value of the function name with the arguments in parentheses as a single string and pass it to the function `eval()` as its argument.

```javascript

function add(num1, num2) {
  console.log(num1 + num2);
}
 
let currentOperation = 'add';

let functionString = currentOperation + '(8, 2)';

eval(functionString); // returns 10

```

However, the `eval()` function is not a good solution for that. Why because it has significant drawbacks. More importantly, it is insecure and slows down the code running. You can learn more about why you should not use `eval()` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#never_use_eval! "Never use eval() - MDN").

#### 2. Using Function Object

We can use the [`Function`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function "Function Object - MDN") object as an alternative to the `eval()` function. Here we create an instance of the `Function` and pass our function in string format as its argument as we did before with `eval()`.

```javascript

function add(num1, num2) {
 console.log(num1 + num2);
}

let currentOperation = 'add';

let functionString = currentOperation + '(8, 2)';

let newFunction = new Function(functionString);

// function call
newFunction(); // returns 10

```

#### 3. Using Function.name

The [`name`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/name "Function.name - MDN") property of the `Function` object returns name of the function as a string.

Using the `.name` property, we can compare a function name as a string value with another string. Then we can call the function as usual.

```javascript

function add(num1, num2) {
 console.log(num1 + num2);
}

let currentOperation = 'add';

if(currentOperation === add.name) {
 add(8, 2); // returns 10
}

```
#### 4. Using window Object

Since all the items, such as variables and functions in JavaScript, are properties (or methods) of the `window` object, we can call a function as a method of the `window` object.

```javascript

function add(num1, num2) {
 console.log(num1 + num2);
}

let currentOperation = 'add';

window[currentOperation](8, 2); // returns 10

```

This method works well with global functions. However, it may not work for some cases such as *namespaced-functions*. 

See the example below.

```javascript
let operations = {
  add: function(num1, num2) {
    console.log(num1 + num2);
  },
  
  subtract: function(num1, num2) {
    console.log(num1 - num2);
  }
};

let currentOperation = 'add';

// The following code will not work
// window[operations.currentOperation](8, 2); // throws an error

```

It works as follows.

```javascript

operations[currentOperation](8, 2); // returns 10

```

#### 5. Using a custom function (Recommended) 

Also, we can create a custom function to call a function using its name as a string. It is more flexible and accurate than the others.

```javascript

function add(num1, num2) {
 console.log(num1 + num2);
}

let currentOperation = 'add';

function executeFunctionByName(functionName, context /*, arg*/) { 
let args = Array.prototype.slice.call(arguments, 2); 

let namespaces = functionName.split("."); 

let func = namespaces.pop(); 

  for(let i = 0; i < namespaces.length; i++) { 

    context = context[namespaces[i]]; 
  } 

return context[func].apply(context, args); 

}

executeFunctionByName(currentOperation, window, 8, 2); // returns 10

```

### THE BEST WAY AND THE WORST WAY

In my opinion, it depends on your requirement when choosing the best way to do the job. However, it is a good practice not to use `eval()` for this purpose. Considering all the methods discussed above, I think the last one is the most efficient way to do it.

If you have tried these methods or any other methods, please let us know your experience with them. Because always we love to learn from each other.
