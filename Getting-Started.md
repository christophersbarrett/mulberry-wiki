## Installation

**You should read the [README](https://github.com/Toura/mulberry/blob/master/README.md) 
for installation instructions before proceeding with the rest of the steps below.**

## Scaffold

Mulberry lets you quickly create the basic structure for an application at the
location you specify. Run this from the directory where you want to create a new
directory that will contain your app.

    mulberry scaffold myapp

## Configure

The scaffolding step will create a few files and directories. You'll need to
edit the file `config.yml` to provide some basic configuration information. See
the notes in the file for details.

## Construct

The scaffolding step also creates a `sitemap.yml` file. Edit this to describe
the structure of your application. Note that the home and about pages are
created for you automatically, and generally are the only two pages at the top
level of the sitemap; all other pages should be placed below the home page.

    - home:
      - schedule
      - speakers
      - points-of-interest
    - about

In order for your app to work, you'll need a Markdown file in the pages
directory for each page in the sitemap. These files also include some metadata
in the form of a YAML header. At a minimum, you need to specify the title to
display for the page, and the template you want to use. Make sure that the file
you create has the same name as the entry in the sitemap; for example, the home
page should be placed at `pages/home.md`:

    ---
    title: CapitolJS
    template: Home
    ---

    Good afternoon agent. Your mission, should you choose to accept it, involves
    the learning of the programming language JavaScript and how to use it properly
    and effectively. You will be joined by an intimate task force of elite members
    of the JavaScript community to assist you in this task. You may select to
    attend either or both of the evening social events occurring the night before
    and night of the event. This event is happening on September 18th, 2011 in
    Washington, DC at the [Hotel Palomar](http://www.hotelpalomar-arlington.com/).
    If you accept this mission, be sure to follow
    [@capitoljs](http://www.twitter.com/capitoljs) and register today, like all
    JSConf events - it will sell out. See you in Washington DC.

For more on creating pages, see the Creating Pages and Assets documentation.

## Customize

Mulberry allows extensive customization of your application. See the documentation
in the repository [Wiki](https://github.com/toura/mulberry/wiki) for details.

## Develop

For rapid iteration, you can run your app in a desktop WebKit browser and get
access to your standard development tools. Run the following from the root of
your project:

    mulberry serve

## Test

Generate test builds that you can run in a simulator or on a properly
provisioned device. Run the following from the root of your project:

    mulberry test

## Deploy

Create binaries for the various form factors and operating systems your app
supports. Run the following from the root of your project:

    mulberry deploy
