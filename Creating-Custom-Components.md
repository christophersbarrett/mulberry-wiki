Mulberry exposes a simple API for creating custom components that you can use on
your own page definitions. This section explains how you might create a simple
component for displaying a Twitter feed.

To start, run the following command from your app directory:

    mulberry create component MyCustomComponent

This will:

- create a component JavaScript file at `javascript/components/MyCustomComponent.js` that uses `mulberry.component` to define the component. The `mulberry.component` method receives the name of the component, and an object to be used as the component's prototype (see the information below about Component Lifecycle Methods and Component Instance Methods).
- create a component resources directory at `javascript/components/MyCustomComponent/`
- create a component template file at
  `javascript/components/MyCustomComponent/MyCustomComponent.haml` that sets the component's initial template as a simple `<div>`.
- add the appropriate `dojo.require` statement to your project's base
  JavaScript file, located at `javascript/base.js`
- create a SCSS file at `javascript/components/MyCustomComponent/_my-custom-component.scss`
- add the appropriate `@import` statement to your project's base SCSS file.

The rest of this page explains the features and methods available to custom components. You may want to [read this post that shows an example of creating a custom component](http://mulberry.toura.com/blog/2011/10/30/custom-functionality/).

### Querying the DOM of Your Component

If your component code needs to refer to a specific element in your component's
DOM structure, you can query that element using jQuery or dojo.query, but a
better solution is to label the element as a `dojoAttachPoint` in the component template.

    %div.component.my-custom-component
      %button{ dojoAttachPoint : 'refreshButton' }
      %ul{ dojoAttachPoint : 'list' }

These nodes will automatically be available to you as `this.refreshButton` and
`this.list` in your component code.

### Component Properties

Component instances have built-in properties that you can take advantage of.

- `baseObj` An object representing the page on which the component is being displayed. This
object contains all of the asset data associated with the page, as well as useful methods. 
- `device` A object containing information about the device environment, with a `type` and
`os` property.
- `domNode` The root DOM node.
- `$domNode` A jQuery object containing the component's root DOM node. Available only if your project has jQuery enabled.
- `region` The region in which the component is displayed.
- `screen` The screen on which the component is displayed.

### Component Properties You Can Provide

- `componentTemplate` A string that can be parsed as Haml that will be used to construct the
component. This can be a plain string, or a string returned by `dojo.cache`
(see the example above). If you do not provide a `componentTemplate` property,
then the default is `%div`.

### Component Lifecycle Methods

Mulberry's extensible component system offers a number of "lifecycle" methods, allowing you to control aspects of a component's behavior at various points. 

These methods run in order when a component is placed on a page. Here are those methods in the order in which they are called:

* `prep`: Prepare the data that your component will need. In this method, you can rearrange data that's contained in the component's `baseObj` property, which will contain all of the information that Mulberry knows about the page that's using the component. This method is part of the default component skeleton that Mulberry generates for you.
* `setupChildComponents`: Runs after the component is created, but before it is visible on the page. If you want to programmatically add child components to your component prior to displaying it, this is the place to do it. Be sure to read about `this.adopt`, below, which allows you to add components in a way that they will be properly torn down when the parent component is removed from the page.
* `adjustMarkup`: You can make changes to your component's markup here. For instance, if you need to dynamically add a class, this is the place to do it. This method fires after the component's DOM has been created, but before it is visible on the page.
* `setupConnections`: You should bind to DOM events (`click`, `focus`, etc.) here using `this.connect`. More information on creating connections is available in the [Dojo documentation](http://dojotoolkit.org/reference-guide/dojo/connect.html).
* `setupSubscriptions`: If you need your component to subscribe to a topic, this is where you should add that subscription using `this.connect`. This is the last lifecycle method to execute before the component is added to the page. More information on creating subscriptions is available in the [Dojo documentation](http://dojotoolkit.org/reference-guide/dojo/subscribe.html). See also [dojo.publish](http://dojotoolkit.org/reference-guide/dojo/publish.html).
* `resizeElements`: You can adjust the size of your component or its elements here. This code will run after the DOM is loaded and visible on the page.
* `init`: Once the component has been placed on the page, you can do any setup that's required in this method. Like `prep`, `init` is a method on the default generated component.
* `teardown`: Perform anything that needs to be done when a component instance is no longer needed. In particular, this is the place to clean up objects that might remain in memory when the component is destroyed. `tearDown` is called right before the component instance is destroyed, which in practice is usually when the user navigates away from the page containing the component. Note that any subscriptions or event connections that you create using `this.connect` or `this.subscribe` do not have to be torn down; they will be torn down automatically.

All of these methods are defined in `toura_app/javascript/toura/components/_Component.js`, so you can look there for more details. You may also find it useful to explore the built-in components in `toura_app/javascript/toura/components/` for examples of how these lifecycle methods can be used.

To take advantage of these methods, add the method to the object you pass to `mulberry.component` (see `prep` and `init` in the generated component for examples). Then add your custom code to the contents of the method.

### Component Instance Methods

Components expose a number of useful instance methods. For example usage, see the built-in components in `toura_app/javascript/toura/components`.

* `this.connect(nodeOrObject, event, handlerFn)`: Handle events inside a component. Works almost exactly like dojo.connect, except the context is always the instance of the component. The `handler` argument may be either a function or a string; if it is a string, it should refer to a method of the component.
* `this.subscribe(topic, handler)`: Make the component subscribe to pub/sub topics. Works almost exactly like dojo.connect, except the context is always the instance of the component.
* `this.addClass(className)`, `this.removeClass(className)`: Add or remove a class from the top level container of the component. A good place to use these methods might be in the `adjustMarkup` lifecycle method.
* `this.show()`, `this.hide()`, `this.toggle()`: By default, these methods toggle a `hidden` class on the component’s root DOM node; this class by default will set `display: none`
* `this.enable()`, `this.disable()`: By default, these methods simply toggle a `disabled` class on the component’s root DOM node. You can attach styling to that class as needed; you can also connect to or override these methods to add your own functionality around enabling and disabling your components.
* `this.populateElement(node, templateFn, data)`: Takes as arguments a DOM node, a function that receives a data object and returns a string, and some data. Populates the template with the specified data, and populates the node with that template. Note that `mulberry.haml` will take a Haml template and return an appropriate template function.
* `this.populate(templateFn, data)`: A convenience method which simply calls `this.populateElement` on the component's DOM node.
* `this.adopt(constructorFn, properties, node)`: A safe way to add a child component to a parent component without risk of memory leaks. The first argument is the constructor function for the component (e.g. `toura.components.BodyText`); the second argument is an object that will be passed to the constructor on instantiation; the third (optional) argument is the node that the component should replace. This method returns the component instance. For more information on `this.adopt` and its companion, `this.orphan`, see [this blog post](http://higginsforpresident.net/2010/01/widgets-within-widgets/). 
* `this.orphan`: If you have instantiated a component via `this.adopt`, `this.orphan` is the way to destroy it when you are finished with it.
* `this.inherited`: When overriding a method from a superclass, invoking `this.inherited(arguments)` within the subclass method will call the superclass method that is being overridden and pass the arguments through. `this.inherited` is similar to `super` in other object-oriented languages.

You can use these instance methods as appropriate throughout your custom component definition.

### Connecting to Events

While Mulberry provides access to jQuery API methods for convenience, **you should not
use jQuery for connecting to events unless you have a plan for managing the
teardown of those events**. Instead, you should use the
component's built-in `connect` method, which will automatically manage event
teardown.

    this.connect(this.refreshButton, 'click', '_onRefresh');

The above code registers an event listener on the DOM node referenced by
`this.refreshButton`. When the node is clicked, the component's `_onRefresh`
method will be called. Note that you must define the `_onRefresh` method.

### Providing Setter Methods

If you want other parts of your application to be able to change the data for
your component, you need to define setter methods on your component. For
example, if you wanted to allow other pieces of your app to change the username
that the Twitter component is using, you could provide a method
`_setUsernameAttr` in your component definition:

    _setUsernameAttr : function(username) {
      $.ajax(userTweets({ username : username, count : 20 }), {
        dataType : 'jsonp',
        success : $.proxy(this, '_onLoad')
      })
    }

Then, any piece of your application that has a reference to an instance of the
Twitter component can set the username for that instance:

    myTwitterInstance.set('username', 'rmurphey');

### Announcing Interaction

When a user interacts with a component, you may want to announce that
interaction so other pieces of the application can react.

For announcements that are of interest to other components on a page, you
should use custom event methods. For example, if you wanted to announce when a
user clicked on the Twitter refresh button, you could do the following in your
component's `init` method:

    this.connect(this.refreshButton, 'click', '_onRefresh');

Then, you would define `_onRefresh` and `onRefresh` methods for your component:

    // ...

    _onRefresh : function() {
      // ... code to refresh the component ...
      this.refreshTimes = (this.refreshTimes || 0)++;
      this.onRefresh(this.refreshTimes);
    },

    onRefresh : function(refreshTimes) {
      // stub for connection
    },

    // ...

Now, you can define a capability that listens to the component's `onRefresh`
custom event method. For details, see the docs for Capabilities.

For announcements that are interesting beyond the page you're currently on --
for instance, announcements that may alter the overall application behavior --
you can publish your announcement:

    _onRefresh : function() {
      // ... code to refresh the component ...
      this.refreshTimes = (this.refreshTimes || 0)++;
      dojo.publish('/mycomponent/refresh', [ this.refreshTimes ]);
    }

Then, other pieces of your application can subscribe to these announcements:

    dojo.subscribe('/mycomponent/refresh', function(refreshTimes) {
      // ...
    });
