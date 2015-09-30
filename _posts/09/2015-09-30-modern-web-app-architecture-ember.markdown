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