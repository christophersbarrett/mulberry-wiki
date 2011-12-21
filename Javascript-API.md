The Mulberry JavaScript framework provides a number of useful methods that you
can use when creating a custom application using the Mulberry toolkit.

Note that the `mulberry` “namespace” is just an alias to the `toura` namespace;
all `mulberry.*` methods can be accessed as `toura.*` as well, and they are
generally defined in the framework code as `toura.*`.


## Building Blocks

### mulberry.component(name, prototype)

*Defined in: `app/toura/components/_Component.js`.*

Creates and returns a constructor for a
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

### mulberry.capability(name, prototype)

*Defined in: `app/toura/capabilities/_Capability.js`*

Creates a capability, which can be added to a template definition to describe
the interaction between components on a page. A capability created using
`mulberry.capability` can be referred to in a template definition as
`CapabilityName`. See [[Capabilities]] for more details.

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

*Defined in: `app/toura/Utilities.js`*

Creats a datasource using the provided name and prototype. The datasource can
be instantiated for application-wide use at application start by subscribing to
the `/app/started` topic, or it can be instantiated as needed in individual
capabilities or components. In either case, the datasource is availalbe at
`client.data.DatsourceName`.

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

Defined in `app/toura/app/Router.js`.

### mulberry.route(route, handler, isDefault)

Defines a custom route for the application. The route argument can be a string
or a regular expression; for examples, see
`toura_app/javascript/toura/app/Routes.js`. The handler argument is a
function to be used to handle the route. It receives two arguments: a params
object containing parameters contained in the URL, and an augmented route
object with information about the route being run. The isDefault argument is
optional; if true, the route will be used as the default (home) route for the
application.

### mulberry.routes(routesArray)

Defines a set of custom routes for the application using an array of routes
objects. Each object must have a `route` and a `handler` property; each
object may optionally include an `isDefault` property. These properties work
just like the arguments to `mulberry.route`.

### mulberry.app.Router.go(hash)

Directs the application to go to the specified hash.

### mulberry.app.Router.home()

Directs the application to go to the home/default route.

### mulberry.app.Router.back()

Directs the application to go back one page.


## Page Creation

*Defined in `app/toura/app/PageFactory`*

### mulberry.app.PageFactory.createPage(pageData)

Directs the application to construct a page based on the provided `pageData`
object. The object should include a `pageController` property to indicate which
pre-defined template to use; if one is not provided, the Default template will
be used. The entire `pageData` object will be available to all components on
the page via their `baseObj` property. For usage examples, see
`app/toura/app/Routes.js`.

## User Interface

*Defined in `app/toura/app/UI.js`*

### mulberry.app.UI.showPage(page)

Places the provided page object in the user interface, and transitions from the
old page to the new page.

### mulberry.app.UI.viewport

An object containing information about the dimensions of the viewport.

### mulberry.app.UI.currentPage

An object containing information about the current page.


## PhoneGap/Device

### mulberry.app.PhoneGap.registerAPI(name, moduleFunction)

*Defined in `app/toura/app/PhoneGap.js`*

Creates an entry in the `mulberry.app.PhoneGap` namespace with the provided
name, exposing the module that is returned when the moduleFunction is executed.
The moduleFunction receives two arguments: a boolean indicating whether
PhoneGap is present in the current environment, and an object containing
information about the current device.

Here is an example:

    mulberry.app.PhoneGap.registerAPI('custom', function(phonegap, device) {
      return {
        logDeviceType : function() {
          console.log(device.type);
          if (phonegap) {
            console.log('Phonegap is available, too!');
          }
        }
      }
    });

    mulberry.app.PhoneGap.custom.logDeviceType();

### mulberry.app.PhoneGap.network

*Defined in `app/toura/app/PhoneGap/network.js`*

- `isReachable()` Returns a promise that will resolve true if a network is
  available; the promise resolves false otherwise.

### mulberry.app.PhoneGap.notification

