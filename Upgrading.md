from 0.1 to 0.2:

* The template definition yaml format [changed](https://github.com/Toura/mulberry/pull/91). Any custom templates should be updated:
    * regions no longer use `containerType` property
    * regions now have a `type` property which can have the value of `row`, `column`, or `component` 
* The default theme is now copied into a scaffolded app, so old apps don't have a copy of it. The main `custom.scss` is also now called `base.scss`. (**Warning:** the following may overwrite some of your files. Make sure you're using version control.) To get the default theme, do `mulberry update_themes`
* The `locations-map` template is now named `google-map-phone` and `google-map-tablet` as appropriate.
* The pattern for the command line commands has changed; see [[Command Line Interface]]

from 0.2 to 0.3:
* Theming changes: To get the changes, run `mulberry update_themes` and resolve any conflicts manually 
  * Icons have been moved to `themes/default/resources/icons`. 
  * All colors have been extracted to `themes/default/_settings.scss`
  * Instead of defining layout in page_def yaml files, use mixins in page_def scss files
    * Any references to the following sass mixins should be changed:
      * `column-container` -> `flex-column-container`
      * `row-container` -> `flex-row-container`
    * For any custom templates you've created:
      * create an scss file in `themes/<my theme>/pagedefs`
      * remove all `type:<row, column>` parameters from the yaml. Apply `flex-row-container` and `flex-column-container` to their parent region in scss instead
      * remove all `layout: size-fixed` from the yaml. Apply `fixed-flex-region` to the element in the scss instead


