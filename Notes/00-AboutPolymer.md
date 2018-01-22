# About Polymer

## Overview 

```
What is Polymer? 
```

Polymer is an open-source JavaScript library created and maintained by the Google Chrome team. 

It helps you create custom reusable HTML elements, and use them to build modern _Progressive Web Apps_ that are performant and maintainable by taking full advantage of modern platform features including:

 * [Web Components](https://developers.google.com/web/fundamentals/web-components/) = extend browser's component model
 * [Service Workers](https://developers.google.com/web/fundamentals/primers/service-workers/) = support offline-ready experiences
 * [HTTP/2](https://developers.google.com/web/fundamentals/performance/http2/) = efficient compression, request prioritization, server push

```
What does "UseThePlatform" mean?
```

The tagline reflects a mission to leverage the new/upcoming capabilities of modern web browser platforms to improve performance and user experience on modern web apps, while making codebases easy to maintain and evolve for developers.

```
What is the Polymer App Toolbox?
```

[Polymer App Toolbox](https://www.polymer-project.org/2.0/toolbox/) is a collection of components, tools and templates for building Progressive Web Apps with Polymer. Features include:

 * Component-based architecture using Polymer and web components.
 * Responsive design using the app layout components.
 * Modular routing using the <app-route> elements.
 * Localization with <app-localize-behavior>.
 * Turnkey support for local storage with app storage elements.
 * Offline caching as a progressive enhancement, using service workers.
 * Build tooling to support serving your app multiple ways: 
    + unbundled for delivery over HTTP/2 with server push,
    + bundled for delivery over HTTP/1.

Adopt these features individually, or use them together to build a full-featured Progressive Web App. 

```
What is the current Polymer version?
```

Polymer is currently at [stable release 2.0](https://www.polymer-project.org/2.0/docs/devguide/feature-overview) with a [3.0 preview](https://www.polymer-project.org/blog/2017-08-23-hands-on-30-preview) available for early exploration.

<br/>
## Getting Started

**INSTALL DEPENDENCIES**

```
$ sudo npm install -g bower
```

This gets warning: 
_Your project can stop working at any moment because its dependencies can change. Prevent this by migrating to Yarn:_ https://bower.io/blog/2017/how-to-migrate-away-from-bower

```
$ brew install yarn
```

Yarn replaces Bower for package management as of [Polymer 3.0](https://www.polymer-project.org/blog/2017-08-23-hands-on-30-preview), which is still in preview mode (and experimental). So for now, we install it - but continue to work with bower.Â


**INSTALL POLYMER CLI**

 ``` 
 $ npm install -g polymer-cli
 ```
 
Note that I encountered [this issue](https://github.com/Polymer/polymer-cli/issues/836) on my Mac, and used the ```sudo npm install -g polymer-cli --unsafe-perm``` solution proposed there, to get around the problem.


**SCAFFOLD BASIC APP**

```
$ mkdir app-one
$ cd app-one
$ polymer init
$ polymer serve --open
```

Note that "polymer init" will give you the option of scaffolding a custom element, a simple application (with one element), or a complex starter-kit enabled SPA (with routes). In this case, we went with option 3 (starter-kit).

Currently the "--open" flag launches browser to the wrong link with 404. This is a [known issue](https://github.com/Polymer/polyserve/issues/171) that is still open. For now, simply use "polymer serve" or click "Go home" to get to main app.

_Verify that you have a working built/deployed SPA app. Then we can move on to the quick tour of features, then look at the boilerplate code for this SPA._

<br/>
## The Quick Tour

Polymer makes it easy to create web components _declaratively_. You can create custom elements (e.g., ```<chicago-bears> 2018 Season </chicago-bears>```) and use them in web pages just like traditional markup.
e.g., 

```
<div class="nfl-schedule"> 
    <chicago-bears> 
        2018 
    </chicago-bears> 
</div>
```

Here are some fundamental concepts from [the quick tour](https://www.polymer-project.org/2.0/start/quick-tour):

 1. _Register an Element_ > [Element Registration](https://www.polymer-project.org/2.0/docs/devguide/registering-elements) | [Lifecycle Callbacks](https://www.polymer-project.org/2.0/docs/devguide/registering-elements#lifecycle-callbacks)
 2. _Add shadow DOM_ > [DOM Templating](https://www.polymer-project.org/2.0/docs/devguide/dom-template)
 3. _Compose with shadow DOM_ > [Composition and Distribution](https://www.polymer-project.org/2.0/docs/devguide/shadow-dom#shadow-dom-and-composition)
 4. _Use data binding_ > [Data binding](https://www.polymer-project.org/2.0/docs/devguide/data-binding) 
 5. _Declare a property_ > [Declared Properties](https://www.polymer-project.org/2.0/docs/devguide/properties) 
 6. _Bind to a property_
 7. _Repeat templates with <dom-repeat>_ > [Template Repeater](https://www.polymer-project.org/2.0/docs/devguide/templates)


### Register an Element

The _index.html_ imports _custom-element.html_ in the <head> element. Then uses it in the <body>.

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://polygit.org/components/webcomponentsjs/webcomponents-loader.js"></script>
    <link rel="import" href="custom-element.html">
  </head>
  <body>
    <custom-element></custom-element>
  </body>
</html>
```

This is what the _custom-element.html_ looks like.

```
<link rel="import"  href="https://polygit.org/components/polymer/polymer-element.html">

<script>
  // Define the class for a new element called custom-element
  class CustomElement extends Polymer.Element {
    static get is() { return "custom-element"; }
    constructor() {
        super();
        this.textContent = "I'm a custom-element.";
      }
  }
  // Register the new element with the browser
  customElements.define(CustomElement.is, CustomElement);
</script>
```

 1. Create an ES6 class _CustomElement_ that extends _Polymer.Element_.
 2. Define its element name by returning the relevant string from _is()_. The chosen custom element (tag) name **must start with an ASCII later, and must contain a '-' i.e., have at least two words, hyphenated together**
 3. Register this class with the browser using ```customElements.define(CustomElement.is, CustomElement)```.

Registering a custom element with the browser associates that custom element's _name_ (defined by CustomElement.is) with a _class_, thus making it possible to have _properties_ and _methods_ associated with that custom element.


### Handling Lifecycle Callbacks

The custom elements _specification_ defines callbacks (called _custom element reactions_) that provide hooks for you to run your code when specific lifecycle changes occur. These are:

 * _constructor_ : called when an element is created, or a previously-created element is 'defined'.
 * _connectedCallback_ : called when element is added to a document (DOM)
 * _disconnectedCallback_ : called when element is removed from a document
 * _attributeChangedCallback_ : called when element's attributes are changed, appended, removed or replaced.

In your implementation of these callbacks you **must** always call the equivalent callback on the superclass first (e.g., super() for constructor, super.connectedCallback() for that callback etc.) - this allows Polymer to hook into the custom element's lifecycle before you handle the event.

**Special Case: Constructor**
The custom code within constructor _cannot_ add (or examine) custom element's attributes and children. So, for the most part, code requiring access to these values must be run in the connectedCallback (after element is stamped and attached to a document).

**Special Case: ready() callback**
While not specified in the custom element specification (i.e., will not be available in core platform implementations of web components), the _Polymer_ library does provide one additional callback: **ready()**.

This is [called the FIRST time the element is added to the DOM](https://www.polymer-project.org/2.0/docs/devguide/custom-elements#polymer-element-initialization) - you can use this for one-time initialization work after element creation (i.e., where shadow DOM is attached and initial values can be given to data bindings) but beware of limitations to use.