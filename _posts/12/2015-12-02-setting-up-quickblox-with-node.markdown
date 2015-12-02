---
layout: post
title:  Setting up quickblox with node
date:   2015-12-02 11:47:00
categories: programming node cht
tags: quickblox node npm express
---

In this post I am going to set a node server to use Quickblox.

1. `brew uninstall node`
2. `npm install -g express`
3. Starting from express 4.00 you also need to install express generator as mentioned [here](http://stackoverflow.com/a/23266782/742173).
    `npm install -g express-generator`
4. Generate new project: `express chat`
5. Install node modules: `cd chat/ && npm install`
6. Git-ignore `node_modules` directory
7. Start server: `DEBUG=chat:* npm start`
8. Check if its working [localhost:3000](http://localhost:3000/)
9. Install http status codes: `npm install http-status-codes --save`
10. Modify `routes/index.js` to return json instead of rendering `index.html`:
    
    ```javascript
    router.get('/', function (req, res, next) {
        res.status(HttpStatus.OK).json({title: 'Express'});
    });

    ```
11. Restart server and test.
12. Install quickblox: `npm install quickblox --save`
13. 10. Modify `routes/index.js` to initialize quickblox:
    
    ```javascript
    var QB = require('quickblox');
    
    // initalise the environmenet with my application id, authentication key and authentication secret
    QB.init(<CLIENTID>, <KEY>, <SECRET>);
    
    // create an user session
    var params = {login: 'system', password: 'welcome123'};
    QB.createSession(params, function (err, result) {
        if (err) {
            console.log('Something went wrong: ' + err);
        } else {
            console.log('Session created with id ' + result.id);
        }
    });
    
    /* GET home page. */
    router.get('/', function (req, res, next) {
        // list the users currently enrolled
        QB.users.listUsers(function (err, result) {
            for (var i = 0; i < result.items.length; i++) {
                console.log('User ' + result.items[i].user.login + ' is registered');
            }
        });
        res.status(HttpStatus.OK).json({title: 'Express'});
    });
    ```
14. Restart and test if users are getting printed.
15. Shift the QB initialisation code to `app.js` and make QB a global var as mentioned [here](http://stackoverflow.com/questions/9765215/global-variable-in-app-js-accessible-in-routes), because we have to use `QB` in multiple routes. 
    
    ```javascript
    //Initialize quickblox
    QB = require('quickblox');
    
    // initalise the environmenet with my application id, authentication key and authentication secret
    QB.init(31620, 'HGKzgragj54ktge', 'YeQ-bXJ7asULOU-');
    
    // create an user session
    var params = {login: 'system', password: 'welcome123'};
    QB.createSession(params, function (err, result) {
        if (err) {
            console.log('Something went wrong: ' + err);
        } else {
            console.log('Session created with id ' + result.id);
        }
    });
    ```
14. Restart and test if users are getting printed. 

## References
1. [Node](https://nodejs.org/en/)
2. [Quickblox](http://quickblox.com/)
3. [Express](http://expressjs.com/en/index.html)
4. [express command not found in bash after installing it with npm](http://stackoverflow.com/questions/6103121/express-command-not-found-in-bash-after-installing-it-with-npm)
5. [Global Variable in app.js accessible in routes?](http://stackoverflow.com/questions/9765215/global-variable-in-app-js-accessible-in-routes)
6. [Build a RESTful API Using Node and Express 4](https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4)
7. [Node.js - RESTful API](http://www.tutorialspoint.com/nodejs/nodejs_restful_api.htm)
8. [npm - QuickBlox JavaScript SDK](https://www.npmjs.com/package/quickblox)
9. [Guide for running with node.js, express, jade, and mongodb](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/)
10. [npm-http-status-codes](https://www.npmjs.com/package/http-status-codes)
11. [How can run app using Express 4 without 'DEBUG=node:* ./bin/www'](How can run app using Express 4 without “DEBUG=node:* ./bin/www”)
12. [Quickblox - js sdk](http://quickblox.com/developers/Javascript#Test_API:_Spec_Runner)