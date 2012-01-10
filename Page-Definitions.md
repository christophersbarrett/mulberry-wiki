Each page in a Mulberry application is displayed using a **page_def **. Keep in
mind that this is different than a **page**. A **page** is a markdown file
which contains your content for the page. A **page_def** defines what Mulberry
will do with that content. There are lots of built in page_defs that you can
use, or you can create your own. To specify a page_def for a page, add a
page_def property to the page's YAML header:

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

Mulberry can create the YAML skeleton for a new page_def (page_def to save you some typing) for you:

    mulberry create page_def my-pagedef

This will create a file `my-pagedef.yml` in your project's page_defs
directory.

A page_def consists of one or more **screens**; each screen contains one or more
**regions** which are defined as either **columns** or **rows**; and each region contains one or more components. Regions can also contain other regions, allowing for complex layouts. page_defs can also specify
interactions between components via **capabilities**.

This is best explained with an example:

    my-pagedef:
      screens:
        - name: index
          regions:
            - type: row
              size: fixed
              components:
                - PageNav
            - type: row
              scrollable: true
              components:
                - PageHeaderImage
                - BodyText

Regions also have a few other options. Set `scrollable: true` to make the region scrollable. The `size` option defines how the region will determine its size. It has two possible values:

* `flex`: [default] the region will take up as much space as it can. Multiple regions within the same container will all take up equal amount of space.
* `fixed`: the region will take the size of its contents -- no more, no less.

If you need more control over the size of your regions (which you probably will), then you can handle that in the Sass for the template, explained in [[Styles and Theming]].

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


TODOC: subregions

TODOC: capabilities

TODOC: multiple screens

TODOC: region properties (scrollable, size, etc.)
