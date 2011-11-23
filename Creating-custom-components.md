Mulberry exposes a simple API for creating custom components that you can use on
your own page templates. This section explains how you might create a simple
component for displaying a Twitter feed.

To start, run the following command from your app directory:

    mulberry create component Twitter

This will:

- create a stub component file at `javascript/components/Twitter.js`
- create a component resources directory at `javascript/components/Twitter/`
- create a component template file at
  `javascript/components/Twitter/Twitter.haml`
- add the appropriate `dojo.require` statement to your project's base
  JavaScript file, located at `javascript/base.js`
- create a SCSS file at `javascript/components/Twitter/_twitter.scss`
- add the appropriate `@import` statement to your project's base SCSS file.

To start with, edit the component's template file. This file uses
[Haml](http://haml-lang.com/) to generate markup from JavaScript. Your Twitter
component can have a simple template:

    %ul.component.twitter

Once you save this file, the component is usable in a page template, but it
won't do much -- it will put the empty unordered list in the page, but nothing
more.  Next, we need to tell the component how to behave by editing the file at
`javascript/components/Twitter.js`.

The component will need to know the username for which it should fetch tweets;
in order for this component to be reusable, we don't want to hard-code that
information into the component. Instead, we can use data that we attached to
the page:

    dojo.provide('client.components.Twitter');

    (function() {
      var userTweets = function(settings) {
            return toura.tmpl(
              'http://twitter.com/status/user_timeline/' +
              '${username}.json?count=${count}',
              settings
            );
          };

      mulberry.component('Twitter', {
        componentTemplate : dojo.cache('client.components', 'Twitter/Twitter.haml'),

        init : function() {
          var data = this.node.data[0].json;

          $.ajax(userTweets(data), {
            dataType : 'jsonp',
            success : $.proxy(this, '_onLoad')
          });
        },

        _onLoad : function(tweets) {

        }
      });

    }());

(See the Assets documentation for information on attaching data to a page.)

In the above code, we've added a "private" function, `userTweets`, that takes a
settings object. The `toura.tmpl` method uses the settings object's `username`
and `count` properties to create and return a URL for fetching JSONP data from
Twitter. We use jQuery's `$.ajax` method to load the data, and specify that we
should call the component's `_onLoad` method when the data is returned.

Next, we need to fill out the `_onLoad` method so it will populate our
component with the data:

    dojo.provide('client.components.Twitter');

    (function() {
      var userTweets = function(settings) {
            return toura.tmpl(
              'http://twitter.com/status/user_timeline/' +
              '${username}.json?count=${count}',
              settings
            );
          },

          tweetURL = function(settings) {
            return toura.tmpl(
              'http://twitter.com/capitoljs/status/${id_str}',
              settings
            );
          };

      mulberry.component('Twitter', {
        componentTemplate : dojo.cache('client.components', 'Twitter/Twitter.haml'),

        tweetTemplate : dojo.cache('client.components', 'Twitter/Tweet.haml'),

        init : function() {
          var data = this.node.data[0].json;

          $.ajax(userTweets(data), {
            dataType : 'jsonp',
            success : $.proxy(this, '_onLoad')
          });
        },

        _onLoad : function(tweets) {
          // prep the template
          var tpl = toura.haml(this.tweetTemplate),

              html = $.map(tweets, function(tweet) {
                // prepare the tweet data
                tweet.link = tweetURL(tweet);
                tweet.text = tweet.text.replace(
                  /@(\S+)/,
                  "<a href='http://twitter.com/#!/$1'>@$1</a>"
                );

                // populate the tweet template
                return tpl(tweet);
              }).join('');

          // place the tweets markup in the component
          this.$domNode.html(html);

          // tell the containing region
          // to update its scroller, if required
          this.region.refreshScroller();
        }
      });

    }());

Here, we've done a few things:

- we added another "private" helper function, `tweetURL`
- we added another template, `tweetTemplate`
- we used `toura.haml` to turn the `tweetTemplate` string into a working Haml
  template function, which we then call with the data for each tweet
- we used jQuery to place the markup for all the tweets in the component's root
  DOM node
- we asked the region that contains the component to refresh its scroller,
  because the size of the component has changed since it was rendered

We now have a working Twitter component that we can use in our custom page
templates. To use this component in a page template definition, be sure to
prefix it with `custom:`. So:

    custom:Twitter

### Querying the DOM of Your Component

If your component code needs to refer to a specific element in your component's
DOM structure, you can query that element using jQuery or dojo.query, but a
better solution is to label the element as a `dojoAttachPoint` in the component template.

    %div.component.twitter
      %button{ dojoAttachPoint : 'refreshButton' }
      %ul{ dojoAttachPoint : 'tweetsList' }

These nodes will automatically be available to you as `this.refreshButton` and
`this.tweetsList` in your component code.

### Component Properties

All component instances have a slew of built-in properties that you can take
advantage of.

#### `baseObj`

An object representing the page on which the component is being displayed. This
object contains all of the asset data associated with the page.

#### device

A object containing information about the device environment, with a `type` and
`os` property.

#### domNode

The root DOM node.

#### $domNode

A jQuery object containing the component's root DOM node.

#### region

The region in which the component is displayed.

#### screen

The screen on which the component is displayed.

### Component Properties You Can Provide

#### componentTemplate

A string that can be parsed as Haml that will be used to construct the
component. This can be a plain string, or a string returned by `dojo.cache`
(see the example above). If you do not provide a `componentTemplate` property,
then the default is `%div`.

### Component Lifecycle Methods

Components call many methods during their setup -- see
`toura_app/javascript/toura/components/_Component.js` for details -- but there
are a couple that you need to know about when creating your own components. You
can specify what should happen during these steps by defining a method
for that step in the object you pass to `mulberry.component`.

- `prep`: Prepare the data that your component will need. In this method, you
  can rearrange data that's contained in the component's `baseObj` property,
  which will contain all of the information that Mulberry knows about the page
  that's using the component.
- `init`: Once the component has been placed on the page, you can do any setup
  that's required in this method.

### Connecting to Events

While Mulberry provides access to jQuery API methods for convenience, _you should not
use jQuery for connecting to events unless you have a plan for managing the
teardown of those events_ when moving to a new page. Instead, you should use the
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
      toura.publish('/twitter/refresh', [ this.refreshTimes ]);
    }

Then, other pieces of your application can subscribe to these announcements:

    toura.subscribe('/twitter/refresh', function(refreshTimes) {
      // ...
    });