*Defined in `app/toura/app/PhoneGap/notification.js`*

- `alert()` Displays an alert notification.

### mulberry.app.PhoneGap.browser

*Defined in `app/toura/app/PhoneGap/browser.js`*

- `url(url)` Opens a ChildBrowser at the specified URL.
- `getBrowser()` Returns the ChildBrowser instance.


## Config

*Defined in `app/toura/app/Config.js`*

### mulberry.app.Config.get(key)

Returns the value of the specified configuration key. Information about an app,
for example, is available via `mulberry.app.Config.get('app');`

### mulberry.app.Config.set(key, value)

Sets the value of the specified configuration key to the provided value.

## Device Storage

### mulberry.app.DeviceStorage.get(key)

Gets the value of the specified key from device storage.

### mulberry.app.DeviceStorage.set(key, value)

Stores the value of the specified key in device storage.

### mulberry.app.DeviceStorage.drop()

Clears *all* device storage, including SQLite and local storage.

## Utilities

*Defined in `app/toura/Utilities.js`.*

### mulberry.tmpl(string, dataObject)

Returns a string based on substituting placeholders in the provided string with
values from the provided dataObject. This is an alias for
[`dojo.string.substitute`](http://dojotoolkit.org/api/1.6/dojo/string/substitute).
For more details, see [this article from Daniel Eric Lee](http://www.enterprisedojo.com/2011/10/14/coast-to-coast-with-dojo-string-substitute/).

### mulberry.haml(hamlTemplateString)

Given a [Haml template](https://github.com/creationix/haml-js), returns a
function that can be passed a data object. The returned function will use the
data object to populate the template, and return a string.

    var templateFn = mulberry.haml(".foo= bar");

    templateFn({ bar : 'baz' }); // <div class="foo">baz</div>

### mulberry.jsonp(url)

Returns a promise that will resolve with the JSON returned from the provided URL.

    var response = mulberry.jsonp('http://foo.com');

    response.then(function(data) {
      console.log(data);
    });


## Useful pub/sub topics

The JavaScript framework publishes several useful "topics" at key points in the
application's lifecycle. You can add a subscription to any of these topics via
`dojo.subscribe` in order to trigger certain actions at certain times:

    dojo.subscribe('/app/started', function() {
      // things to do when the app is started
    });

Note that the `dojo.subscribe` method returns a "handle" that can be passed to
`dojo.unsubscribe` if the subscription is no longer needed.

### /app/deviceready

Subscriptions to this topic will run as soon as the device is ready. In a
PhoneGap environment, this topic is triggered when the `deviceready` event
fires; in a browser environment, this topic is triggered when the `body`
element's `onload` event fires. *This is intended for internal use only, but it
may prove useful to you.* Triggered in `toura_app/javascript/toura/base.js`.

### /app/ready

Subscriptions to this topic will run as soon as the data for the application
has been bootstrapped, but before the DOM has been populated.  *This topic is
also intended for internal use only, but it may prove useful to you.*
Triggered in `app/toura/base.js`.

### /app/started

Subscriptions to this topic will run once the initial view of the application
has been displayed. Triggered in `app/toura/base.js`.

### /router/handleHash/after

Subscriptions to this topic will run once a change in the URL's hash has been
handled; note that the new content may not yet be displayed at this time.
Triggered in `app/toura/app/Router.js`.

### /node/view

Subscriptions to this topic will run when any page specified via markdown is
viewed; subscribers to this topic will receive an argument containing the URL
of the page that was viewed. Triggered in
`app/toura/app/Routes.js`.

### /window/resize

Subscriptions to this topic will run whenever the window is resized. Triggered
in `app/toura/app/UI.js`.

### /button/menu

Subscriptions to this topic will run whenever a device's menu button is
pressed. Triggered in `app/toura/app/UI.js`.

### /page/transition/end

Subscriptions to this topic will run when the application is finished
transitioning to a new page. Triggered in
`app/toura/containers/Pages.js`
