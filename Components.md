Pages in Mulberry are displayed using a combination of components as specified by
the page's page definition. Each component receives information about the page that is
being displayed, as well as information about where on the page the component
will be displayed.

Components are responsible for receiving data to be displayed; displaying the
data using a template; and observing and announcing user interaction. All
built-in and custom components follow the same basic path:

- Receive information about the page and store that information in the
  component instance's `baseObj` property.
- Use the `baseObj` and other data to prepare the data to be used by the
  component's template.
- Create a DOM structure by populating the template with the prepared data.
- Register event listeners that call methods of the component when the event
  occurs.
- Expose an API for updating the component's data, if required.

## Built-In Components

Mulberry comes with several built-in components that you can use in your own
page definitions; they are located at `toura_app/javascript/toura/components/`. The
built-in components are described below, including the asset type(s) that the
component works with. *Asset types* appear in italics. For more about the
different asset types, see the Assets documentation.

### AppNav

Displays application-level navigation elements, including links to the built-in
Search, About, and Favorites pages.

### AssetList

Displays a list of assets, and announces when a user selects an asset from the
list.

### AudioCaption

Displays a caption for an audio. Exposes a setter that allows changing the
content of the caption. (Based on BodyText.)

### AudioList

An audio-specific AssetList.

### AudioPlayer

A wrapper around an HTML5 audio player -- with fallbacks for browsers that do
not support the HTML5 `audio` tag -- for displaying an *audio*. Exposes a
setter that allows changing the current audio source file. Announces user
interaction with the player.

### BodyText

Displays the *body text* associated with a page, and exposes a setter that
allows the content to be updated.

### ChildNodeGrid

Displays the *subpages* of the page in a grid layout; each item in the grid
links to the corresponding page. (Subpages must have a `featured_image`
property specified in order to be displayed in the grid.)

### ChildNodes

Displays the *subpages* of the page in a list layout; each item in the list
links to the corresponding page.

### ColumnHeaderImage

Displays the page's *header* image inside a column region (see the Page Definitions
documentation for more information on page definitions and regions).

### DetailTitle

Displays a title region on a detail screen (see the Page Definitions
documentation for more information on page definitions and screens), and exposes a
setter for setting the title. Announces user interaction with the close button.

### FeedItemDetail

Displays the contents of an RSS *feed* item, including an image enclosure if one
is included in the feed item.

### FeedItemList

Displays a list of RSS items from a *feed*. Announces a user's selection from
the list of items.

### GoogleMap

Displays a set of *locations* on a Google Map, and displays information about each
location when the user selects a location.

### ImageCaption

Displays a caption for an image. Exposes a setter that allows changing the
content of the caption. (Based on BodyText.)

### ImageDetail

Displays a set of *images* in a full-screen gallery; each image can be panned and zoomed.
Announces user interaction with the gallery.

### ImageGallery

Displays a set of *images* in a swipeable gallery. Announces user interaction
with the gallery.

### LocationList

Displays a set of *locations*.

### NodeTitleBanner

Displays the page title.

### PageHeaderImage

Displays the page's *header* image across the width of the page.

### PageNav

Displays the page's title, as well as various page and application tools.

### VideoCaption

Displays a caption for a video. Exposes a setter that allows changing the
content of the caption. (Based on BodyText.)

### VideoPlayer

A wrapper around an HTML5 video player -- with fallbacks for browsers that do
not support the HTML5 `video` tag -- for displaying a *video*. Exposes a
setter that allows changing the current video source file. Announces user
interaction with the player.

## Creating Custom Components
Read about [[Creating Custom Components]]