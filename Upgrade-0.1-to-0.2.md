How to upgrade from 0.1 to 0.2:

* The template definition yaml format [changed](https://github.com/Toura/mulberry/pull/91). Any custom templates should be updated:
    * regions no longer use `containerType` property
    * regions now have a `type` property which can have the value of `row`, `column`, or `component` 
* The `locations-map` template is now named `google-map-phone` and `google-map-tablet` as appropriate.