I suggest we break up information about Mulberry into three categories:

* **Core:** Concepts universal to all Mulberry apps. 
* **Toura (possibly useful):** Specific components and features in the Toura base app that might be useful
* **Toura (probably not useful):** Aspects of the Toura base app that will be left undocumented because they are specific to the Toura problem domain

There are a few other items that are currently not well documented but may have value for users depending on their requirements. They don't necessarily apply to all apps, but they're not necessarily Toura-specific:

* Internationalization
* Search
* Favorites
* Text resizing
* Feature flags
* DevConfig

# Core Topics

These will step through each concept demonstrating the use of the CLI, then delving into the relevant JavaScript. They should each be written so that they can be followed as a step-by-step guide, or referred to randomly.

* Getting Started 
    * Installation
    * Scaffold an App
    * Start the Server
        * run multiple servers with -p
* Make Stuff
    * Components
      * Create a component
      * Put it in a page definition 
          * **ugh, what's that?**
      * Create a Haml or Mustache template
          * Say "Hello World" 
          * Say it With Style
    * Use Real Data
       * Create a Model
       * Put some data in it
       * Use it in your component
    * Page Definitions
       * Create another component
       * Put them both in the same page
       * Make them rows or columns
       * Make them scroll
    * Capabilities
       * Make your components talk to each other
    * Routes
       * Make another page and navigate to it
* Deploy It
 * Install XCode
 * Install Android SDK
 * Using the iOS Simulator
 * Using Weinre
 * Build and Install in Simulator or Device

# Core Reference Outline

## JS

This is (somewhat) in order of importance

    mulberry.component
    mulberry.route
    mulberry.store
    components.buttons._Button
    containers
      _LayoutBox (rename to _Container)
      Page
      Screen
      Region
    _Model
    _Capability
    _PageDef
    toura.* 
    ui/
      BackgroundImage -> Image? SafeImage?
      Scrollable
    _Logging
    _Nls
    app/
      Config
      Data
      Device
      DeviceStorage
      Has
      PhoneGap
        accelerometer
        analytics
        audio
        browser
        camera
        device
        geolocation
        network
        notification
        push
      Router
      UI
      URL -> maybe?

    style/
      _debug-tools
        debug
      _helpers
        icon
        hideable
      _layout

## Core Pub/Sub Topics

    '/app/ready'
    '/app/deviceready'
    '/app/started'
    '/data/loaded', [ data ]
    '/node/view', [ route.hash ]
    '/window/resize'
    '/button/menu'
    '/content/update'
    '/debug/user', [ q ]
    '/page/transition/end'

## Core Command Line

    create
    data
    deploy
    generate
    publish_ota
    scaffold
    serve
    test
    update_themes

# Toura Base App Topics (Possibly Useful)

\* means I think it's more likely that it's useful, usually because it's solving something that's tricky in a mobile app or dealing with PhoneGap quirks

    components/
      _ImageGallery *
      _ImageScroller *
      _MediaPlayer *
      AudioPlayer *
      ChildNodeGrid *
      Favorites *
      GoogleMap *
      Hotspots
      ImageGallery *
      NodeGallery
      PageNav
      PinCaption
      PinInfo
      SearchInput
      SearchResults
      SiblingNav
      VideoPlayer *
      ZoomableImageGallery *
      buttons/
        BackButton.js
        DeleteButton.js
        FavoritesButton.js
        FontSize.js
        HomeButton.js
        SearchButton.js

Toura Base App Topics

    '/fontsize'
    '/' + this.playerType + '/play', [ this.media.id ]
    '/favorite/remove', [ id ]
    '/share', [ topic, [ n ]

# Toura Base App (not worth documenting)
   * all existing page_defs
   * all existing capabilities