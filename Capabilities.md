Capabilities describe how two or more components that are present on a page should interact with each other. Capabilities have names, which allows them to be referenced in a plain-text template configuration, and also allows them to be reused in multiple template configurations.

Here is an example of a capability that defines the interaction between an Image Gallery and an Image Caption component:

    "ImageGallery_ImageCaption" : {
      requirements : {
        imageGallery : 'ImageGallery',
        imageCaption : 'ImageCaption'
      },

      connects : [
        [ 'imageGallery', 'onScrollEnd', '_setCaption' ]
      ],

      _setCaption : function(imageIndex) {
        var image = this.node.images[imageIndex];
        this.imageCaption.set('content', image && image.caption || '');
      }
    }
    
A capability indicates the components that it requires, and then indicates the function that should run when a component announces an event. In the example above, the capability indicates that it requires an ImageGallery and ImageCaption component, and that the `_setCaption` method defined in the capability's configuration should run when the `onScrollEnd` method of the Image Gallery instance is called.

To add a capability to a custom page template, specify the capability's name
and the involved component(s) in the template definition. Note that each
involved component should be prefixed by the name of the screen on which it
appears -- this allows the same component to be used on multiple screens
without confusion.

    ImageGallery
      screens:
        -
          name: index
          regions:
            -
              size: fixed
              scrollable: false
              components:
                - PageNav
            -
              size: flex
              scrollable: true
              components:
                - ImageGallery
                - ImageCaption
        capabilities:
          -
            name: ImageGallery_ImageCaption
            components:
              - index:ImageGallery
              - index:ImageCaption