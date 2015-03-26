---
layout: post
kind: post
title: Add an upload directory to your ember cli project
description: Make ember cli development server serve files in another directory
tags:
- ember.js
---

I currently work on a projet with a backend in <a href="http://golang.org">Go</a> and a web app powered by <a href="http://emberjs.com">Ember.js</a> with an image upload feature.

When developing, I use the server provided by <a href="http://www.ember-cli.com">ember cli</a> with a proxy enabled to send api requests to my backend:

    $ ember server --proxy http://127.0.0.1:<BACKEND_PORT>

All requests starting with `/api` are proxied to the backend, and the ember cli server serves the ember app files and the assets, with a nice live-reload feature. This setup is perfect for development.

However, how to handle uploaded files ? My backend needs to store them somewhere on the file system where the ember cli server can serves them too.

You may think that storing them inside the `/public` directory of the ember cli project is the solution. But this is not the way to go, because everything in the `/public` folder is copied into the `/dist` directory when building your app, including for production. So that's a bad idea.

I searched how to make the ember cli server serve another directory. I'm not sure this is the best way to do it, but here is the solution I came to.

First let's create the `upload` directory in the ember cli project:

    $ cd myapp
    $ mkdir upload

Add a `/upload/test.html` file with this content:

    This file is in the upload directory

Now let's generate a `server` directory:

    $ ember g server

![ember g server](/img/emberjs/ember_cli_server_01.png)

The main purpose of the new `server` directory is to mock server enpoints.

Open the `/server/index.js` file and add those lines:

{% highlight javascript %}
var express = require('express');
app.use('/upload', express.static(__dirname + "/../upload"));
{% endhighlight %}

For example, my file looks like that:

{% highlight javascript %}
// To use it create some files under `mocks/`
// e.g. `server/mocks/ember-hamsters.js`
//
// module.exports = function(app) {
//   app.get('/ember-hamsters', function(req, res) {
//     res.send('hello');
//   });
// };

module.exports = function(app) {
  var globSync   = require('glob').sync;
  var mocks      = globSync('./mocks/**/*.js', { cwd: __dirname }).map(require);
  var proxies    = globSync('./proxies/**/*.js', { cwd: __dirname }).map(require);

  // Log proxy requests
  var morgan  = require('morgan');
  app.use(morgan('dev'));

  mocks.forEach(function(route) { route(app); });
  proxies.forEach(function(route) { route(app); });

  // Permits to serve files in /upload directory
  var express = require('express');
  app.use('/upload', express.static(__dirname + "/../upload"));
};
{% endhighlight %}

Start the server:

    $ ember server

And now browse to: <http://127.0.0.1:4200/upload/test.html>

![serving upload dir](/img/emberjs/ember_cli_server_02.png)

Victory \o/

My backend can now save uploaded file into the `/upload` directory of my ember cli project, and the ember cli server can serves them.

![victory](/img/emberjs/ember_cli_server_victory.jpg)

You can now continue having fun with <a href="http://emberjs.com">Ember.js</a>.

<a href="https://twitter.com/aymerick">@aymerick</a>
