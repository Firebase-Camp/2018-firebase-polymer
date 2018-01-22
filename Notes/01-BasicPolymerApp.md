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
