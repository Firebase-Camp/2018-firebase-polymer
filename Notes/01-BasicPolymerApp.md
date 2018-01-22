# Building a Basic Polymer 2.0 App


## 1. Introduction 

This is a walkthrough of the [_Build and Deploy a Polymer 2.0 App From Scratch_](https://codelabs.developers.google.com/codelabs/whose-flag/index.html) Codelab

> What you build

A simple flag-guessing game. Present a flag to the user. Give them two choices for an answer. They pick one and you tell them if they are right or wrong. Plus, a button that let's them go another round.

> What you learn

 * Setup development environment for Polymer
 * Generate app from a Polymer (starter) template
 * Add pre-built Web Components to your app
 * Style app with CSS and custom properties
 * Use Polymer elements & features to 
    - load files
    - manipulate data
    - respond to user interaction

> What you need
 
 * Command-line terminal (MacOS check!)
 * Simple text editor (Sublime3 check!)
 * Latest version of Chrome (optimal WC support)

> The target audience

Targeted at absolute beginners to web components. However basic web development knowledge is expected (HTML, CSS, JS). And basic object-oriented programming knowledge is expected (classes, encapsulation, etc). 

## 2. Getting Setup

Verify you have Node/NPM installed, else [install from site](https://nodejs.org/en/).

```
$ node -v
v8.9.4
$ npm -v
5.6.0
```

Install Bower and Polymer CLI. Verify installs were successful by checking versions.

```
$ npm install -g bower
$ bower -v
1.8.2

$ npm install -g polymer-cli
$ polymer --version
1.5.7
```

**Note:**
I encountered [this issue](https://github.com/Polymer/polymer-cli/issues/836) on my Mac, and used the ```sudo npm install -g polymer-cli --unsafe-perm``` solution proposed there, to get around the problem.

Also _bower_ is [being replaced by _yarn_ as the package manager in Polymer 3.0.](https://www.polymer-project.org/blog/2017-08-23-hands-on-30-preview) (currently in early preview) so expect future codelabs/process to change.


## 3. Create a Polymer App from template

First create a new directory, then run commands within it.

```
$ mkdir whose-flag
$ cd whose-flag/
$ polymer init

? Which starter template would you like to use? (Use arrow keys)
❯ polymer-2-element - A simple Polymer 2.0 element template 
  polymer-2-application - A simple Polymer 2.0 application 
  polymer-2-starter-kit - A Polymer 2.x starter application template, with navigation and "PRPL pattern
" loading 
  shop - The "Shop" Progressive Web App demo 
```

Select _polymer-2-application_ as the template, provide some description ("flag-guessing game") and confirm. 

```
? Which starter template would you like to use? polymer-2-application
info:    Running template polymer-2-application...
? Application name whose-flag
? Main element name whose-flag-app
? Brief description of the application Flag-Guessing Game
   create bower.json
   create index.html
   create manifest.json
   create polymer.json
   create README.md
   create src/whose-flag-app/whose-flag-app.html
   create test/whose-flag-app/whose-flag-app_test.html

Project generated!
Installing dependencies...
...
..

Setup Complete!
Check out your new project README for information about what to do next.
```

The resulting directory will look as follows:

```
README.md          - default documentation for app
bower_components/  - package dependencies    
manifest.json      - app metadata, useful for PWA
src/               - app's HTML/CSS/JS code
bower.json         - app's package dependency config
index.html         - app's entry point
polymer.json       - app's project structure, build config
test/              - app's automated tests

```

Currently, the app is defined in a single file at _src/whose-flag-app.html_. The code looks as follows:

```
<link rel="import" 
 href="../../bower_components/polymer/polymer-element.html">

<dom-module id="whose-flag-app">

  <template>
    <style>
      :host {
        display: block;
      }
    </style>
    <h2>Hello [[prop1]]!</h2>
  </template>

  <script>

    /**
     * @customElement
     * @polymer
     */
    class WhoseFlagApp extends Polymer.Element {
      static get is() { return 'whose-flag-app'; }
      static get properties() {
        return {
          prop1: {
            type: String,
            value: 'whose-flag-app'
          }
        };
      }
    }

    window.customElements.define(WhoseFlagApp.is, WhoseFlagApp);
  </script>

</dom-module>

```

Let's break it down.

 1. The [**dom-module**](https://www.polymer-project.org/2.0/docs/api/elements/Polymer.DomModule) is a custom element itself. 
    - It provides a registry of _relocatable_DOM content by "id" that is agnostic to bundling. 
    - By registering the enclosed dom against the specified id, it makes that accessible later via its static _import_ API, regardless of where the content/file was relocated during the bundling. 
 
 2. The [**template**](https://www.polymer-project.org/2.0/docs/devguide/dom-template) element provides a way for you to create a DOM subtree for your element. 
    - The **template** block is where we define the visual elements (user interface) of the page, ideally by composing other elements. _(HTML)_
    - The **style** block defines "look and feel" of the interface. _(CSS)_

 3. The **script** block contains code that defines your app & provides its functionality or behavior (e.g., handling interactions). _(JS)_.
    - prop1 is a [**property**](https://www.polymer-project.org/2.0/docs/devguide/properties) that is declared in the class definition for the associated custom element (_WhoseFlagApp_). 
    - Properties provide state for the element, enable 2-way [data binding](https://www.polymer-project.org/2.0/docs/devguide/data-binding) (using double-backet ```[[propname]] ``` syntax) and can [mapped/reflected](https://www.polymer-project.org/2.0/docs/devguide/properties#property-name-mapping) to element attributes.

## 4. Serve the app

You can test your app locally using ```polymer serve ``` and manually opening the browser to the specified URL. Alternatively use ```polymer serve --open``` to auto-launch browser to the app page.

Note: this does auto-rebuild the app as you make changes to source. However it does _not_ seem to do hot reloads; rather, you need to manually refresh the browser to see the updated app.

## 5. Create the User Interface with existing Web Components

The advantage of web components is that you can not only _create_ custom elements, but you can _import and reuse_ custom elements created by others. [WebComponents.org](https://www.webcomponents.org/) is a central repository where both third-party-developed and Polymer-team-developed custom elements can be found (with demos and documentation).

Process to install/use these components:
 * ```cd``` to the root folder of your app
 * ```bower install <name> --save``` where name is replaced by the appropirate custom-element name e.g., ```bower install app-layout --save``` will install the "app-layout" component and save that dependencys to the bower.json file.
 * ```<link rel="import" href="../../bower_components/app-layout/app-layout.html"> ``` in the _whose-flag-app.html_ file to "import" that custom element's definition into the current app.
 * Now go ahead and _use_ that custom element in the HTML file -- e.g., you can now say ```<app-header> .. </app-header>``` in the template section, to effectively include the _app-header_ dom subtree into that of the _whose-flag-app_ custom element.

Note: Some web components are not about visual HTML elements (structure) but about custom style that is reusable - e.g., see [paper-styles](https://www.webcomponents.org/element/PolymerElements/paper-styles) which enforces Material Design guidelines - e.g., it defines [custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables) for Material Design colors, making it easier to use them consistently in other custom elements that "import" this paper-style element.

## 6. Style your app

For our element, we first installed paper-styles

```
bower install paper-styles --save
```

then added it to our imports as before. We then changed the default element style in our custom element as follows:

```
    <style>
      :host {
        display: block;
        font-family: Roboto, Noto, sans-serif;
      }
      paper-button {
        color: white;
      }
      paper-button.another {
        background: var(--paper-blue-500);
        width: 100%;
      }
      paper-button.another:hover {
        background: var(--paper-light-blue-500);
      }
      paper-button.answer {
        background: var(--paper-purple-500);
        flex-grow: 1;
      }
      paper-button.answer:hover {
        background: var(--paper-pink-500);
      }
      app-toolbar {
        background-color: var(--paper-blue-500);
        color: white;
        margin: 20px 0;
      }
      iron-image {
        border: solid;
        width: 100%;
        --iron-image-width: 100%;
         background-color: white;
      }
      #flag-image-container {
        max-width: 600px;
        width: 100%;
        margin: 0 auto;
      }
      #answer-button-container {
        display: flex; /* or inline-flex */
        flex-flow: row wrap;
        justify-content:space-around;
      }
    </style>
```

The ```<style>``` within the element template is _scoped locally_ - meaning those styles are enforced only on that element. The rest of the page does not know about it - this is the encapsulation that shadow DOM enables.

However, the ```:host``` selector allows you to pierce this constraint and enforce styles at the _parent_ element, enabling a consistent look & feel app-wide.





