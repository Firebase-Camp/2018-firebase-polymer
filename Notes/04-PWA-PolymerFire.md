# Building a Progressive Web App with Polymer / Firebase

We'll do a rapid walkthrough of a second Polymer + Firebase [integration codelab](https://codelabs.developers.google.com/codelabs/polymer-firebase-pwa/index.html). 

_The codelab estimated 55 minutes. It took: _

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




