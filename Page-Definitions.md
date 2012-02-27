Each page in a Mulberry application is displayed using a **Page Definition** (or as used in the code: **page_def**) which defines what components will be used to render the page and how they will interact. Mulberry comes with a set of built in page_defs that you can
use, or you can create your own. To specify a page_def for a page, add a
`page_def` property to the page's YAML header:

    ---
    title: My Page
    page_def:
      phone: default
      tablet: default
    ---

    My page's content is amazing.

The page_def name is added as a class on the page's root element as a hook for
styling. In this case we're using the `default` page_def for both phone and
tablet. For really simple pages this might be ok, but most of the time you'll
probably want your tablet pages to have a different layout. In that case, you'd
specify different page_defs for each device type:

    ---
    title: My Image Page
    page_def:
      phone: image-gallery-phone
      tablet: image-gallery-tablet
    ---

    My images are amazing.

If you specify a page_def that does not exist, the page will not be
displayed in your app. If you do not specify a page_def, then the built-in
`default` page_def will be used.

## Creating Your Own page_defs

You can specify your own page_defs using a simple YAML structure. Your
page_defs can use Mulberry's built-in components and capabilities, or you can create
your own custom components and capabilities.

Mulberry can create the YAML skeleton for a new page_def for you:

    mulberry create page_def my-pagedef

This will create a file `my-pagedef.yml` in your project's page_defs
directory and `my-pagedef.scss` in your project's `themes/<my theme>/page_defs` directory.

A page_def consists of one or more **screens**; each screen contains one or
more **regions** which will usually be styled as either columns or rows; and
each region contains one or more components. To make a region scrollable, set
`scrollable: true`. Regions can also contain other regions, allowing for
complex layouts. page_defs can also specify interactions between components via
**capabilities**.

This is best explained with an example:

    my-pagedef:
      screens:
        - name: index
          regions:
            - className: page-nav
              components:
                - PageNav
            - scrollable: true
              components:
                - PageHeaderImage
                - BodyText

Each page_def has it's own scss file in the `themes/<theme name>/page_defs` directory where you handle the page layout. Mulberry provides a few helpful Sass mixins to handle common row/column layouts:

    .page.my-pagedef {
      .screen.index {
        @include flex-row-container;

        .region.page-nav {
          @include fixed-flex-region;
        }
      }
    }

Layouts are more fully explained in [[Styles and Theming]].

## Built-In page_defs

Mulberry comes with several built-in page_defs. Any of these page_defs can be
specified as the page_def for a page in the page file's YAML header.

### audio-with-images-phone and audio-with-images-tablet

Displays audios associated with a page; selecting an audio plays
the audio in an audio player. If an audio has an associated caption, it will
be displayed when the audio is selected. Also displays the page text and
links to subpages, if any.

### default

Displays the page text, a page header image (if specified), and
links to subpages, if any.

### grid-view

Displays the page text and a grid of links to subpages; a featured image
should be specified in order to be displayed in the grid.

### home

Displays the page text and links to subpages. Also includes an
application-level navigation component, which links to the Search, About, and
Favorites pages.

### images-and-text-phone and images-and-text-tablet

Displays the images associated with a page in a swipeable image gallery;
selecting an image in the image gallery opens a larger view of the image,
which can be panned and zoomed. If an image has a caption, it will be
displayed when the associated image is displayed. This page_def also displays
the page text and links to subpages, if any.

### full-screen-images

Displays the images associated with a page in a full-screen view. If an image
has a caption, it will be displayed when the associated image is displayed.

### locations-map

Displays the locations associated with a page on a map.

### location-list

Displays the locations associated with a page in a list. If
the page also has images associated with it, they will be displayed in a
gallery above the list of locations; selecting an image in the gallery will
display the image in a full-screen view.

### videos

Displays videos associated with a page; selecting a video from the
list of videos plays the video in a video player. If a video has an
associated caption, the caption will be displayed when the audio is selected.
Also displays the page text and links to subpages, if any.

## Multiple screens

A page_def may contain multiple **screens** that each provide a different layout
of regions.  Since these screens are contained within the same page_def they may
have tightly integrated behavior between their components.

    product:
      screens:
        - name: show
          regions:
            - scrollable: true
              components:
                - ProductImage
                - ProductDetails
        - name: edit
          regions:
            - components:
                - ProductEditor

In this example, there is a `show` product screen that displays a given product.
There is also an `edit` product screen that allows the user to edit the same
product.  It might be nice to automatically update the show screen to reflect
any changes made on the edit screen.  Since these screens are a part of the same
page def, they may be linked together using a **capability**.  (Please see the
[[Capabilities]] page for details.)

To switch between multiple screens, call a page's showScreen() method in a
capability as follows:

    dojo.provide('client.capabilities.Products');

    mulberry.capability('Products', {
      requirements : {
      },

      connects : [
      ],

      init : function() {
        this.page.showScreen('index');
      }
    });

The show screen method will show the given screen and hide all others.  Note
that if this method is never called then all screens will be shown
simultaneously, layered on top of one another.

TODOC: subregions

TODOC: capabilities

TODOC: region properties (scrollable, size, etc.)

