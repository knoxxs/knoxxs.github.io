---
layout: post
title:  "Modern web app architecture - Ember"
date:   2015-09-30 02:08:00
categories: programming web
tags: ember mvc mozilla web-app architecture
---

Currently while planning for scupids my team decided to use some new backend & frontend technologies/frameworks for our next idea. For the frontend we decided to use `Ember`. So to get started with ember Vipul gave me [this](https://developer.mozilla.org/en-US/Apps/Build/Modern_web_app_architecture) tutorial to follow and here are my notes for the same.

## Introduction
There are many different ways to build an open web application, including a lot of different tool choices, and a lot of new theory to learn. If you're just starting to build complex modern web applications and have tried to research what tools to use, it's easy to become frustrated by the sheer number of choices available. The tools used in this tutorial are part of the [Mozilla Recommended Toolchain](https://developer.mozilla.org/en-US/Apps/Quickstart#Mozilla_recommended_tool_chain). The main tool for this tutorial is `Ember.js/Ember CLI`. `Ember.js` is a well-known framework for rapid development of MVC applications, and `Ember CLI` is a command line interface for generating Ember code.

## MVC architecture

In general, it separates out the application logic into three separate parts, promoting modularity and ease of collaboration and reuse. It also makes applications more flexible and welcoming to iterations.

**Example**: Let's imagine a simple shopping list app. All we want is a list of the name, quantity and price of each item we need to buy this week.

![Basic MVC architecture]({{ site.url }}assets/2015/09/modern-web-app-architecture/basic-mvc.png)

### The Model
The model defines what data the app should contain. If the state of this data changes, then the model will usually notify the view (so the display can change as needed) and sometimes the controller (if different logic is needed to control the updated view).

Going back to our shopping list app, the model would specify what data the list items should contain — item, price, etc. — and what list items are already present.

### The View    
The view defines how the app's data should be displayed.

In our shopping list app, the view would define how the list is presented to the user, and receive the data to display from the model.

### The Controller
The controller contains logic that updates the model and/or view in response to input from the users of the app.

*Using Model*: So for example, our shopping list could have input forms and buttons that allow us to add or delete items. These actions require the model to be updated, so the input is sent to the controller, which then manipulates the model as appropriate, which then sends updated data to the view.

*Directly*: You might however also want to just update the view to display the data in a different format, e.g., change the item order to alphabetical, or lowest to highest price. In this case the controller could handle this directly without needing to update the model.

### Evolution of MVC on the web
In the early days of the Web, MVC architecture was mostly implemented on the server-side, with the client requesting updates via forms or links, and receiving updated views back to display in the browser. However, these days, more of the logic is pushed to the client with the advent of client-side data stores, and XMLHttpRequest allowing partial page updates as required.

## Ember's MVC
Ember implements MVC according to a strict set of concepts and rules. Some important ember resources:

1. [Core Concepts](http://emberjs.com/guides/concepts/core-concepts/)
2. [Naming Conventions](http://emberjs.com/guides/concepts/naming-conventions/) - are enforced strictly to allow associated objects to talk to each other easily without a lot of extra cruft to link them together.
3. [Ember CLI](http://www.ember-cli.com/#overview) - to automate a lot of process

### Ember View layer
In Ember, views are slightly different to how they are in general MVC. In MVC terms, we usually think of the view as being _the entire UI layer_, whereas in [Ember a view](http://emberjs.com/guides/views/) is another specific component of the UI, along with `routes` and `templates`.

1. A typical simple Ember.js UI uses `Route` objects and `template` files to handle Views.
2. Routes specify the URL at which a certain template will be accessible and can also specify what model that template should display data from.
3. Each route is represented by a JavaScript file.
4. Templates specify what the UI should look like, and are represented by Handlebars template files (`.hbs`). These files are HTML with special dynamic expressions inserted in them, contained in double curly braces.
          
    ```html
    <div class="entry">
      <h1>{{title}}</h1>
      <div class="body">
        {{body}}
      </div>
    </div>
    ```
5. These properties will automatically update in the template when the Model/Controller changes.
6. Ember CLI can generate a route and associated template using the following command: `ember generate route my-route-name`. This will generate:
    - A JavaScript file in your-app-root/app/routes to control the route.
    - A Handlebars template in your-app-root/app/templates to define the content that will appear at the named URL.
    - A unit test file in your-app-root/tests/unit/routes where you can define a test for the functionality at your route.
7. The route file will contain a basic skeleton for you to start writing any necessary route logic:
    
    ```javascript
    import Ember from 'ember';
    
    export default Ember.Route.extend({
      // your own features, as required
    });
    ```
8. The route and template files will both be called the same thing, for example shopping-list.js and shopping-list.hbs, so they associate together by default; the view is then available to the user at `your-server.com/shopping-list/`.

### Ember Controllers
Ember.js represents Controllers using controller objects, which are contained in JavaScript files.

1. Ember CLI can generate a controller using the following syntax: `ember generate controller name-of-my-controller`. This will create two new files:
    - A JavaScript file in your-app-root/app/controllers to control that particular model/view.
    - A unit test file in your-app-root/tests/unit/controllers where you can define a test for the functionality in your controller.
2. So for our shopping list example, if we already have a route and template called shopping-list, it would make sense to generate a controller called shopping-list. The generated controller file contains this:
    
    ```javascript
    import Ember from 'ember';
    
    export default Ember.Controller.extend({
      // your own features, as required
    });
    ```

### Ember Models
Ember.js represents Models using model objects, which as before are contained in JavaScript files.

1. Ember CLI can generate a model using the following syntax: `ember generate model name-of-my-model`. This will create two new files:
    - A JavaScript file in your-app-root/app/models to define the data to be associated wth this particular view.
    - A unit test file in your-app-root/tests/unit/models where you can define a test for the functionality in your model.
2. So for our shopping list example, it would make sense to generate a model called shopping-list. The generated model file contains this:
    
    ```javascript
    import DS from 'ember-data';
    
    export default DS.Model.extend({
      // your own features, as required
    });
    ```

## Sample World Clock App
[Original Demo](http://mdn.github.io/world-clock/#/clock), [Original Source Code](https://github.com/mdn/world-clock). 
 
### Installing toolset

External libs used:

1. [Handlebars](http://handlebarsjs.com/) for writing application templates. Ember includes this automatically.
2. [LocalForage](http://mozilla.github.io/localForage/#localforage) for storing and managing offline data. 
3. [Moment Timezone](http://momentjs.com/timezone/) to provide a list of available timezones, and allow us to easily format them in a more readable way.

Steps:

1. Install git
2. Install `node.js`. Ember CLI requires Node.js.
3. Ember-cli `npm install -g ember-cli`

### Setting up

1. **Creating Project**: `ember new world-clock`
2. **Serve**: `ember serve`. Ember CLI also provides a development server that allows you to view and debug your changes as you make them.
3. Open [http://localhost:4200/](http://localhost:4200/).
 
### Ember app structure

1. *dist directory*: whenever Ember CLI builds your application, it places the final production-ready files of your app in this directory. You should never edit any of these files directly, as they will be overwritten any time Ember CLI builds your files.
2. *app directory*: this is where you make direct changes to your application's code. The contents of this directory are built then copied into dist when you run ember serve.
3. *public directory*: where you put raw assets such as fonts and images that don't require any building. This is copied over to the dist directory unchanged when you run ember serve.
4. *package.json*: contains configuration information, such as the name of the app, and what dependencies it has.
5. *Brocfile.js*: contains details of all the 3rd party libraries, assets, etc. that need to be included when your app is built. Ember CLI asset compilation is based on the broccoli asset pipeline tool: [Brocfile.js](https://github.com/joliss/broccoli) is broccoli's build specification file.

A more detailed directory structure explanation can be found [here](http://www.ember-cli.com/user-guide/#folder-layout)

### Routes
![Ember MVC architecture detailed view]({{ site.url }}assets/2015/09/modern-web-app-architecture/ember-mvc-detail-view.png)

We'll start by adding a clock 'page' to our app, to display the current time: `ember generate route clock`. It will be available at [http://localhost:4200/clock](http://localhost:4200/clock).

### Templates

1. Our template file has already been generated at app/templates/clock.hbs. Open it up in your text editor and you'll see that by default this contains only the following:
    
    ```html
    [[outlet]]
    ```
2. Replace {{outlet}} with the following: `<h2>Local Time: <strong>{{localTime}}</strong></h2>`.
3. You'll notice that the page still renders "Welcome to Ember.js", albeit with "Local Time: ", underneath it. This is because the welcome message comes from `app/templates/application.hbs`. If you look in that file, you'll see two lines:
    
    ```html
    <h2 id="title">Welcome to Ember.js</h2>
    
    [[outlet]]
    ```
4. When any view is navigated to by the app's user, application.hbs is shown, but with the view's template content inserted too.

### The main application template
`application.hbs` is a special template created when you first created the app, which represents the main, or master application view. You are given this automatically so you'll always have something displayed at the root of your app (i.e. your-server.com/). Whenever you view any "page" in your app, it is actually the `application.hbs` template that is shown, but with the contents of the route's corresponding template inserted in place of the [[outlet]].
 
Since `clock` is going to be the only/main page of the app, it'd be nice to have it appear when we go to the root of our domain.

#### Generating an application route

`ember generate route application`
 
At this point you will be prompted with the question [?] Overwrite app/templates/application.hbs?, as application.hbs already exists. It is fine to go ahead and overwrite it.  You'll notice that localhost:4200/clock will now only display "Local time:", because the new application.hbs only contains [[outlet]], and not the welcome message.

#### Making the application route redirect to the clock route
At this point, open your app/routes/application.js file and update the contents like so:

```javascript
import Ember from 'ember';

export default Ember.Route.extend({
    redirect: function() {
        this.transitionTo('clock');
    }
});
```

Here we are extending the default Ember Route functionality with a [`redirect()`](http://emberjs.com/api/classes/Ember.Route.html#method_redirect) method that will forward any requests for the root of our application to the clock route using [`transitionTo()`](http://emberjs.com/api/classes/Ember.Route.html#sts=transitionTo).

#### Adding links to different displays
In addition to showing the clock on our main page (which we'll fix in the next article!), we're going to want some navigation that allows users to toggle between the main clock view, and a view allowing them to choose new timezones to add to the main display.

Update the contents of `app/templates/application.hbs` to look like this:

```html
<h1 id='title'>It's 5'o'clock somewhere</h1>

<ul>
    <li>{{#link-to 'clock'}}Clock{{/link-to}}</li>
    <li>{{#link-to 'timezones'}}Manage Timezones{{/link-to}}</li>
</ul>

{{outlet}}
```

`{{#link-to}}` is a built-in Handlebars helper for creating links — it takes one parameter, the route you want to link to.

If you go to look at your application now, you'll notice the page is blank. We've lost our "Local time:" heading. We must have an error in our code somewhere, so let's go and investigate now.

### Debugging Practice
If you open the console tab in your developer tools, you should see an error coming specifically from Ember: `Uncaught Error: There is no route named timezones`.

This is because we are trying to link to a timezones route that does not yet exist. We'll deal with this in a later article, so for now, update the line `<li>{{#link-to 'timezones'}}Manage Timezones{{/link-to}}</li>` to simply `<li>Manage Timezones</li>`.

## Controllers

### Generating a controller for our clock display
Create the controller `ember generate controller clock`.

Open up `app/controllers/clock.js`. To set the value of localTime, we will add it as a property within Ember.Controller.extend().

```javascript
import Ember from 'ember';

export default Ember.Controller.extend({
  localTime: new Date().toLocaleTimeString()
});
```

Any property inside the controller is accessible inside that controller’s template (in this case `app/templates/clock.hbs`).

### Making the clock update every second
So far, our clock display just shows the time when the controller was initialized — and then doesn't change. This is not very useful; we ideally want to update it every second.

```
import Ember from 'ember';

export default Ember.Controller.extend({
    init: function() {
        // Update the time.
        this.updateTime();
    },
 
    updateTime: function() {
        var _this = this;
 
        // Update the time every second.
        Ember.run.later(function() {
            _this.set('localTime', new Date().toLocaleTimeString());
            _this.updateTime();
        }, 1000);
    },
 
    localTime: new Date().toLocaleTimeString()
});
```

The two methods you've added are as follows:

1. `init()` is a special method in Ember that runs automatically when the controller is first initialized. You can put whatever set up code you might need here; in this case you are just invoking the updateTime() method.
2. `updateTime()` uses [`Ember.run.later()`](http://emberjs.com/api/classes/Ember.run.html#method_later) to run the code inside the contained function after 1000ms. The function sets a new value for the localTime property (using the Ember [`set()`](http://emberjs.com/api/classes/Ember.Controller.html#method_set) method), then runs the `updateTime()` function again, so the time is updated after each new second has gone by.

This demonstrates how to work with Ember’s data-binding between controllers and templates.

> Note: var `_this = this;` is used because of variable scoping in functions — we want to run the functions inside `Ember.run.later(function() { ... });` on `this` as it relates to `updateTime: function() { ... }`, so we store a reference to this inside the `_this` variable before we run the inner function. [Jack Franklin's Scope and this in JavaScript](http://javascriptplayground.com/blog/2012/04/javascript-variable-scope-this/) provides more context and explanation.

## Models and application data

Here we'll add functionality that enables users to choose additional timezones to display when they click on the timezones link.

### Generating resource in Ember
`ember generate model name-of-model` will generate a model.

In Ember terms, model + route + template together make up a resource. To generate a resource: `ember generate resource plural-name-of-model`.

### Generating a resource for our app

1. `ember generate resource timezones`
2. You can also now revert the change we made to `app/templates/application.hbs` — open this file up and change the line `<li>Manage Timezones</li>` to `<li>{{#link-to 'timezones'}}Manage Timezones{{/link-to}}</li>`

### Ember Data
Our app needs a data store — we want each timezone included in our app to have a name and an offset value associated with it, and be able to save our data on the client-side so the app will work offline.

To manage data in Ember apps, we can use [Ember Data](https://github.com/emberjs/data), a library included with Ember CLI that acts as a data store inside your model. You define data structures in your model using Ember Data, reference the model in your route and the controller/template for your app will then have access to that data. Models don't have any data themselves; they just define the properties and behavior of specific instances, which are called `records`.

Ember Data lets you use pretty much any underlying storage mechanism you like to store your application data, as long as there is an `adapter` available to translate data between the actual data storage mechanism you are using, and Ember data records.

When an app requires data:

1. It goes to the model's data store to find it.
2. The data store sends a request to the adapter to return the data to it.
3. The adapter goes to the actual data storage mechanism (IndexedDB, RESTful XHR API, whatever), retrieves the data, and converts it into JSON — which the model can understand.
4. The model receives the JSON, converts it into a data record via a serializer, and sends it back to the app.

In our case, we're going to make use of the `LocalForage` library, which automatically detects data stores available in the browser and selects the most optimal store available (it uses IndexedDB, and then falls back to WebSQL/localStorage for browsers that don't support the former).

To know more about models click [here](http://guides.emberjs.com/v1.10.0/models/).

This code simply specifies what the data structure is going to be, according to Ember — a string to contain each timezone name, and a number to contain each timezone's offset value.

### Storing data with LocalForage
Now we'll install the LocalForage Adapter so we can use it with Ember Data. Do this by running the following command: `ember install:addon ember-localforage-adapter`. un the following bower command with the specific version: `$ bower install localforage -v '1.2.2'`

Once Bower has finished installing the LocalForage package, you should see a new localforage directory under world-clock/bower_components.

### Creating a new Ember adapter
Now that we've included LocalForage and the Ember LocalForage adapter into our app, we have access to an LFAdapter object that we can use to feed data from our data store into Ember Data: we'll use this to create our timezones database.

Ember allows you to generate an adapter file specifically to contain this code, meaning that it can be kept separate from your models, controllers, etc. Let's generate our new adapter file using the following command — run this from the root of your project: `ember generate adapter application`. This generates:

1. A JavaScript adapter file at app/adapters that contains our adapter code.
2. A JavaScript file at tests/unit/adapters for you to include an adapter unit test in.

You may also have to run bower install to install some missing dependencies — this is fine.

With this done, open up the `app/adapters/application.js` and replace the code in it with this:

```javascript
import LFAdapter from 'ember-localforage-adapter/adapters/localforage';

export default LFAdapter.extend({
    namespace: 'WorldTimeZones'
});
```
This code creates a data store called WorldTimeZones in LocalForage and joins it to our Ember Data instance.

### Retrieving records from the data store
Finally, we'll want to return our timezone records as the model for our timezone route. Setting a model (using Ember.Route's [`model method`](http://emberjs.com/api/classes/Ember.Route.html#method_model), also see [Specifying a route's model](http://emberjs.com/guides/routing/specifying-a-routes-model/)) on a route gives the controller and template access to the data specified so that it can be manipulated and displayed.

Open up `app/routes/timezones.js` and modify the code inside to look like this:

```javascript
import Ember from 'ember';

export default Ember.Route.extend({
    model: function() {
        return this.get('store').findAll('timezone');
    }
});
```

This code simply tells Ember to make the timezone data model available at the timezones route: "let me have access to this data when I navigate to this route, so I can control it with my controller and display it with my template".

### Gathering timezone data
We're going to want to let users choose from a list of all timezones. For this we’ll use [Moment Timezone](http://momentjs.com/timezone/), an awesome library for dealing with dates and times in JavaScript. This library will give us a list of all available timezones, and allow us to format them in a more readable way.

In order to make sure our app can access the moment scripts when formatting a timezone, we'll need to edit `ember-cli-build.js` and `.jshintrc` in the root of our project.

1. `ember-cli-build.js` an asset pipeline/dependency manager that Ember CLI uses to include dependencies into a project. Ember CLI will look at `ember-cli-build.js` during build time, and incorporate any dependencies you specify with `app.import()`. This ensures that your application has all the components it needs to function properly.
2. `ember-cli-build.js` will import the moment scripts our application needs. Add the following two lines into `ember-cli-build.js`, below the `var app = new EmberApp();` line:
    
    ```javascript
    app.import('bower_components/moment/moment.js');
    app.import('bower_components/moment-timezone/builds/moment-timezone-with-data-2010-2020.js');
    ```
3. In the `.jshintrc` file, add moment to the `predef` array. This will prevent `jshint`, a code-quality tool included in Ember CLI apps, from throwing errors while checking your code for errors or potential problems.
    
    ```javascript
    "predef": [
        "document",
        "window",
        "-Promise",
        "moment"
      ]
    ```
    
> Warning: So far, we've been able to see any changes to our application automatically reflected in the browser without having to refresh or restart the server. However, whenever you edit ember-cli-build.js, you must restart the Ember server.

### Interacting with timezone models
Our application should allow users to add a timezone from a select menu, or delete a previously selected timezone. As mentioned before, Ember controllers can be used to manipulate data. When creating the clock, we sent information about the current local time from our clock controller to our clock template. In this example, we'll send information from our timezones template to our timezones controller through user interactions.

Let's create a timezones controller that adds the timezone data from Moment.js, and implements two actions: "add" and "remove".

1. `ember generate controller timezones`
2. Now update app/controllers/timezones.js to look like so:
    
    ```javascript
    import Ember from 'ember';
    
    export default Ember.Controller.extend({
        /* create array of timezones with name & offset */
        init: function() {
            var timezones = [];
            for (var i in moment.tz._zones) {
              timezones.push({
                name: moment.tz._zones[i].name,
                offset: moment.tz._zones[i].offset[0]
              });
            }
            this.set('timezones', timezones);
            this._super();
          },
          selectedTimezone: null,
          actions: {
            /* save a timezone record to our offline datastore */
            add: function() {
              var timezone = this.store.createRecord('timezone', {
                name: this.get('selectedTimezone').name,
                offset: this.get('selectedTimezone').offset
              });
              timezone.save();
            },        
            /* delete a timezone record from our offline datastore */
            remove: function(timezone) {
              timezone.destroyRecord();
            }
          }
    });
    ```
    
Next we'll modify the timezones template to use the actions and variables we just created. We can use the `Ember.SelectView` and `{{action}}` helper to call our add and remove methods — update app/templates/timezones.hbs so it looks like this:

```html
<h2>Add Timezone</h2>

<div>{{ view Ember.Select content=timezones selection=selectedTimezone
   optionValuePath='content.offset' optionLabelPath='content.name'}}</div>

<button {{action 'add'}}>Add Timezone</button>

<h2>My Timezones</h2>

<ul>
{{#each model}}
  <li>{{name}} <button {{action 'remove' this}}>Delete</button></li>
{{/each}}
</ul>
```

Also Now I faced a error that `offset` is not defined. Finally I found the solution and answered it on [SO](http://stackoverflow.com/a/32869186/742173).

### Comparing timezones
The last thing we need to do is show these times relative to our local time in our clock route. To do this we need to load the timezone model in the clock route — update the contents of app/routes/clock.js to this:

```javascript
import Ember from 'ember';
export default Ember.Route.extend({
    model: function() {
        return this.get('store').find('timezone');
    }
});
```

In our clock controller, we will update each timezone's current time using moment.js, as well as update our local time — update the contents of app/controllers/clock.js to the following:

```javascript
import Ember from 'ember';

export default Ember.Controller.extend({
    init: function() {
        // Update the time.
        this.updateTime();
    },

    updateTime: function() {
        var _this = this;
 
        // Update the time every second.
        Ember.run.later(function() {
            _this.set('localTime', moment().format('h:mm:ss a'));
 
            _this.get('model').forEach(function(model) {
                model.set('time',
                          moment().tz(model.get('name')).format('h:mm:ss a'));
            });
 
            _this.updateTime();
        }, 1000);
    },

   localTime: moment().format('h:mm:ss a')
});
```

Finally, we will add an {{each}} helper to our clock template that will iterate over the timezones in our model and output their name and time properties to our view — update app/templates/clock.hbs like this:

```html
<h2>Local Time: <strong>{{localTime}}</strong></h2>
 
<ul>
  {{#each model}}
    <li>{{name}}: <strong>{{time}}</strong></li>
  {{/each}}
</ul>
```

## References

1. [Modern web app architecture](https://developer.mozilla.org/en-US/Apps/Build/Modern_web_app_architecture)
2. [Mozilla Recommended Toolchain](https://developer.mozilla.org/en-US/Apps/Quickstart#Mozilla_recommended_tool_chain)
3. [Core Concepts](http://emberjs.com/guides/concepts/core-concepts/)
4. [Naming Conventions](http://emberjs.com/guides/concepts/naming-conventions/)
5. [Ember CLI](http://www.ember-cli.com/#overview)
6. [Ember View](http://emberjs.com/guides/views/)
7. [Handlebars](http://handlebarsjs.com/)
8. [LocalForage](http://mozilla.github.io/localForage/#localforage)
9. [Moment Timezone](http://momentjs.com/timezone/)
10. [Brocfile.js](https://github.com/joliss/broccoli)
11. [Folder Layout](http://www.ember-cli.com/user-guide/#folder-layout)
12. [redirect()](http://emberjs.com/api/classes/Ember.Route.html#method_redirect)
13. [transitionTo()](http://emberjs.com/api/classes/Ember.Route.html#sts=transitionTo)
14. [Jack Franklin's Scope and this in JavaScript](http://javascriptplayground.com/blog/2012/04/javascript-variable-scope-this/)
15. [Ember Data](https://github.com/emberjs/data)
16. [Models](http://guides.emberjs.com/v1.10.0/models/)
17. [Specifying a route's model](http://emberjs.com/guides/routing/specifying-a-routes-model/)
18. [Moment Timezone and _zones](http://stackoverflow.com/a/32869186/742173)