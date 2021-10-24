# Binary-Calculator-Post
Pending Post...

# BUILDING A BASIC BINARY CALCULATOR ONLY WITH BASIC HTML, CSS, JAVASCRIPT.

I know some of you just completed learning javascript, or just started learning it, or maybe expecting to learn it. When it comes to me, no matter what level I am in the learning process when I get the project ideas to work with. 

I know it is really hard to build something with basics, since, as always, smarter and shorter ways can be learned at the end of the learning process.

For now, you can create this project only with the basics.

The project that I built was a simple **Binary Calculator**. I got the inspiration for the project idea while I was learning [JavaScript DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction "Introduction to the DOM - MDN"). 

The calculator performs basic mathematical operations such as addition, subtraction, also some bitwise operations such as AND, OR, XOR. It takes input in both binary and decimal formats and gives results in both types too.


### DESIGN THE INTERFACE OF THE CALCULATOR

For the most part, I enjoyed designing the interface of the calculator. I have just used basic HTML and CSS to design it without any specific library or framework. And I didn't use any online assets such as images, fonts to keep it simple and to make sure it works well offline too.

I used CSS flexbox instead of CSS grid to organize the buttons in the interface. Because, in my opinion, flexbox is more flexible to use than CSS grid.

The interface has following specific HTML elements:
* `<select>` : to select what the type of the input value is decimal or binary.
* `<input>` (accept only numbers) : to get input values
* `<button>` : 
  1. mathematical operators
  1. bitwise operators
  1. reset button 
  1. equal operator that generates the result. 
* `<table>` : to includes the results in both binary and decimal format.
* `<label>` : to show results and warning messeges.


### GIVE IT A LIFE WITH JAVASCRIPT 

Let's get into my most favorite part, JavaScript. 

I had to study some concepts along with JavaScript to add functionality to the calculator before move on to the actual implementation. Such as, 
* How to convert binary into decimal in JavaScript.
* How to convert decimal into binary in JavaScript.
* Working with bitwise operators such as AND, OR, XOR, etc.

In the calculator, I had to implement few click events with the `button` elements. There are three types of buttons that do different things.

1. reset the page : window.location.reload()
1. button to generate the result : finalResult()
1. operator buttons : getOperator(this.id)


There was no trouble with the reset button since there is an in-built method called `window.location.reload()`. The method will reload the page when the button gets clicked. 

The next button generates the final result and the function `finalResult()` is the main function of the whole process. All the other functions are getting called inside this function.

Then started the *tough part* of the *click event story*. I used only one function for all the operators to keep it simple. As a result, I had to figure out what operator button was clicked by the user, each time the function was getting called. 

In simple words, look at the following JavaScript code snippets. Both operator buttons have the same function, if you click one button of them, the function will be getting called, but the problem is how do I get to know what button was getting pressed.
 
```html
<button id="plus" class="btn operator-btn" onclick="getOperator(this.id)">+</button> <!--plus-->

 <button id="subtract" class="btn operator-btn" onclick="getOperator(this.id)">-</button> <!--subtract-->
```

That was the time `this.id` method comes into the scene. To be honest, I got this idea after doing some research on *StackOverflow*, and love to mention always there is a problem similar to mine that was asked by someone else with a set of incredible answers.

The method [`this.id`](https://developer.mozilla.org/en-US/docs/Web/API/Element/id "Element.id - MDN") returns the [`id`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id "id - MDN") of the element which referred by [`this`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this "this - MDN") keyword, and I used it as a parameter of the `getOperator(this.id)` function. So then it was easy to recognize that what operation has to be done, for example, addition or subtract.

Also, have to say, it was not a good decision that I made, to use `onclick` method in HTML, instead of using `addEventListener()` in JavaScript to listen to the click events.


### FOUND SOMETHING NEW TO LEARN

It was all about laziness that always put me into complexity. 

After dealing with the above problem, I got the `id` of the operator which is a string simply describes the operation type. Also, I created functions for each operation with a name similar to its related `id`. And I wanted to try something like the below code.

```javascript
let operator = "subtract"; //the id in the string format

// the function that has the same name as the string 

function subtract(value1, value2) {
  return value1 - value2;
}

// expected way to call the function 
/*  
let result = operator(7, 5); 
console.log(result); 
*/
//gives error  
// Uncaught TypeError: operator is not a function 

```

After doing some research I realized, it is possible to do by using `window` object. Since the *operator* variable is just a property of the [`window`](https://developer.mozilla.org/en-US/docs/Web/API/Window "Window - MDN") object, we can access its value using the property key in dynamic way, in this case, it is *operator*.

```javascript
let res = window[operator](7, 5);
console.log(result);
```

However, I went with the simple way by matching string with hard-coded string sake of simplicity. 

Although I did not apply the solution to the project, I must say that I learned something new and realized that there was much more to learn. 


### THINGS YOU CAN'T DO WITH THE CALCULATOR?

Now I'll tell you about the operations that you can't do with this calculator. 
* calculate negative values.
* calculate floating points.
* NOT (~), LEFT SHIFT (<<) and RIGHT SHIFT (>>) operations.

You can try with division operation that gives output as floating points, but it won't generate result in binary but just the same output in decimal. I think it was the biggest drawback of this project. 

Also, I never considered the bit representation (such as 32 bit or 64 bit) since I was not expecting to implement left shift and right shift operator, at this moment.


### CONCLUSION

Now is your turn to check my project [Binary Calculator](https://codepen.io/MkDay/pen/zYdOGjm "Binary Calculator Project") and don't forget to tell me what do you think about that? Because I always love to improve myself with your opinion.
