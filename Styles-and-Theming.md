Mulberry uses [Sass](http://sass-lang.com/) for CSS, which gives us a lot of tools for keeping our code DRY, but since we use the **scss** syntax, you can write all your code in plain CSS if that’s what you’re more comfortable with.

To begin styling your app, you need to define its theme. When you scaffold an app, by default it will get the -- you guessed it -- **default** theme. This is an ok starting point but not that exciting, so you’ll want to either tweak it by overriding some of the existing styles, or start from scratch and create a new theme. Either way, you do all your work in the themes folder. 

## Tweaking an existing theme
You’ll do this by overriding some styles in the default theme. Start by looking in the themes directory You should see this:

    themes
    - default
      - custom.scss 

After mulberry has applied all the framework styles and the default theme, it will load
custom.scss, which is where you can make any changes you want. If you want to make any drastic changes, you’ll be better off creating a new theme from scratch, otherwise, you’ll need to use a lot of !important declarations, which can be error-prone.

To create a new theme, simply 
	create a new directory in themes
	create a custom.scss file in that directory
	change the theme name in _config.yml to your theme’s directory
	add your styles or import directives to custom.scss 

You can probably figure out how to style what you want by looking through the Web Inspector, but there are a few things you might want to know about how Mulberry markup that will be helpful.

## Pages, Screens, Regions and Components

These are the building blocks of a Mulberry app. You’ve already seen references to them in the template definitions, but here’s what it means for styling. Mulberry takes a template definition and constructs consistent markup with helpful hooks for styling whichever part of your app you need to. For instance the page template definition classname gets added to the page’s root element, so if you want to apply a style to pages with that template, it’s no problem:
--
.page.my-template {
	color: green;
}
--

Pages contain screens, which also contain their name as a class: 
--
.page.my-template {
	color: MidnightBlue;
	
	.screen.index {
		background-color: LightGoldenrodYellow;
	}
}
--

And lastly there are regions (columns and rows), which can be selected either based on their name (optional in the template definition) or based on its position in the template.
--
.page.my-template {
	color: green;
	
	.screen.index {
		background-color: orange;
		
		.row.header {
			height: 60px;
		}

		> .row.nth-child(2) {
			> .column.first-child {
				width: 200px;
			}
		}
	}
}
--

Components
You probably don’t want to put too many styles for your components within the page template styles, because you want the component styles to be reused no matter what page it’s placed in. So component styles are best handled separately, although the conventions are similar. Say we want to style the ImageGallery component. 
--
.component.image-gallery {
	background-color: LemonChiffon;
}
--


Hiding and Showing Components and Elements 
One of the most common UI style actions is simply hiding and showing something depending on user input. By default every component and ever element within a component has a .hidden class which specifies how you want that element to be hidden when the _Component::hide method is called. By default it’s simply display:none. But by overriding the hidden class you can use CSS transforms to give things a little more flair.
--
.component.my-component {
	.something-cool {

	opacity: 1;
	-webkit-transition: opacity .3s ease-in;
	
	&.hidden {
      		opacity: 0;
      		display: block !important;
	}

}
--

Now, whenever the something-cool element gets hidden, it will fade out nicely. When it gets unhidden, it fades in nicely. The even nicer thing here is that we have added this without touching any application code. We can can keep presentation logic out of Javascript and in CSS.

