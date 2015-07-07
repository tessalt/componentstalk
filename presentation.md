# Components are the future of the web.
## It's going to be okay. 
![](https://s3.amazonaws.com/f.cl.ly/items/1V3Q3r0X3B2p2o1m0O0G/VbuO4SHCyIkkyLTh.jpg)

---

# A romanticized and slightly inaccurate history of web apps

^
- all interactivity came from forms and links
- forms and links were pretty good at doing formy, linky things
- we had nice clear APIs for configuring our forms and links
- and it was good

---


```html
<form action="submit.php" method="POST">
  <input name="name" type="text">
  <select name="favourite_color">
    <option value="00ffff">Red</option>
    <option value="ff00ff">Blue</option>
    <option value="ff0000">Yellow</option>
  </select>
  <input type="submit">
</form>
```

^
- adding interactivity to a web page was a fairly simple process: add in a form tag to synchronously collect data,
then send that data off to your server
- the forms were easy and obvious to configure
- easy to tell what's going on

---

# Then JavaScript grew up

^ heyyyy we can do cool stuff now
- ajax lets us communicate with our server async so we can have interfaces that are less obviously just forms and links

```html
<form>
  ...
  <button onclick="submitForm()">Submit</button>
</form>
```

---
![fit](https://s3.amazonaws.com/f.cl.ly/items/3n2g1k19401I1k2I0b3c/Untitled-1.jpg)

^ then we decided that inline event handlers along with inline styles were a bad idea, because markup is precious and we shouldn't contaminate it. 

---

# Strict separation of concerns!
- HTML is for **content**
- JavaScript is for **behaviour**
- CSS is for **presentation**

and so it was written

---

```html
<button id="formSubmitButton">Submit</button>

<script>
$("#formSubmitButton").click(function () {
  $.ajaxStuff();
  return false;
});
</script>
```

and we called it **Best Practices**

^ 
- we moved all that behaviour out of our HTML and into some jqueryish javascript

---

# Remember when **Our best practices were killing us**? 

![inline](http://image.slidesharecdn.com/bestpractices-110330135557-phpapp02/95/our-best-practices-are-killing-us-1-728.jpg?cb=1302790778)

^ 
- in the name of separating concerns, we thought we could use CSS unassisted by HTML to style our web pages
- we tried to maintain a "purity" of HTML semantics and suffered for it
- need to have knowledge of two codebases basically
- it was a mistake

---

#  ... We did it again. 

In the fast-changing world of JavaScript-heavy interactive web applications, these best practices haven't kept up

---

# Is HTML **really** for content? 

```html
<select id="countryList"></select>
<script>
$.get('countrylist.json', function(data) {
  $('#countryList').populateOptions(data);
});
</script> 
```

^ Is an empty `select` tag populated by an ajax request for a json file really "content"? 

---
```html
<input type="range" min="0" max="10" step="2" value="6">
````
![inline](http://upload.wikimedia.org/wikipedia/en/archive/f/f4/20100830193250!The_Scream.jpg)

^
Is this content? Looks kind of like logic and behaviour

---

# HTML isn't just for content, and it never was

^
- HTML comes with some really nice declarative APIs for rich content and user interfaces
- like `<select>`, `<input>`, and `<img>`

---

```html
<input type="range" min="0" max="10" step="2" value="6">

<picture>
  <source srcset="images/extralarge.jpg" media="(min-width: 1000px)">
  <source srcset="images/large.jpg" media="(min-width: 800px)">
  <img srcset="images/medium.jpg" alt="A giant stone face ">
</picture>

<select name="provinces">
  <option value="AB">Alberta</option>
  <option value="BC">British Columbia</option>
</select>
````

^ 
- these represent more than just content
- there's plenty of behaviour and logic here declared inline

___

# Time out: What's a declarative API, and why do I want one? 
![](https://unsplash.imgix.net/19/clock.JPG?q=75&fm=jpg&s=c424614ae83cef32017167031925460d)

---

# Declarative vs. Imperative programming

*Imperative* code instructs the compiler what steps to take to achieve the desired result

*Declarative* code specifies what the desired result is and lets the compiler figure out the implementation

^ 
- abstract away the explicit process and focus on what you want to happen, not how it's done
- or if the compiler can't figure it out, you write an API that lets you write declarative code

---

# Example: SQL is everybody's favourite declarative language

`SELECT * FROM students WHERE id = 5`

^
- There's a for loop underneath looping through each row in the table comparing ids, but we don't need to worry about that
- declarative programming lets us separate the process of stating the problem we're trying to solve with the process of solving it
- lets focus on our own application and not low-level problems that have already been solved

---

# HTML has declarative APIs

```html
<input type="range" min="0" max="10" step="2" value="6">
```

[jsbin](http://jsbin.com/yuzanoziro/1/edit?js,console,output)

^
- There's a lot of stuff going on in the background 
- event listeners
- data binding
- styling
- But as a developer using the input tag, *you don't need to know or care about any of that*. 
- declarative code is about hiding the implementation details where you just need an interface
- gives you a contract of inputs and outputs

---

# We didn't really learn from those built-in examples


```html
<a href="#salesModal" class="launchModal">Launch Modal</a>

<script>
$('.launchModal').on('click', function () {
  someLibrary.openModal({
    content: $($(this).attr('href'))
  });
});
</script>
```
^
- When we need some custom UI, we do some weird stuff:
- implementation details are partly hidden in the jQuery library
- but the actual "wtf is going on here" is split up between the HTML, the DOM querying, the event listener, and the library config

---

Like, *what is this even*: 

```html
<a href="#salesModal" class="launchModal">Launch Modal</a>
````

^
- All we can actually infer is that it's a link that may or may not do anything. We can make guesses based on the class, but that's just coincidence. 

---

What about this bit of JavaScript: 

```html
<script>
$(".launchModal").on("click", function() {});
</script>
```

So what does `$(".launchModal")` even do? 

^ 
- Go into that big DOM mess we've made, and find this element that we just added. -
- Go look for it, even though we already know where it is. Because we put it there.

---
# *We're writing JavaScript like we don't have control over our HTML*
![](https://ununsplash.imgix.net/44/MIbCzcvxQdahamZSNQ26_12082014-IMG_3526.jpg?q=75&fm=jpg&s=238b14e81cab674316e3d4a4205be060)

---
# We think our HTML should be dumb, but then we have to do all sorts of shit in JS to make up for it

^ 
- like DOM querying and event listening
- this makes our code very brittle and tightly-coupled
- no modularity
- relies on the dumb markup being dumb in a specific way
- hard to read and maintain
- implementation details all over the place
- unnecessarily imperative

___

# So what happens when we re-introduce some declarative attribtues to our HTML?

2009: Meet Angular.js

---

```xml
<a href="" ng-click="archive()">archive</a>

```
![inline](http://macrobits.pinetreecapital.com/wp-content/uploads/angry-mob.jpeg)

^ 
- from the first example on angularjs.org

___

# Angular's HTML made a lot of people very angry

```xml
<a href="" ng-click="archive()">archive</a>

```

> "I don't like how it breaks the separation between html/js/css"

> "I hate the way the HTML looks"

> "Isn't this a step backwards???"

-- some nerds on twitter

---

![right](http://vignette2.wikia.nocookie.net/simpsons/images/2/24/Abe_simpson.gif/revision/latest?cb=20130203191514)

```xml
<a href="" ng-click="archive()">archive</a>

```

# OMG separation of concerns

I have a lot of things to say about this

^ 
- like I said, the separation doesn't hold
- declarative attribtues on HTML5 tags (or "action" in a form tag)
- your content isn't in your HTML for web apps anyways (json etc)
- you need to glue your HTML and JS together *somewhere* and querying the dom sucks
- could just as easily say OMG YOU'RE POLLUTING MY JS WITH DOM
- we can separate our concerns more intelligently than this

---

```xml
<a href="" ng-click="archive()">archive</a>

```

# OMG inline event handlers

- it's ok

![right](http://i.imgur.com/3sxE5Vr.png)

^ 
- they've grown up: not globals any more
- it's easier to read and reason about
- it's a lot less code
- these ones are scoped
- they were not the biggest mistake we made back then

---

```xml
<a href="" ng-click="archive()">archive</a>

```

# But it looks bad and you should feel bad

![left inline](http://i97.photobucket.com/albums/l210/T_Man_2006/Zoidberg.png)

- no

^ 
- what actually is the point of this HTML "purity" myth?
- didn't we go through this with CSS?
- didn't we decide to be pragmatists instead of purists?

---

![fit](https://s3.amazonaws.com/f.cl.ly/items/3t251B3I0M3D1Y1E2B1Z/DA8FA0E6-A927-409D-A9FA-D12A0D786266.jpg)

---

# If we're ok with this then...

![inline](https://s3.amazonaws.com/f.cl.ly/items/1M0L2J1P2d0A391j3e3Z/click.jpg)

# ... let's slide down this slippery slope

^ 
- and even if you're not ok with this, you don't really have a choice...

---

# Make up your own declarative attributes

```html
<button onClick="openModoal()" modal-target="#salesModal">Open</button>
```

# Make up your own declarative elements

```html
<modal-trigger-button target="#salesModal">Open</modal-trigger-button>
```

^ 
What if you could make something as useful as `<select>` or `<input>`

---

# Welcome to the future it is called web components

---

# What are Web Components? 


- Custom elements
- Shadow DOM
- templates
- HTML imports

^
- Web Components (capital W, capital C) are a set of browser specs that let you teach HTML new tricks!
- Web Component specs let you create your own reusable, encapsulated UI elements with custom behaviours and styles. 
- Create your own tags! Expose a public API! GO MAD WITH POWER. 

---

# An example: tabs

```html
<div id="tabs" class="tabs-container">
  <ul class="tabs-nav">
    <li><a href="#tab1">Tab 1</a></li>
    <li><a href="#tab2">Tab 2</a></li>
  </ul>
  <div class="tabs-content">
    <div id="tab1">Tab 1 content</div>
    <div id="tab2">Tab 2 content</div>
  </div>
</div> 

<script>
$("#tabs").tabs();
</script>
```

^
- now copy and paste all that HTML wherever you want some tabs
- try having multiple copies on one page! (This is where the *encapsulation* offered by components is cool)
- none of that HTML is particularly meaningful
- wtf is .tabs

---

# A better way

```html
<demo-tabs>
  <demo-tab title="Tab 1">Tab 1 content</demo-tab>
  <demo-tab title="Tab 2">Tab 2 content</demo-tab>
</demo-tabs>
```
^
- meaningful markup 
- nice and declarative 
- re-usable
- composable
- scoped

---

```html
<template id="template">
  <p>I'm in Shadow DOM. My markup was stamped from a &lt;template&gt;.</p>
</template>

<script>
var proto = Object.create(HTMLElement.prototype, {
  createdCallback: {
    value: function() {
      var t = document.querySelector('#template');
      var clone = document.importNode(t.content, true);
      this.createShadowRoot().appendChild(clone);
    }
  }
});
document.registerElement('x-foo', {prototype: proto});
</script>

<x-foo></x-foo>
```

^ 
- hides implementation details

---

# Composability is a big thing

```html 

<fancy-toggle active={{drawerVisible}}>Open Drawer</fancy-toggle>

<animated-drawer shown={{drawerVisible}}>
  Things in a drawer
</animated-drawer>

```

^
- these two components could be from totally different sources
- what matters to you is the public API
- this is a lot better than gluing together jQuery libraries

___

# OKAY I WANT SOME WEB COMPONENTS GIVE ME SOME WEB COMPONENTS

---

...it's a little bit complicated right now. 

Browser support for these specs is... not great. YET. 

# BUMMER

There's some options though

---

![](https://s3.amazonaws.com/f.cl.ly/items/35450O373G3s1U3q0K0W/polymer.png)
#  Polymer
## [www.polymer-project.org/](https://www.polymer-project.org/)

^
- Polymer is a set of polyfills + some syntax sugar + some free custom elements for using Web Components RIGHT NOW
- it doesn't have all the browser support either but it's better than the raw APIs 
- but components aren't always enough

^ tom dale's talk https://www.youtube.com/watch?v=AK6xMvq4E5Q

---

```html
<polymer-element name="x-foo">
  <template id="template">
    <p>I'm in Shadow DOM. My markup was stamped from a &lt;template&gt;.</p>
  </template>
</polymer-element>

<x-foo></x-foo>
```

---

# The philosophy behind components has SPREAD to a framework near you

---

#  Angular
##  Directives
  

```html
<tabset>
  <tab ng-repeat="tab in tabs" heading="{{tab.title}}" active="tab.active">
    {{tab.content}}
  </tab> 
</tabset>

```

![inline](https://lh6.googleusercontent.com/-TlY7amsfzPs/T9ZgLXXK1cI/AAAAAAABK-c/Ki-inmeYNKk/w749-h794/AngularJS-Shield-large.png)

^
- let you create new HTML elements but don't leverage the native browser APIs 
- Implementation-wise, would look pretty much the same as Web Components example
- Defining a directive is a bit different than defining a web component, it's a *little* more involved
- but you get the same declarative API niceness and scope management and reusability 
- Angular 2.0 will support Web Components out of the box. Not sure what this will look like yet.

---

# Ember COMPONENTS

```js
{{demo-tabs}}
  {{demo-tab title="Tab 1"}}Tab 1 content{{/demo-tab}}
  {{demo-tab title="Tab 2"}}Tab 2 content{{/demo-tab}}
{{/demo-tabs}}
```

^
- try to adhere closely to the Web Components spec as possible
- they're hoping that once the native specs are widely available, you'll have an easy update path to migrate your components
- Ember components look different from the native ones because handlebars. Basically just swap the angle brackets for double curly braces mostly. 
- Ember 2.0 makes components much more important
- like, one of the central abstractions
- HTMLbars will make the syntax for components look the same as the native spec (Ember 1.11)

---

![left](http://facebook.github.io/react/img/logo_og.png)
![right](http://cdn.meme.am/instances/400x/56354301.jpg)
# React.js

^
- React is literally just components all the way down
- but they're a bit different than the components in W3C spec/Angular/Ember
- they're not aiming to replicate or be interoperable with Web Components
- they think they have a better solution
- Web Components are all about the DOM
- React is all about not the DOM
- *Virtual DOM* 

---

```JavaScript
React.createClass({
  render: function() {
    return (
      <Tabs>
        <Tabs.Panel title='Tab #1'>
          <h2>Content #1 here</h2>
        </Tabs.Panel>
        <Tabs.Panel title='Tab #2'>
          <h2>Content #2 here</h2>
        </Tabs.Panel>      
      </Tabs>
    );
  }
});
```

^ 
- the abstraction is similar though
- declarative API configurable through HTML(* not HTML in React's case but JSX which is similar)
- focus on reusable, composable elements

---

# The JavaScripts of the future
![left](https://s3.amazonaws.com/f.cl.ly/items/3w2w2W3h0K0v0Q3g2v10/tumblr_mzvktpA8Qf1sh4q2jo1_500.jpg)

- rather than going back and forth between dumb but specific markup and JS with a lot of DOM querying
- you'll be going back and forth between wiring together components and tweaking their implementations

---

# The limits of declarative programming

- A lot of this is big wins for productivity and developer happiness
- but if declarative programming was perfect we wouldn't still be writing C et al
- sometimes you need to tweak the implementations

^ 
- example: performance tuning in SQL
- most of the time it does the right thing
- but sometimes you need to tweak the implementation because you know better

---

# In Summary

## the "Declarative renaissance" of the web is coming whether you like it or not

- Angular is doing it
- Ember is doing it
- React is doing it
- The browsers themselves are doing it

---

# But you're going to like it

- declarative APIs
- Meaningful HTML
- encapsulation (styles too in the native spec)
- re-usability
- composability

---

# Choose your own adventure

- Angular directives
- Ember components
- React components
- Polymer
- Native (Coming Soon(ish))

---

# You're going to "pollute" your HTML and you're going to like it

---
# Links

- [Declarative vs imperative programming](http://latentflip.com/imperative-vs-declarative/)
- [Our best practices are killing us](http://www.slideshare.net/stubbornella/our-best-practices-are-killing-us)
- [The web's declarative, composable future](http://addyosmani.com/blog/the-webs-declarative-composable-future/)
- [Angular.js](https://angularjs.org/)
- [Angular 2.0](http://ng-learn.org/2014/03/AngularJS-2-Status-Preview/)
- [Ember.js](http://emberjs.com/)
- [Ember road to 2.0](https://github.com/emberjs/rfcs/pull/15)

---

- [React.js](http://facebook.github.io/react/)
- [Polymer](https://www.polymer-project.org/)
- [Web Components](http://webcomponents.org/)
