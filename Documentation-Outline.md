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

These will step through each concept demonstrating the use of the CLI, then delving into the relevant JavaScript 

   * Routes
   * Page Definitions (including screens and regions)
   * Components
   * Capabilities

# Core Reference Outline

## JS

This is (somewhat) in order of importance

    _View
    _Component
    components.buttons._Button
    containers._LayoutBox (rename to _Container)
    containers.Page
    containers.Screen
    containers.Region
    _Model
    models/
      _StorableAsset
      _Updateable
    _Capability
    _PageDef
    toura.* 
    ui/
      BackgroundImage -> Image? SafeImage?
      Scrollable
    _Logging
    _Nls
    _Store
    app/
      Config
      Data
      DeviceStorage
      Has
      Notifications
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
      Routes
      UI
      URL

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
   all existing page_defs
   all existing capabilities









