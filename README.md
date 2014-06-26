aymerick.github.io
==================

Some stuff...

Development
===========

Install tools:

    npm install -g grunt-cli
    npm install -g bower
    gem install bundle

Install dependencies:

    npm install
    bower install
    bundle install

Build the site and watch for changes:

    grunt -v

Browse: http://127.0.0.1:4000

Repository setup
================

Rename local master to source:

    $ git branch -m master source

Push remote source:

    $ git push -u origin source

Delete remote master:

    $ git push origin :master

Create new local master (empty):

    $ git checkout --orphan master
    $ git rm --cached -r .
    $ echo 'soon' >> index.html
    $ git add index.html
    $ git commit -m "init"

Push remote master:

    $ git push -u origin master

Checkout master in _deploy subdirectory:

    $ mkdir _deploy
    $ cd _deploy
    $ git init
    $ echo 'Coming soon' > index.html
    $ git add .
    $ git commit -m "Init"
    $ git remote add origin git@github.com:aymerick/aymerick.github.com.git
    $ ...

References
==========

- Jekyll: <http://jekyllrb.com>
- Pygment CSS: <https://github.com/richleland/pygments-css/blob/master/zenburn.css>
- Build system inspirations:
  - <https://github.com/myna/help>
  - <http://winstonyw.com/2013/02/24/jekyll_haml_sass_and_github_pages>
