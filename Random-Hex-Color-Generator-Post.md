# Random-Hex-Color-Generator-Post
Posted @codenewbie : 20/09/2021


# Random Hex Color Generator 

Choosing matching colors when creating a web page is crucial as same as choosing clothes to wear. With the help of **Random Hex Color Generator**, it can be done in a little easier way. It generates hex color code as an actual color and also in text format randomly. 

If you have basic HTML, CSS, and JavaScript knowledge, you can create this project within a few minutes. For a JavaScript beginner, it is a good project where to practice JavaScript [`Math.random()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) function. On the other hand, you can use this tool in real projects to speed up the color choosing process.

For simplicity, I developed this project idea in two versions. 

1. First version: was developed as a stand-alone tool to generate random hex colors.
1. Second version: was developed to use as an embedded code snippet to generate matching colors randomly for an element of another web project.

The stand-alone version of the project has `<div>` elements to display generated color and its hex code in text format. Also a `<button>` to trigger the process.

In the second version, before adding the JavaScript code, apply the following changes to any HTML element which wants a `background-color` in your web project. Make sure your element is not empty or it has some assigned `width` and `height`, otherwise it won't be visible. 

```html
<element id="yourElement" onclick="randomHexColors('yourElement')"></element>
```
Note: `<element>` can be any HTML element such as `<div>`. And make sure there is no `background-color` added in CSS for the element. 

Both versions use JavaScript code in the same way. The only difference is, `randomHexColors()` function was made as parameterized in the second version.

1. Stand-alone version: `randomHexColors()` 
1. Embedded version: `randomHexColors(element)`
 
There are two functions in JavaScript,
* one for generating a hex code randomly: `hexCode()`
* one for displaying the generated hex color as the actual color and also in text format: `randomHexColors()`

### Complete Code For The Stand-alone Version
#### HTML 
![HTML Code](https://community.codenewbie.org/remoteimages/uploads/articles/n618gtwzpbe5w834vxjk.png)

#### CSS
![CSS Code](https://community.codenewbie.org/remoteimages/uploads/articles/ia74vokuca7azuw796iu.png)

#### JavaScript
![JavaScript Code: 1st-version](https://community.codenewbie.org/remoteimages/uploads/articles/ke0hktwaauy0asgm2zn3.png)

You can see the live preview of the stand-alone version at CodePen [here](https://codepen.io/pen/?template=qBjjbmy "Random Hex Color Generator").

### JavaScript Code Snippets For The Second Version
![JavaScript Code: 2nd-version](https://community.codenewbie.org/remoteimages/uploads/articles/1a0hrjrabx1s1kkudhmg.png)

Note that, when you are using the second version in your project, don't forget to remove all the stuff from your code after choosing your matching color.

As I mentioned before, I've got these two versions of the project idea built within half an hour and I tested them for a couple of my projects. 

While testing the second version, I realized that it is more effective when choosing matching colors for separate but close elements at the same time.

Although this is only for choosing background colors, I hope it can be improved to choose `text-color` and `border-color` at the same time.

Furthermore, if you are a beginner and want to build this project, first go with the stand-alone tool and then work with the second one. Because, in my opinion, building the second version of the project was a bit trickier than the first one.
 
Also, I hope this will be a very useful project for any web developer against its simplicity. 
