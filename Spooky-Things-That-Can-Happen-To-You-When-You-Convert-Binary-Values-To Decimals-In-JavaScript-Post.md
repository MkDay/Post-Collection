# Spooky-Things-That-Can-Happen-To-You-When-You-Convert-Binary-Values-To Decimals-In-JavaScript-Post.

Pending Post...

# Spooky Things That Can Happen To You When You Convert Binary Values To Decimals In JavaScript


Last week I spent a lot of time working on binaries. What exactly I was trying to do is, take the binary value from the user and convert them to decimals. It was pretty straightforward to do with the following code.


```javascript

 //convert binary to decimal

function convertBinary(binary) {
 let inDecimal = 0;
 let power = 0;
 for(let i=binary.length-1; i>=0; i--) {
   if(binary[i]==1) {
   inDecimal += Math.pow(2, power);   
   }
   power++;
 }
 return inDecimal;
}

console.log(convertBinary('101')); // returns 5

```


As you can see, there is a couple of variables, loops, in-built methods used. So now it's time to search for even shorter and more simple ways to do it.



### IS IT POSSIBLE WITH ONE LINE OF CODE?

Of cause yes! 
We can use a couple of ways to do that.


#### Method 1: ES6 Binary Literals 

With ES6, we can achieve this with [binary literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Numbers_and_dates#numbers "Binary Numbers").


```javascript

console.log(0B101); // returns 5

// or
console.log(0b101); // returns 5

```


#### Method 2: Number() Object

[`Number(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number#description "Number") Object can be used as a function that accepts a value in string format or another type as an argument and converts it to the `Number` type.

Since `Number` can be expressed in literal forms like `0b101`, `0o13`, `0x0A`, it can accept values in literal forms and convert them to decimals. 


```javascript

Number('0b101'); // returns 5

// or 
Number(0b101); // returns 5

```



#### Method 3: parseInt() method

[`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt "parseInt()") accepts two arguments, `parseInt(string, radix)`. First argument is the value to parse and the second argument is the radix of that value.



```javascript

console.log(parseInt('101', 2)); // returns 5

```


**NOTE:** If you know more ways, please comment below. It will make this article more useful.



### NOW IS THE SPOOKIEST PART

Now here is how you can get the spooky results when converting binary values.

#### 1. parseInt() doesn't accept values in literal forms.

If we try to convert binary literals like below, we end up with the following output. Because it just grabs all the values that can be converted to number-type until it finds something that cannot be converted. 

In this case, it only grabs *0*, and it just takes *b* as a string-type character. So that's why it just returns only zero.


```javascript

parseInt('0b101', 2); // returns 0

```


#### 2. parseInt() doesn't work with numeric separator

We use [numeric separator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#numeric_separators) to separate long numbers to make them easy to read.

But if we use them with `parseInt()`, it will give us a wrong result. It just grabs the first characters until it finds the underscore and converts them to `number` type. 


```javascript

parseInt('11_00', 2); // returns 3 expected 12

```


#### 3. parseInt() returns **NaN** with incompatible radix


```javascript

parseInt('321', 10); // returns 321

parseInt('321', 2)); // returns NaN

```


These are the terrible results I got from working with binary. If you get more, please don't forget to let us know how scary they are.
