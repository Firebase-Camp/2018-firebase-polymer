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

Here are some fundamental concepts:

 1. _Register an Element_ > [Element Registration](https://www.polymer-project.org/2.0/docs/devguide/registering-elements) | [Lifecycle Callbacks](https://www.polymer-project.org/2.0/docs/devguide/registering-elements#lifecycle-callbacks)
 2. _Add shadow DOM_ > [DOM Templating](https://www.polymer-project.org/2.0/docs/devguide/dom-template)
 3. _Compose with shadow DOM_ > [Composition and Distribution](https://www.polymer-project.org/2.0/docs/devguide/shadow-dom#shadow-dom-and-composition)
 4. _Use data binding_ > [Data binding](https://www.polymer-project.org/2.0/docs/devguide/data-binding) 
 5. _Declare a property_ > [Declared Properties](https://www.polymer-project.org/2.0/docs/devguide/properties) 
 6. _Bind to a property_
 7. _Repeat templates with <dom-repeat>_ > [Template Repeater](https://www.polymer-project.org/2.0/docs/devguide/templates)
