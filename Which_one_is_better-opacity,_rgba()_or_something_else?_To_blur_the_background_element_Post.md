
# Which one is better - opacity, rgba() or something else? To blur the background element

Maybe you had a lot of hard times like I had, trying to blur the background element without affecting its internal elements such as child elements, text, etc.

As the first thought, you may think it's not a big deal and, you may suggest to use `opacity` for it. 

And the next moment, you might be wondering, okay if the `opacity` does the job then what does the *alpha* value of the `rgba()` do?

It seems there are some questions to be solved, right?

So let's dig into the details.

Here is the output that we expected to get without getting unexpected result. 


$$$$ screenshot working one $$$$



To get that output, we can try the following options.

1. Using two separate elements 
2. Using *opacity* 
3. Using *rgba()*
4. Changing *visibility* to *hidden* or *color* to *transparent* (to completely hide the background element)

Let's try these options one by one to achieve our requirement.

### 1. Using two separate elements

Here, we have two separate `div` called `outside-div` and `inside-div`.

```html

<body>

 <div id="outside-div"></div>
 <div id="inside-div"></div>

</body>

```

In the CSS, before doing anything let's center everything on the screen at the first place. 

Using *flex* with the `body`, we can center everything on the screen. 

```css

body {
 display: flex;
 justify-content: center;
 align-items: center;
}

```

Then, give some `width`, `height` and `background-color` to the `outside-div` and `inside-div`.

```css

#outside-div {
 width: 400px;
 height: 400px;
 background: rgb(255, 165, 0);
}


#inside-div {
 width: 200px;
 height: 200px;
 background: rgb(0, 0, 255);
}

And, to drag the both `div` bit down from the top of the screen, we can use `position: absolute` and `top: 25%` for the both elements. 

```css

#outside-div {
 position: absolute;
 top: 25%;
}


#inside-div {
 position: absolute;
 top: 25%;
}

```

Next, we have to center the `inside-div` inside the `outside-div`. To do that we can set the `margin-top` value like below.

**_`margin-top` for the innermost div = ((height of outermost div) - (height of innermost div))/ 2_**

```css

#inside-div {
 margin-top: 100px;
}

```

Now we can easily make the `opacity: 0.3` of the `outside-div`.

```css

#outside-div { 
 opacity: 0.3; 
}

```

And, here is our full CSS code.

```css

* {
 box-sizing: border-box;
 margin: 0;
 padding: 0;
}

body {
 display: flex;
 justify-content: center;
 align-items: center;
}

#outside-div {
 position: absolute;
 top: 25%;
 width: 400px;
 height: 400px;
 background: rgb(255, 165, 0);
 opacity: 0.3; 
}


#inside-div {
 position: absolute;
 width: 200px;
 height: 200px;
 top: 25%;
 margin-top: 100px;
 background: rgb(0, 0, 255);
}

```

Congrats! you made it! 

Finally we're finished with solving the problem!

But hold on, why are we only using two separeate elements? 

Is there any luck with parent and child elements or maybe an element with its pseudo (`:before` or `:after`) element instead, where we should have to use them?

Well, the answer is *YES*!

However, some of them can be bit tricky. 

So let's take a look how to blur an background element that has child/pseudo element as its inner element.

### 2. Using opacity


One of ways to blur an element is using `opacity` property. 

In the simple words, the `opacity` sets the see-through (transparent) level of an element. If its value is,

* 1: which means there's no transparency at all (default value). 

* 0: which means it is fully transparent.

* 0.5: which means see-through level is 50%.

So let's try to use `opacity`. 

This is our HTML code.

```html

<body>

<div id="box"></div>

</body>

```

#### Using *opacity* - 1st Attempt: 

In the CSS we have a `div` called `#box` and, we have a pseudo element (`#box:before`) of it. 

```css

#box { 
 width: 400px;
 height: 400px;
 background: rgb(255, 165, 0);
}

#box:before {
 content: "";
 width: 200px;
 height: 200px;
 background: rgb(0, 0, 255);
}

```


As the first thing let us place them neatly.

To do so, `#box:before` make as centered using *flex* inside the `#box`. Also, make the `position` as `absolute` of the `#box` and, push it down by `25%` away from the screen. 

```css

body {
 display: flex;
 justify-content: center;
 align-items: center;
}

#box {
 position: absolute;
 top: 25%;
 display: flex;
 justify-content: center;
 align-items: center; 
}

#box:before {
 content: "";
 position: absolute;
}

```




As the final thing we can set the `opacity: 0.3` to the `#box`.

```css

#box {
 opacity: 0.3; 
}

``` 

Here is the complete CSS code.

```css

body {
 display: flex;
 justify-content: center;
 align-items: center;
}

#box {
 position: absolute;
 top: 25%;
 width: 400px;
 height: 400px;
 display: flex;
 justify-content: center;
 align-items: center; 
 background: rgb(255, 165, 0);
 opacity: 0.3; 
}

#box:before {
 content: "";
 position: absolute;
 width: 200px;
 height: 200px;
 background: rgb(0, 0, 255);
}

``` 

