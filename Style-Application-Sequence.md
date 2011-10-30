Mulberry defines the user interface using a few different containers. Each page can contain one or more screens. Screens contain either rows or columns, which in turn contain either more rows or columns, or ultimately components, which is where all the interesting stuff happens.

Mulberry does most of the grunt work of defining a mobile layout, so you can focus on more interesting things. It does this by applying styles in a few successive steps:

1. Reset: This resets most elements to a styleless baseline and applies basic styles we need to make a web page feel like a mobile app.
1. Container Framework: This is where we have some styles for defining rows, columns, and screens as well as handle things like page transitions. For the most part, you probably don't need worry about this too much. '
1. Component base styles: These are the bare minimum styles the component needs to display correctly. No more, no less.
1. Theme: This is where you apply your styles. There is currently one default theme. A theme defines two different types of styles.
  1. Page templates: This is where general page styles should be applied.
  1. Components: This is where most of the interesting styles are applied. Components follow a simple convention which allows you to easily scope styles safely and avoid difficult cascade bugs.
