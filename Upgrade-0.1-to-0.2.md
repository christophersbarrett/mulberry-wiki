How to upgrade from 0.1 to 0.2:

* The template definition yaml format [changed](https://github.com/Toura/mulberry/pull/91). Any custom templates should be updated:
    * regions no longer use `containerType` property
    * regions now have a `type` property which can have the value of `row`, `column`, or `component` 
* The default theme is now copied into a scaffolded app, so old apps don't have a copy of it. The main `custom.scss` is also now called `base.scss`. (**Warning:** the following may overwrite some of your files. Make sure you're using version control.) To get the default theme, do `mulberry update_themes`
* The `locations-map` template is now named `google-map-phone` and `google-map-tablet` as appropriate.
* The pattern for the command line commands has changed; see [[Command Line Interface]]