This is the visual result that we've got.

$$$$ screenshot of the burly blue box. $$$$$

Okay, I know what are you thinking right now. Because it isn't the result that we expected, right?

What we just want is make the `opacity` of the `#box` as 0.3 and `opacity` of the `#box:before` as 1 (default opacity).


#### Using *opacity* - 2nd Attempt: 

To acheive that, let's follow these steps.

* **step 1:** Swap `width` and `height` values between `#box` and `#box:before`


```css

 /* step 1 */
 
#box {
 width: 200px;
 height: 200px;
}

#box:before {
 content: "";
 width: 400px;
 height: 400px;
}

``` 

* **step 2:** Change the `top` property from `top: 25%` to `top: calc(25% + 100px)` of the `#box` 

By doing so, `#box:before` can be placed exactly 25% down from the screen top.

```css

/* step 2 */

#box {
 position: absolute;
 top: calc(25% + 100px);
}

```

* **step 3:** Set *z-index*

* for the `#box`: `z-index: 2` 
* for the `#box:before`: `z-indx: 1`  

By doing so, `#box:before` will be pushed behind the `#box`.

```css

/* step 3 */

#box {
 z-index: 2; 
}

#box:before {
 z-index: 1;
}

```

* **step 4:** Set `opacity: 0.3` to the `#box:before`

```css

/* step 4 */

#box:before {
 opacity: 0.3;
}

```

Complete CSS code is like below.

```css

#box {
 position: absolute;
 top: calc(25% + 100px);
 width: 200px;
 height: 200px;
  
 display: flex;
 justify-content: center;
 align-items: center; 
 background: rgb(0, 0, 255);

 z-index: 2; 
}

#box:before {
 content: "";
 position: absolute;

 width: 400px;
 height: 400px;
 
 background: rgb(255, 165, 0);

 opacity: 0.3; 
 
 z-index: 1;
}

```

Now you'll get the following result as you expected.

$$$$ screenshot no opacity blue box $$$$$


#### 3. Using *rgba()*

This is so simple, all you need to do is just remove the `opacity: 0.3` from `#box` and change its `background-color` as `rgba()` color and set your opacity as the fourth value there.  

```css

#box {
 position: absolute;
 top: 25%;
 width: 400px;
 height: 400px;
 display: flex;
 justify-content: center;
 align-items: center; 
 
 /* change rgba() color like this */
 
 background: rgba(255, 165, 0, 0.3);
}

#box:before {
 content: "";
 position: absolute;
 width: 200px;
 height: 200px;
 background: rgb(0, 0, 255);
}

```


#### 4. Changing visibility to hidden or color to transparent (to completely hide the background element) 

If your requirement is only make the background `div` as **_invisible_**, better to mention, just make the child/pseudo element as visible and background (parent) element invisible. 

You can simply use this code for it.

```css

#box {
 visibility: hidden;
 position: absolute;
 top: 25%;
 width: 400px;
 height: 400px;
 display: flex;
 justify-content: center;
 align-items: center; 
 background: rgb(255, 165, 0);
}

#box:before {
 visibility: visible;
 content: "";
 position: absolute;
 width: 200px;
 height: 200px;
 background: rgb(0, 0, 255);
}

```

Or you can use `color: transparent` for the background (parent) element.

```css

#box {
 background: transparent;
}

#box:before {
 background: rgb(0, 0, 255);
}

```


### Which one is better - *opacity* or *rgba()*?

Now let us turn in to the ideal question, *which one is better?*

By looking at methods that used `opacity` and `rgba()` that we discussed above you may wondering which one is better to use for your specific case. 

Well, as always, it depends.

However, let me explain some of differences between both of them.

* The value of `opacity` applies to the whole element, including its child elements, text, borders, colors, etc.

* In the meantime, *alpha* value (which used to set the opacity) of the `rgba()` only applies only to the color of that particular element.

So that's the reason you got the `#box:before` as blured at our 1st attempt where we used `opacity` to blur only the background element.

Also, since the same reason, you may feel some visual differences between two results where we got the expected result using `opacity` and `rgba()`.

In my opinion, it is better to use `rgba()` if you just want to blur the background element only.

### Conclusion

As the last thing, let me give you a quick recap what you exactly got from this artcle. 


* a few ways to blur the background element without affecting its internal elements. 

* some sense of the difference between `opacity` and `rgba()` 

* some sense of where to use `opacity` and where to use `rgba()` based on your specific requirement.

If you enjoyed this post find me at [ko-fi](https://ko-fi.com/mkdaycode). I always appreciate your support, it really encourages me to keep going. 

**_Happy Coding!_** 

[![image_description](https://cdn.ko-fi.com/cdn/kofi2.png?v=3)](https://ko-fi.com/mkdaycode) 
