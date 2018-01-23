# Building a Progressive Web App with Polymer / Firebase

We'll do a rapid walkthrough of a second Polymer + Firebase [integration codelab](https://codelabs.developers.google.com/codelabs/polymer-firebase-pwa/index.html). 

_The codelab estimated 55 minutes. It took 120 mins (if learning right) _

While the 00-BasicPolymerApp codelab used Firebase, it did so only for hosting. There was no integration with the Firebase SDK itself; all that was needed was the Firebase CLI to deploy the production build to the backend for hosting.

This time, we are looking to use Firebase Products (e.g., Auth, Database etc.) and will therefore look at how we setup Polymer apps to initialize the Firebase SDK, and then implement the code to access the relevant APIs.

## 1. Introduction


## 2. Install Global Dependencies

Check if dependencies are met:
```
$ bower -version
1.8.2
$ firebase --version
3.17.3
$ node --version
v8.9.4
$ npm --version
5.6.0
```


## 3. Initialize Firebase

Create new project on [Firebase Console](https://console.firebase.google.com/). I created _pwa-polymer-fire-app_.

Initialize front-end project for Firebase usage. Accept all default config and set it up to use Hosting and Database products.

```
firebase init
```

You should see something like this:

```
? Which Firebase CLI features do you want to setup for this folder? Press Space to select f
eatures, then Enter to confirm your choices. Database: Deploy Firebase Realtime Database Ru
les, Hosting: Configure and deploy Firebase Hosting sites

=== Project Setup

First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add, 
but for now we'll just set up a default project.

? Select a default Firebase project for this directory: pwa-polymer-fire-app (pwa-polymer-f
ire-app)

=== Database Setup

Firebase Realtime Database Rules allow you to define how your data should be
structured and when your data can be read from and written to.

? What file should be used for Database Rules? database.rules.json
✔  Database Rules for pwa-polymer-fire-app have been downloaded to database.rules.json.
Future modifications to database.rules.json will update Database Rules when you run
firebase deploy.

=== Hosting Setup

Your public directory is the folder (relative to your project directory) that
will contain Hosting assets to be uploaded with firebase deploy. If you
have a build process for your assets, use your build's output directory.

? What do you want to use as your public directory? public
? Configure as a single-page app (rewrite all urls to /index.html)? No
✔  Wrote public/404.html
✔  Wrote public/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

✔  Firebase initialization complete!
```

## 4. Install node-app sample code

We are using the _public_ directory as our build/ target. So let's install some dependencies there.

```
cd public
$ bower install -p polymerlabs/note-app-elements firebase/polymerfire
$ cd ../
```

This will install pre-existing Polymer components in the _bower-components_ directory there, including the [starter code: note-app-elements](https://github.com/googlearchive/note-app-elements). You can open the relevant directory (within public/bower_components/note-app-elements) to see the Polymer code.

# 5. Initialize Project Folder

For a PWA we need a _manifest.json_, a _service worker_ (sw-import.js), and a _custom element_ (note-app.html). We'll create empty placeholders for these.

```
$ touch public/manifest.json 
$ touch public/sw-import.js
$ touch public/note-app.html
```

# 6. Start local server to view "production" build

```
firebase serve

=== Serving from '/Users/.../pwa-polymer-fire'...

i  hosting: Serving hosting files from: public
✔  hosting: Local server: http://localhost:5000
```

Open browser to that URL and verify the "Firebase Hosting" default page is shown.

# 7. Configure Web App Manifest

[Here's an example](https://github.com/googlearchive/note-app/blob/master/common/manifest.json) of a standard Web App Manifest, as required in Progressive Web Apps.

You can craft a [Web App Manifest](https://www.w3.org/TR/appmanifest/) file by hand, using the [spec](https://www.w3.org/TR/appmanifest/) as a guide. Or you can use the [Web App Manifest Generator](https://app-manifest.firebaseapp.com/).

Get the basic configuration and icons generated. I configured manifest using the generator above, but repurposed the icons provided by the note-app example (as recommended in codelab)

# 8. Fill out sw-import.js

We'll use the service worker component (```<platinum-sw>```) later, but the first step is to have a file that acts as the entry point for service workers.
Within that file, simply import the service worker script from the component (uses the [importScripts](https://developer.mozilla.org/en-US/docs/Web/API/WorkerGlobalScope/importScripts) method for loading a script into a web worker's scope)

```
importScripts('bower_components/platinum-sw/service-worker.js');
```

# 9. Customize index.html

Replace the index.html boilerplate with this.

```
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, minimum-scale=1.0, initial-scale=1, user-scalable=yes">
    <title>Notes</title>
    <style>
      body {
        margin: 0;
      }
    </style>
  </head>
  <body>
  </body>
</html>
```

Change the title, then tell the browser about our Manifest.

```
<link rel="manifest" href="manifest.json">
```

Add the following to support older Chrome configs:

```
<!-- Older Chrome configuration -->
<meta name="mobile-web-app-capable" content="yes">
<meta name="application-name" content="Notes">
<link rel="icon"
      sizes="192x192"
      href="bower_components/note-app/common/images/icon-4x.png">
```

# 10. Declare app dependencies

# 11. Configure Service Workers

The advantage of using Web Components to configure our service worker is that the entire configuration can be done declaratively. Using simple HTML tags and attributes, we can provide to an underlying JavaScript implementation everything it needs to know to configure and launch our service worker.

# 12. Firebase Project Attributes



# 20. Basic <note-app> behavior

Polymer supports sharing common implementation (code) across custom elements using a feature called [behaviors](https://www.polymer-project.org/2.0/docs/devguide/behaviors). The sample code for this codelab ships with a basic implementation of the <note-app> JavaScript as Polymer.NoteAppBehavior.

# 21. Binding things together

Whether data flow goes down from host to target, up from target to host, or both ways is controlled by the type of binding annotation and the configuration of the target property.

 * Double-curly brackets ({{ }}) support both upward and downward data flow.
 * Double square brackets ([[ ]]) are one-way, and support only downward data flow.

For more on data flow, see [How data flow is controlled](https://www.polymer-project.org/2.0/docs/devguide/data-system#data-flow-control).


# 22. Implement the signin listener

Use ```this.$.auth``` to reference the element with id="auth" in the template associated with this custom element. Read about [Static Node Maps](https://www.polymer-project.org/2.0/docs/devguide/dom-template)

# 24. Prepare for Firebase data

Polymer.NoteAppBehavior is a generalized JavaScript implementation for our <note-app> element created in advance for expediency. It does not have special knowledge of Firebase or how our Firebase database is structured. We need to customize some implementation to describe the shape of our Firebase database for our Firebase-backed <note-app>.

# 26. Query and Persist Firebase Data

Here ```<firebase-query>``` generates an ordered list of data that corresponds to some path through our Firebase database. 

We want to query for all of the notes made by the authenticated user, so we set the path accordingly, leveraging the user session object produced by <firebase-auth>

_Persist data for offline access._

It's great to have access to our notes when we are online, but it can be especially useful to review notes in moments when we do not have any internet access.  Yes: there is a ready-to-go custom element that will solve this problem for us. The element is called ```<app-indexeddb-mirror>```. It is purpose-built to make it easy to cache live data (data queried from a Firebase database, for example) in a local IndexedDB database that will be made available to the user when she uses the app offline.

We provide the current unique user ID as the session attribute of the ```<app-indexeddb-mirror>```. This causes the locally stored IndexedDB database to be erased when the current user logs out. Erasing the data, in turn, ensures that other users cannot spy on the local database to get unauthorized access to data.
