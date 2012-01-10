Mulberry uses [Sass](http://sass-lang.com/) for CSS, which gives us a lot of tools for keeping our code DRY, but since we use the **scss** syntax, you can write all your code in plain CSS if that’s what you’re more comfortable with.

To begin styling your app, you need to define its theme. When you scaffold an app, by default it will get the -- you guessed it -- **default** theme. This is an ok starting point but not that exciting, so you’ll want to either tweak it by overriding some of the existing styles, or start from scratch and create a new theme. Either way, you do all your work in the themes folder.

### Tweaking the existing default theme

When you scaffold an app it gives you a complete copy of the default theme in `themes/default`. If you don't want something drastically different, this is a good starting point that you can tweak as needed. The starting point is `base.scss`, where you add your styles or import other files. If you just want to change some colors or fonts, you can modify those in the `_settings.scss` file.

The built in themes may change or be updated with bugfixes in the future, but since you're working on your own copy, you won't get them immediately. To get the latest themes, type

    mulberry update_themes

And it will overwrite any changes you have locally with the latest version. If you're using version control you should be able to merge the updates into your code without a problem.

### Creating a new theme
While the default theme is an OK starting point, you'll probably want to create your own theme. To create a new theme from scratch, simply:

* create a new directory in themes
* create a `base.scss` file in that directory
* change the theme name in `_config.yml` to your theme’s directory
* add your styles or import directives to `base.scss`

You can probably figure out how to style what you want by looking through the Web Inspector, but there are a few things you might want to know about how Mulberry markup that will be helpful.

### Pages, Screens, Regions and Components

These are the building blocks of a Mulberry app. You’ve already seen references to them in the page definitions, but here’s what it means for styling. Mulberry takes a page definition and constructs consistent markup with helpful hooks for styling whichever part of your app you need to. For instance the page definition classname gets added to the page’s root element, so if you want to apply a style to pages with that page definition, it’s no problem:

    .page.my-page-def {
      color: green;
    }

Pages contain screens, which also contain their name as a class:

    .page.my-page-def {
      color: MidnightBlue;

      .screen.index {
        background-color: LightGoldenrodYellow;
      }
    }

And lastly there are regions, which can be selected either based on the className you assign to them in the page definition (optional in the template definition) or based on their position in the page definition.

    .page.my-page-def {
      color: green;

      .screen.index {
        background-color: orange;

        .region.header {
          height: 60px;
        }

        > .region.nth-child(2) {
          > .region:first-child {
            width: 200px;
          }
        }
      }
    }

### Screen Layout

Usually, you'll need to lay out your regions in columns and rows, so Mulberry provides a few Sass mixins to handle this in a reliable way. They use the [flexible box model](http://www.the-haystack.com/2010/01/23/css3-flexbox-part-1/).

  * `flex-row-container`: Defines a region as a flex-box container with child regions as rows.
  * `flex-column-container`: Defines a region as a flex-box container with child regions as columns.
  * `flex-region(<proportion>)`: Used for child elements of `flex-row-container` and `flex-column-container`, but allows you to specify the proportion of space the region should take up.
  * `fixed-flex-region`: Defines fixed size region within a flex-box container.

### Components
You probably don’t want to put too many styles for your components within the page definition styles, because you want the component styles to be reused no matter what page it’s placed in. So component styles are best handled separately, although the conventions are similar. Say we want to style the ImageGallery component.

    .component.image-gallery {
      background-color: LemonChiffon;
    }

### Hiding and Showing Components and Elements
One of the most common UI style actions is simply hiding and showing something depending on user input. By default every component and ever element within a component has a `hidden` class which specifies how you want that element to be hidden when the `_Component::hide` method is called. By default it’s simply `display:none`. But by overriding the `hidden` class you can use CSS transforms to give things a little more flair.

    .component.my-component {
      .something-cool {
        opacity: 1;
        -webkit-transition: opacity .3s ease-in;

        &.hidden {
          opacity: 0;
          display: block !important;
        }
      }
    }

Now, whenever the something-cool element gets hidden, it will fade out nicely. When it gets unhidden, it fades in nicely. The even nicer thing here is that we have added this without touching any application code. We can can keep presentation logic out of Javascript and in CSS.

## Other Topics

* [[Style Application Sequence]]
