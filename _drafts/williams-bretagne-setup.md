---
layout: post
title: Williams Bretagne website setup
---

Note: for a user or organization site, keep `master` branch and use it instead of `gh-pages`

Create a new empty repository on github: `williams-bretagne`

Generate website:

    $ jekyll new williams-bretagne

Init git:

    $ cd williams-bretagne
    $ git init
    $ git add .
    $ git commit -m "Generated by Jekyll v2.0.3"

Rename `master` to `source`:

    $ git branch -m source

Push `source` to github:

    $ git remote add origin git@github.com:aymerick/williams-bretagne.git
    $ git push -u origin source

Create an orphan `gh-pages` branch:

    $ git checkout --orphan gh-pages
    $ git reset .
    $ rm -r *
    $ rm .gitignore
    $ echo 'Coming soon' > index.html
    $ git add index.html
    $ git commit -m "init"

Push `gh-pages` to github:

    $ git push -u origin gh-pages

Now you should see `Coming soon` message when you browse to: <http://aymerick.github.io/williams-bretagne/>

Let's work in `source` branch:

    $ git checkout source

Checkout `gh-pages` into `_site` directory:

    $ git clone git@github.com:aymerick/williams-bretagne.git -b gh-pages _site

`_site` is already in `.gitignore` so we are just fine.

Let's test our new website:

    $ jekyll serve

Browse to <http://127.0.0.1:4000/> and Bingo!

Install bower:

    $ npm install -g bower
    $ bower init

The `bower.json` file should have been created with something like that:

    {
      "name": "williams-bretagne",
      "version": "0.0.0",
      "homepage": "https://github.com/aymerick/williams-bretagne",
      "authors": [
        "Aymerick <aymerick@jehanne.org>"
      ],
      "license": "MIT"
    }

Install `bootstrap` with bower:

    $ bower install bootstrap --save

Now `bootstrap` and `jquery` files are available here:

    /bower_components/bootstrap/dist/css/bootstrap.min.css
    /bower_components/bootstrap/dist/css/bootstrap.min.js
    /bower_components/jquery/dist/css/jquery.min.js

I really don't want to push the whole `bower_components` directory to github. So let's setup a build system with grunt to only copy the needed files.

Setup npm:

    $ npm init

The `package.json` file should have been created with something like that:

    {
      "name": "williams-bretagne",
      "version": "0.0.0",
      "description": "Site web de l'association Williams Bretagne",
      "main": "_site/index.html",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "repository": {
        "type": "git",
        "url": "git://github.com/aymerick/williams-bretagne.git"
      },
      "author": "Aymerick Jéhanne",
      "license": "MIT",
      "bugs": {
        "url": "https://github.com/aymerick/williams-bretagne/issues"
      },
      "homepage": "https://github.com/aymerick/williams-bretagne"
    }

Install grunt packages:

    $ npm install -g grunt-cli
    $ npm install grunt grunt-bower-task grunt-contrib-connect grunt-contrib-copy --save-dev
    $ npm install grunt-contrib-copy grunt-contrib-sass grunt-contrib-watch grunt-exec --save-dev

Now the fun part begins, let's create the `Gruntfile.coffee` file:

    #global module:false

    "use strict"

    module.exports = (grunt) ->
      grunt.loadNpmTasks "grunt-bower-task"
      grunt.loadNpmTasks "grunt-contrib-connect"
      grunt.loadNpmTasks "grunt-contrib-copy"
      grunt.loadNpmTasks "grunt-contrib-watch"
      grunt.loadNpmTasks "grunt-exec"

      grunt.initConfig

        copy:
          jquery:
            files: [{
              expand: true
              cwd: "bower_components/jquery/dist/"
              src: "jquery.min.js"
              dest: "vendor/js/"
            }]
          bootstrap:
            files: [{
              expand: true
              cwd: "bower_components/bootstrap/dist/css/"
              src: "bootstrap.min.css"
              dest: "vendor/css/"
            },
            {
              expand: true
              cwd: "bower_components/bootstrap/dist/js/"
              src: "bootstrap.min.js"
              dest: "vendor/js/"
            }]

        exec:
          jekyll:
            cmd: "jekyll build --trace"

        bower:
          install: {}

        watch:
          options:
            livereload: true
          source:
            files: [
              "_drafts/**/*"
              "_includes/**/*"
              "_layouts/**/*"
              "_posts/**/*"
              "css/**/*"
              "js/**/*"
              "_config.yml"
              "*.html"
              "*.md"
            ]
            tasks: [
              "exec:jekyll"
            ]

        connect:
          server:
            options:
              port: 4100
              base: '_site'

      grunt.registerTask "build", [
        "copy"
        "exec:jekyll"
      ]

      grunt.registerTask "serve", [
        "build"
        "connect:server"
        "watch"
      ]

      grunt.registerTask "default", [
        "serve"
      ]

@todo `blablabla`

Edit the `index.html` file:

    <link rel="stylesheet" href="{{ "/vendor/css/bootstrap.min.css" | prepend: site.baseurl }}">
    <link rel="stylesheet" href="{{ "/css/main.css" | prepend: site.baseurl }}">

Edit the `_layouts/default.html` file and add those lines before `</body>`:

    <script src="{{ "/vendor/js/jquery.min.js" | prepend: site.baseurl }}"></script>
    <script src="{{ "/vendor/js/bootstrap.min.js" | prepend: site.baseurl }}"></script>

@todo Bootstrap NAV BAR !

Start server:

    $ grunt

Or, if you want verbose logs:

    $ grunt -v

Then browse to <http://127.0.0.1:4100/>. Yeah \o/

Be sure to exclude all dev files from Jekyll build by editing `_config.yml`:

    exclude:
      - "bower_components"
      - "node_modules"
      - "bower.json"
      - "Gruntfile.coffee"
      - "LICENSE"
      - "package.json"
      - "README.md"

And add temporary directories to `.gitignore` file:

    /_site/
    /bower_components/
    /node_modules/
    /vendor/

TODO:

  - Live reload
  - Add a `CNAME` file
  - ...