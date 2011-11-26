The Mulberry JavaScript framework provides a number of useful methods that you
can use when creating a custom application using the Mulberry toolkit.

Note that the `mulberry` “namespace” is just an alias to the `toura` namespace;
all `mulberry.*` methods can be accessed as `toura.*` as well, and they are
generally defined in the framework code as `toura.*`.

## Creating Custom Components, Capabilities, and Data Sources

### mulberry.component

_Defined in: `toura\_app/javascript/toura/components/\_Component.js`

`mulberry.component(name, prototype)` Creates and returns a constructor for a
component with the provided name using the provided prototye. The created
component can be used as part of a template definition as
`custom.ComponentName` or programmatically as
`client.components.ComponentName`. See [[Creating Custom Components]] for more
details.

Here is an example of a component defined with mulberry.component:

    mulberry.component('WidgetA', {
      componentTemplate : dojo.cache('client.components', 'WidgetA/WidgetA.haml'),
      prep : function() {
        var images = this.baseObj.images;
      },

      init : function() {
        this.connect(this.domNode, 'click', '_wiggle');
      },

      _wiggle : function(e) {
        e.preventDefault();

        // ... code to do the wiggling ...

        this.onWiggle();
      },

      onWiggle : function() {
        // method for capability connection
      }
    });

### mulberry.capability

_Defined in: `toura\_app/javascript/toura/capabilities/\_Capability.js`

`mulberry.capability(name, prototype)` Creates a capability, which can be added
to a template definition to describe the interaction between components on a
page. A capability created using `mulberry.capability` can be referred to in a
template definition as `CapabilityName`. See [[Capabilities]] for more details.

Here is an example of a capability that describes how WidgetB should behave
when WidgetA's `onWiggle` method is called:

    mulberry.capability('WidgetA_WidgetB', {
      requirements : {
        widgetA : 'custom.WidgetA',
        widgetB : 'custom.WidgetB'
      },

      connects : [
        [ 'widgetA', 'onWiggle', 'handleWiggle' ]
      ],

      handleWiggle : function() {
        widgetB.jiggle();
      }
    });

### mulberry.datasource

_Defined in: `toura\_app/javascript/toura/Utilities.js`

`mulberry.datasource(name, prototype)` Creats a datasource using the provided
name and prototype. The datasource can be instantiated for application-wide use
at application start by subscribing to the `/app/started` topic, or it can be
instantiated as needed in individual capabilities or components. In either
case, the datasource is availalbe at `client.data.DatsourceName`.

    mulberry.datasource('Google', {
      search : function(term) {
        return mulberry.jsonp('http://www.google.com?q=' + term + '&format=json');
      }
    });

    var s = dojo.subscribe('/app/started', function() {
      client.data.Google = new client.data.Google();
      dojo.unsubscribe(s);
    });

## Routing

For details on these methods, see `toura\_app/javascript/toura/app/Router.js`.

- `mulberry.route(route, handler, isDefault)` Defines a custom route for the
  application. The route argument can be a string or a regular expression; for
  examples, see `toura\_app/javascript/toura/app/Routes.js`. The handler
  argument is a function to be used to handle the route. It receives two
  arguments: a params object containing parameters contained in the URL, and an
  augmented route object with information about the route being run. The
  isDefault argument is optional; if true, the route will be used as the
  default (home) route for the application.
- `mulberry.routes(routesArray)` Defines a set of custom routes for the
  application using an array of routes objects. Each object must have a `route`
  and a `handler` property; each object may optionally include an `isDefault`
  property. These properties work just like the arguments to `mulberry.route`.
- `mulberry.app.Router.go(hash)` Directs the application to go to the specified
  hash.
- `mulberry.app.Router.home()` Directs the application to go to the
  home/default route.
- `mulberry.app.Router.back()` Directs the application to go back one page.

## Page Creation

- `mulberry.app.PageFactory.createPage(pageData)` Directs the application to
  construct a page based on the provided `pageData` object. The object should
  include a `pageController` property to indicate which pre-defined template to
  use; if one is not provided, the Default template will be used. The entire
  `pageData` object will be available to all components on the page via their
  `baseObj` property. For usage examples, see
  `toura\_app/javascript/toura/app/Routes.js`.

## User Interface

- `mulberry.app.UI.set(prop, val)`
- `mulberry.app.UI.showPage(page)`
- `mulberry.app.UI.viewport`
- `mulberry.app.UI.currentPage`

## PhoneGap/Device

- `mulberry.app.PhoneGap.registerAPI`
- `mulberry.app.PhoneGap.network`
  - isReachable()
- `mulberry.app.PhoneGap.notification`
  - alert()
- `mulberry.app.PhoneGap.browser`
  - url(url)
  - getBrowser()

## Config

- `mulberry.app.Config.get(key)`
- `mulberry.app.Config.set(key, value)`

## Device Storage

- `mulberry.app.DeviceStorage.get(key)`
- `mulberry.app.DeviceStorage.set(key, value)`
- `mulberry.app.DeviceStorage.drop()`

## Utilities

- `mulberry.tmpl(string, dataObject)`
- `mulberry.haml(hamlTemplateString)`
- `mulberry.jsonp(url)`

# Useful pub/sub topics

The JavaScript framework publishes several useful "topics" at key points in the
application's lifecycle. You can add a subscription to any of these topics via
`dojo.subscribe` in order to trigger certain actions at certain times:

    dojo.subscribe('/app/started', function() {
      // things to do when the app is started
    });

Note that the `dojo.subscribe` method returns a "handle" that can be passed to
`dojo.unsubscribe` if the subscription is no longer needed.

## Application state

- `/app/deviceready` Subscriptions to this topic will run as soon as the device
  is ready. In a PhoneGap environment, this topic is triggered when the
  `deviceready` event fires; in a browser environment, this topic is triggered
  when the `body` element's `onload` event fires. *This is intended for
  internal use only, but it may prove useful to you.* Triggered in
  `toura\_app/javascript/toura/base.js`.
- `/app/ready` Subscriptions to this topic will run as soon as the data for the
  application has been bootstrapped, but before the DOM has been populated.
  *This topic is also intended for internal use only, but it may prove useful
  to you.* Triggered in `toura\_app/javascript/toura/base.js`.
- `/app/started` Subscriptions to this topic will run once the initial view of
  the application has been displayed. Triggered in
  `toura\_app/javascript/toura/base.js`.
- `/router/handleHash/after` Subscriptions to this topic will run once a change
  in the URL's hash has been handled; note that the new content may not yet be
  displayed at this time. Triggered in
  `toura\_app/javascript/toura/app/Router.js`.
- `/node/view` Subscriptions to this topic will run when any page specified via
  markdown is viewed; subscribers to this topic will receive an argument
  containing the URL of the page that was viewed. Triggered in
  `toura\_app/javascript/toura/app/Routes.js`.

## User Interface

- `/window/resize` Subscriptions to this topic will run whenever the window is
  resized. Triggered in `toura\_app/javascript/toura/app/UI.js`.
- `/button/menu` Subscriptions to this topic will run whenever a device's menu
  button is pressed. Triggered in `toura\_app/javascript/toura/app/UI.js`.
- `/page/transition/end` Subscriptions to this topic will run when the
  application is finished transitioning to a new page. Triggered in
  `toura\_app/javascript/toura/containers/Pages.js`
