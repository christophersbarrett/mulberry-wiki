## Setup

To keep regular installation time and hassle to a minimum, the install script does not install the necessary libraries to run the tests. Unfortunately, due to implementation concerns of "bundler", the tool we use to manage dependencies, installing the test libraries requires a small bit of work. Luckily, you'll only need to do this once.

1. cd into your `mulberry` installation directory
1. `cat .bundle/config`

If all you see is something like this:

```
---

BUNDLE_WITHOUT: test
```

You can safely delete the file, e.g.

`rm .bundle/config`

If there are other items in your .bundle/config, delete just the `BUNDLE_WITHOUT:` line.

Now you can run: `bundle install` and it will install the test libraries.

## Setting up .rvmrc

The installation script downloads and installs rvm and creates a gemset to store all of our gems. The mulberry command line interface will load this gemset automatically, but `rake` and other ruby scripts won't. In order to run `rake`, you must create a .rvmrc file in your mulberry root directory that contains the following:

`rvm use 1.9.3@mulberry`

Note this will introduce a harmless warning message "RVM is not a function" when running mulberry commands within the mulberry repo (say, running `mulberry serve` for one of the demo apps).

## Installing chromedriver

You will need chromedriver in order to run the JavaScript tests. You can
[download chromedriver](http://code.google.com/p/chromium/downloads/list)
if you do not already have it installed; make sure you install it somewhere in your $PATH.

This is automatically installed for you by the OSX installer, other platforms will need to install it by hand.

## Running the Tests

In the Mulberry installation directory, simply type:

`rake`

You can also run individual suites:

    rake                  # run all of the tests & jshint. Alised of rake ci
    rake spec             # run the ruby unit tests
    rake integration      # run the javascript integration tests
    rake evergreen:run    # run the javascript unit tests
    rake evergreen:serve  # serve the javascript tests for manual testing
    rake jshint           # run jshint on the js code and js tests
