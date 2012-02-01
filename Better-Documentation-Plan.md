In order to make the documentation task more manageable, we should break topics up into a three groups:

* **Core:** Concepts universal to all Mulberry apps. Includes:
 * Topic Guides
 * JS API Reference
 * Command line reference
* **Toura (possibly useful):** Specific components in the Toura base app that might be useful. Includes:
 * JS API Reference
* **Toura (probably not useful):** Aspects of the Toura base app that will be left undocumented because they are specific to the Toura problem domain

# Core

## JS

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
    components/
      buttons/
        _Button
    containers/
      _LayoutBox -> _Container
      Page
      Region
      Screen
      Viewport
    models/
      _StorableAsset
      _Updateable
    style/
      _debug-tools
        debug
      _helpers
        icon
        hideable
      _layout
    ui/
      BackgroundImage -> Image? SafeImage?
      Scrollable
    _Capability
    _Component
    _Logging
    _Model
    _Nls
    _PageDef
    _Store
    _View
    Utilities

    vendor
      haml
      iscroll
      urbanairship

## Core Topics

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

# Toura Base App (Possibly Useful)

    app/
      user/
        Facebook
        Favorites
        Twitter
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
      ZoomableImageGallery
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


# Toura Base App (Completely Uninteresting)

    page_defs/
      audio-with-images-phone
      audio-with-images-tablet
      debug
      default
      favorites
      feed-item
      feed-list-phone
      feed-list-tablet
      full-screen-images
      google-map-phone
      google-map-tablet
      grid-view
      home-phone
      home-tablet
      home-with-header-phone
      home-with-header-tablet
      hotspots
      images-and-text-phone
      images-and-text-tablet
      location-list
      node-gallery
      search
      videos-and-text-phone
      videos-and-text-tablet




