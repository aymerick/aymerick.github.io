aymerick.github.io
==================

Some stuff...

Development
===========

Install tools:

    $ npm install -g grunt-cli
    $ npm install -g bower
    $ gem install bundle

Install dependencies:

    $ npm install
    $ bower install
    $ bundle install

Build the site and watch for changes:

    $ grunt -v

Browse <http://127.0.0.1:4000>

Deploy to Github Pages:

    $ rake deploy

Repository setup
================

Rename local `master` to `source`:

    $ git branch -m master source

Push `source` to remote:

    $ git push -u origin source

Delete remote `master`:

    $ git push origin :master

Create new local `master` (empty):

    $ git checkout --orphan master
    $ git rm --cached -r .
    $ echo 'Coming soon' > index.html
    $ git add index.html
    $ git commit -m "init"

Push `master` to remote:

    $ git push -u origin master

Checkout `master` in _deploy subdirectory:

    $ git clone git@github.com:aymerick/aymerick.github.io.git -b master _deploy

References
==========

- Jekyll: <http://jekyllrb.com>
- Pygment CSS: <https://github.com/richleland/pygments-css/blob/master/zenburn.css>
- Build system/workflow inspirations:
  - <https://github.com/myna/help>
  - <http://winstonyw.com/2013/02/24/jekyll_haml_sass_and_github_pages>
  - <https://github.com/imathis/octopress>
