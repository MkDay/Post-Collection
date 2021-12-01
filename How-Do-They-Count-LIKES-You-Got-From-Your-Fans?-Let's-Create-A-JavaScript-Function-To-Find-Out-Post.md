# How Do They Count LIKES You Got From Your Fans? Let's Create A JavaScript Function To Find Out.

How many times a day do you check your *Likes Count* somewhere on the internet? It's countless, right?
 
So what if you have your own *LIKE* button that allows you to manipulate your *likes count*?

You can create your own *LIKE* button if you know the basic HTML and CSS. 

Also, you can manipulate your *likes count* if you know the following basic JavaScript concepts.

* data types
* array methods
* JavaScript DOM 
* switch-case
* object
* regular expressions
* arrow function
* spread operator
* ternary operator


### 1. Create the HTML 

```html
<h2>Let's Manipulate Your Likes-Count</h2>

<h3>Follower List</h3>

<div id="follower-list">

  <input type="checkbox" id="friend" value="My Best Friend">
  <label for="friend">My Best Friend</label> <br>

  <input type="checkbox" id="pet" value="My Pet">
  <label for="pet">My Pet</label> <br>

  <input type="checkbox" id="neighbour" value="My Neighbour">
  <label for="neighbour">My Neighbour</label> <br>

  <input type="checkbox" id="teacher" value="My Teacher">
  <label for="teacher">My Teacher</label> <br>

  <input type="checkbox" id="parents" value="My Parents">
  <label for="parent">My Parents</label> <br>

  <input type="checkbox" id="boss" value="My Boss">
  <label for="boss">My Boss</label> <br>

  <input type="checkbox" id="alien" value="An Alien From The Mars">
  <label for="alien">An Alien From The Mars</label> <br>
</div>

<h3>Let's Check Who Likes This...?</h3>

<h4>Please Click The Like Button If You Like This...</h4>

<button id="like-btn">Like</button>

<p id="show-likes"></p>

```
Add your own flavour to it using CSS, it's all up to you.

### 2. Give it a life with JavaScript

```javascript

let likeBtn = document.querySelector("#like-btn");
let showLikes = document.querySelector("#show-likes");
let followerList = document
  .querySelector("#follower-list")
  .querySelectorAll("input[type=checkbox]");
let likes = [];

followerList.forEach((follower) => {
  follower.addEventListener("change", function (e) {
    likes = Array.from(followerList)
      .filter((e) => e.checked)
      .map((e) => e.value);
  });
});

likeBtn.addEventListener("click", (e) => {
  showLikes.innerHTML = countLikes(likes);
  console.log(countLikes(likes));
});

function countLikes(likes) {
  switch (likes.length) {
    case 1:
      return `${likes[0]} likes this.`;
    case 2:
      return `${likes[0]} and ${likes[1]} like this.`;
    case 3:
      return `${likes[0]}, ${likes[1]} and ${likes[2]} like this.`;
    default:
      return likes.length > 3
        ? `${likes[0]}, ${likes[1]} and ${likes.length - 2} others like this.`
        : `No one, but only I like this.`;
  }
}

```
 
### 3. Other different ways to count likes

Now you are done! you can play with it as much as you want.

But wait!

Here, we used `switch-case` statement to count likes. However, there are thousands ways to create your `function countLikes() {}`. 

So let's try some of cool ways to count your likes.

#### Method 1: 
#### Concepts That Used: Object, arrow function and ternary operator

```javascript

const countLikes = (likes) => {
  let format = {
    1: `${likes[0]}`,
    2: `${likes[0]} and ${likes[1]}`,
    3: `${likes[0]}, ${likes[1]} and ${likes[2]}`,
    4: `${likes[0]}, ${likes[1]} and ${likes.length - 2} others`
  };

  let who =
    likes.length > 0 ? format[Math.min(likes.length, 4)] : `No one, but only I`;
  let likeThis = likes.length === 1 ? ` likes this.` : ` like this.`;

  return who + likeThis;
};

```

#### Method 2:
#### Concepts That Used: regular expressions, String.replace() and Array.shift() 

```javascript

function countLikes(likes) {
  let format = [
    "No one, but only I like this.",
    "{name} likes this.",
    "{name} and {name} like this.",
    "{name}, {name} and {name} like this.",
    "{name}, {name} and {n} others like this."
  ];

  let index = Math.min(likes.length, 4);
  return format[index].replace(/{name}|{n}/g, function (val) {
    return val === "{name}" ? likes.shift() : likes.length;
  });
}

```

#### Method 3:
#### Concepts That Used: ternary operator and Array.shift()

```javascript

function countLikes(likes) {
  let len = likes.length;

  if (len <= 1) {
    return len === 0
      ? "No one, but I like this."
      : likes.shift() + " likes this.";
  } else {
    return (
      (len < 4
        ? (len < 3 ? "" : `${likes.shift()}, `) +
          `${likes.shift()} and ${likes.shift()}`
        : `${likes.shift()}, ${likes.shift()} and ${likes.length} others`) +
      " like this."
    );
  }
}

```

#### Method 4:
#### Concepts That Used: spread operator and undefined

```javascript

function countLikes([first, second, ...others]) {
  let whoLikesThis;
  if (others.length <= 0) {
    if (second === undefined) {
      if (first === undefined) {
        whoLikesThis = `No one, but only I like this.`;
      } else {
        whoLikesThis = `${first} likes this.`;
      }
    } else {
      whoLikesThis = `${first} and ${second} like this.`;
    }
  } else {
    if (others.length === 1) {
      whoLikesThis = `${first}, ${second} and ${others[0]} like this.`;
    } else
      whoLikesThis = `${first}, ${second} and ${others.length} others like this.`;
  }
  return whoLikesThis;
}

```

Now tell us how many likes that you got (or wish to get) from your favourite ones.

{% codepen https://codepen.io/MkDay/pen/yLzLPBr %}

**_Note:_** 
This post inspired from this codewars problem [*Who likes it?*](https://www.codewars.com/kata/5266876b8f4bf2da9b000362/javascript). And I'd like to thank all the warriors who solved this problem amazingly. All these solution ideas that shown here are inspired from you!

**_Happy Coding!_**

**_Image Credit:_**
