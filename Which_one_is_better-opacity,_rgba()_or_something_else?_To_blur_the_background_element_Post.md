# Which one is better - opacity, rgba(), or something else? To blur the background-element

Maybe you had a lot of hard times like I had, trying to blur the background element without affecting its inner contents such as child elements, text, borders, etc.

In my case, as the first thought, I turned to the `opacity` to do the job.

And the next moment, I got a weird result and thought about another option.

Which was `rgba()` and, it worked for me.

But, it's not the problem. If the alpha value of `rgba()` can do the job, why do we need the `opacity` in any way? 

It seems there are some questions to be solved, right?

So let's dig into the details.

We have a blue color inner element and orange color background element like below.
$ screenshot: without blur $

And, we want the background element to be blurred, like below.

$ screenshot: with blur $



To get that output, we can try the following options.

1. Using two separate elements 
2. Using *opacity* for the parent element
3. Using *rgba()* for the parent element
4. Changing *visibility* to *hidden* or *color* to *transparent* (to completely hide the background element)

Let's try these options one by one to achieve our requirements.

### 1. Using two separate elements

Here, we have two separate `div` called `outside-div` and `inside-div`

```html

<body>

 <div id="outside-div"></div>
 <div id="inside-div"></div>

</body>

```

In the CSS, before doing anything, let's center everything on the screen in the first place. 

Here, we use *flexbox* to do the job.

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

```
And, to drag both `div` a bit down from the top of the screen, we can use `position: absolute` and `top: 25%` for both elements. 

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

Next, we have to center the `inside-div` inside the `outside-div`.

To do that, we can set the `margin-top` value like below.

**_`margin-top` for the innermost div = ((height of outermost div) - (height of innermost div))/ 2_**

```css

#inside-div {
 margin-top: 100px;
}

```

Now, set the `opacity` as `0.3` of the `outside-div`.

```css

#outside-div { 
 opacity: 0.3; 
}

```

And, here is our complete CSS code.

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

Oh, it works!

Finally, we're finished with solving the problem!

But hold on, why are we only using two separate elements? 

Is there any luck with parent and child elements, or maybe, an element with its pseudo (`:before` or `:after`) element instead, where we should have to use them?

Well, the answer is *YES*!

However, some of them can be a bit tricky. 

So let's take a look at how to blur a background element that has a child element/pseudo-element as its inner element.

### 2. Using *opacity* for the parent element


One of the ways to blur an element is using the `opacity` property. 

In simple words, the `opacity` sets the see-through (transparent) level of an element. If its value is,

* **_1_** :- which means there's no transparency at all (default value). 

* **_0_** :- which means it is fully transparent.

* **_0.5_** :- which means see-through level is 50%.

So let's try to use `opacity`. 

Here is our HTML code.

```html

<body>

<div id="box"></div>

</body>

```

#### Using *opacity* - 1st Attempt: 

So, we have a `div` element called `#box` and a pseudo-element (`#box:before`) in it.

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


As the first thing, let us place them neatly.

Also, make the `position` as `absolute` of the `#box` and push it down by `25%` away from the screen. 

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




As the final thing, set the `opacity` of the `#box` as `0.3`.

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

Here is the visual result that we've got.

$ screenshot: not working opacity $

Okay, I know what you are thinking right now. Because it isn't the result that we expected, right? 

We only want to make the `opacity` of the `#box` as 0.3 and `opacity` of the `#box:before` as 1 (default opacity).


#### Using *opacity* - 2nd Attempt: 

To fix the problem, let's follow these steps.

* **Step 1:** Swap `width` and `height` values between `#box` and `#box:before`


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

* **Step 2:** Change the `top` property from `top: 25%` to `top: calc(25% + 100px)` of the `#box` 

By doing so, `#box:before` can be placed exactly 25% down from the screen top.

```css

/* step 2 */

#box {
 position: absolute;
 top: calc(25% + 100px);
}

```

* **Step 3:** Set *z-index*

  1. for the `#box`: `z-index: 2` 
  2. for the `#box:before`: `z-indx: 1`  

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

* **Step 4:** Set `opacity: 0.3` to the `#box:before`

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

$ screenshot: working opacity $


#### 3. Using *rgba()* for the parent element

Let's try another way to get the expected result. And, it is the most simple one. 

All you need to do is, remove the `opacity: 0.3` from `#box` and change its `background-color` as `rgba()` colors and set your opacity as the fourth value there.  

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

$ screenshot: working rgba() $


#### 4. Changing visibility to hidden or color to transparent (to completely hide the background element) 

If your requirement is only to make the background `div` as **_invisible_**, it's better to make the child element/pseudo-element as *visible* and the background (parent) element as *invisible*. 

You can use this code for it.

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

Now, let us turn into the ideal question *which one is better?*

By looking at methods that used `opacity` and `rgba()` that we discussed above, you might be wondering which one is better to use for your specific case. 

Well, as always, it depends.

However, let me explain some of the differences between both of them.

* The value of `opacity` applies to the whole element, including its child elements, text, borders, colors, etc.

* In the meantime, the *alpha* value of the `rgba()` only applies to the color of that particular element. 

So that's the reason you got the `#box:before` as blurred at our 1st attempt where we used `opacity` to blur only the background element.

Also, for the same reason, you may feel some visual differences between the two results where we got the expected result using `opacity` and `rgba()`.

In my opinion, it is better to use `rgba()` if you only want to blur the background element.

### Conclusion

As the last thing, let me recap what you exactly got from this article. 


* a few ways to blur the background element without affecting its inner contents. 

* some sense of the difference between `opacity` and `rgba()` 

* some sense of where to use `opacity` and where to use `rgba()` based on your specific requirement.

I hope you enjoyed this article, and you can support me at [ko-fi](https://ko-fi.com/mkdaycode). I always appreciate your support. It really encourages me to keep going. 

**_Happy Coding!_** 

[![image_description](https://cdn.ko-fi.com/cdn/kofi2.png?v=3)](https://ko-fi.com/mkdaycode) 
