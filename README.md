# aurelia-firebase

 [![Circle CI](https://circleci.com/gh/PulsarBlow/aurelia-firebase/tree/master.svg?style=svg)](https://circleci.com/gh/PulsarBlow/aurelia-firebase/tree/master)   [![Join the chat at https://gitter.im/aurelia/discuss](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/aurelia/discuss?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

A Firebase plugin for [Aurelia](http://aurelia.io/) that supports Authentication, Reactive data collections (auto-sync) and other Firebase features. This plugin has been developed from scratch in order to provide an ubiquitous aurelia's experience and a [Promise/A+](https://promisesaplus.com/) support.

This is an early version which comes with :

- A complete firebase password authentication support.
- A reactive collection providing auto-sync with Firebase server.

Roadmap for the next versions :
 
* Full Query support (order, startAt, endAt etc..)
* Priorities
* Transactions
* Third party authentications (Google, FB etc..)

###Demo

[Aurelia on Fire](https://github.com/PulsarBlow/aureliaonfire) is a demo app providing complete working implementation of the plugin features.

Play with the demo : https://aureliaonfire.azurewebsites.net  
Check the demo source : https://github.com/PulsarBlow/aureliaonfire

# Installation


#### Install via JSPM
Go into your project and verify it's already `npm install`'ed and `jspm install`'ed. Now execute following command to install the plugin via JSPM:

```
jspm install aurelia-firebase=github:pulsarblow/aurelia-firebase
```

this will add the plugin into your `jspm_packages` folder as well as an mapping-line into your `config.js` as:

```
"aurelia-firebase": "github:aurelia-firebase@X.X.X",
```

If you're feeling experimental or cannot wait for the next release, you could also install the latest version by executing:
```
jspm install aurelia-firebase=github:pulsarblow/aurelia-firebase@master
```


#### Migrate from aurelia-app to aurelia-app="main"
You'll need to register the plugin when your aurelia app is bootstrapping. If you have an aurelia app because you cloned a sample, there's a good chance that the app is bootstrapping based on default conventions. In that case, open your **index.html** file and look at the *body* tag.
``` html
<body aurelia-app>
```
Change the *aurelia-app* attribute to *aurelia-app="main"*.
``` html
<body aurelia-app="main">
```
The aurelia framework will now bootstrap the application by looking for your **main.js** file and executing the exported *configure* method. Go ahead and add a new **main.js** file with these contents:

``` javascript
export function configure(aurelia) {
  aurelia.use
    .standardConfiguration()
    .developmentLogging();

  aurelia.start().then(a => a.setRoot('app', document.body));
}
```

#### Load the plugin
During bootstrapping phase, you can now include the validation plugin:

``` javascript
export function configure(aurelia) {
  aurelia.use
    .standardConfiguration()
    .developmentLogging()
    .plugin('aurelia-firebase'); //Add this line to load the plugin

  aurelia.start().then(a => a.setRoot('app', document.body));
}
```

#Configuration
##One config to rule them all
The firebase plugin has one global configuration instance, which is passed to an optional callback function when you first install the plugin:
``` javascript
export function configure(aurelia) {
  aurelia.use
    .standardConfiguration()
    .developmentLogging()
    .plugin('aurelia-firebase', (config) => { config.setFirebaseUrl('https://myapp.firebaseio.com/'); });

  aurelia.start().then(a => a.setRoot('app', document.body));
}
```

>Note: if you want to access the global configuration instance at a later point in time, you can inject it:
``` javascript
import {Configuration} from 'aurelia-firebase';
import {inject} from 'aurelia-framework';

@inject(Configuration)
export class MyVM{
  _config = null;
  constructor(config)
  {
    this._config = config;
  }
}
```

##Possible configuration
>Note: all these can be chained:
``` javascript
(config) => { 
  config
    .setFirebaseUrl('https://myapp.firebaseio.com/')
    .setMonitorAuthChange(true);
}
```

###config.setFirebaseUrl(firebaseUrl: string)
``` javascript
(config) => {
  config.setFirebaseUrl('https://myapp.firebaseio.com/');
}
```
> Sets the Firebase URL where your app lives.
> This is required and the plugin will not start if not provided.

###config.setMonitorAuthChange(monitorAuthChange: boolean)
``` javascript
(config) => { config.setMonitorAuthChange(true); }
```
When set to true, the authentication manager will monitor authentication changes for the current user 
The default value is false.

#AuthenticationManager

The authentication manager handles authentication aspects in the plugin.
 
#ReactiveCollection

The ReactiveCollection class handles firebase data synchronization.  

# Sample implementation
Check the sample implementation at : https://github.com/PulsarBlow/aureliaonfire