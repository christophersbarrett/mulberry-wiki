With the exception of `mulberry scaffold <name>`, Mulberry commands must be run from inside a directory that contains a Mulberry project. 

**The location of the `cli/bin/mulberry` file must be in your $PATH in order for any of these commands to work!**

You can run `mulberry -v` from anywhere to find out which version of Mulberry you are currently running.

## Creating apps

### mulberry scaffold \<name\>
Create the skeleton of an app in a directory with the provided name. The provided directory should not exist prior to running this command.

### mulberry generate
This command does two things:

- Creates any pages that are defined in the `sitemap.yml` file but that do not exist in your project's `pages` directory.
- Creates any custom components that are used in your templates, but that do not exist in your project's `javascript/components` directory.

### mulberry create page \<name\>
Create a file at `pages/<pagename>.md` that contains the basic structure for defining a page.

## Asset creation

The following commands create assets that can be associated with pages in your app. You can create other asset types, such as images, videos, and audios, by adding them to the proper directories in the `assets` directory. 

**Note that an asset must be associated with a page in order to be available in your app.**

### mulberry create data \<name\>
Create a file at `assets/data/<name>.yml`, which you can populate with custom data to be used by your app.

### mulberry create feed \<name\>
Create a file at `assets/feeds/<name>.yml`, which you can populate with information about an RSS feed to be included in your app.

### mulberry create location \<name\>
Create a file at `assets/locations/<name>.yml`, which you can populate with information about a geographic location to be included in your app.

## Code creation

The following commands create code that can be used as the basis for adding custom functionality to your app.

### mulberry create page_def \<name\>
Create a file at `page_defs/<name>.yml`, which you can populate with information about the layout, components, and capabilities to be used on a page. [Read more about page definitions](https://github.com/Toura/mulberry/wiki/Page-Definitions)

### mulberry create component \<name\>
Create a file at `javascript/components/<name>.js`, along with supporting Haml and CSS files for the component. [Read more about components](https://github.com/Toura/mulberry/wiki/Components)

### mulberry create capability \<name\>
Create a file at `javascript/capabilities/<name>.js`, which you can populate with information about how components should interact on a given page. [Read more about capabilities](https://github.com/Toura/mulberry/wiki/Capabilities).

### mulberry create datasource \<name\>
Create a file at `javascript/data/<name>.js`, which you can populate with information about a remote datasource. **This option is under development.**

## Testing apps

### mulberry serve
Serves the app at http://localhost:3001. The port can be changed by providing an alternate port via `-p`: `mulberry serve -p 3002`. When the app is being served, you can access it via any browser, including a browser on a device connected to the same network as the computer running the server.

### mulberry test
Creates development builds for all devices specified in a project’s config.yml. These builds are suitable for running on a simulator or on a (properly provisioned) device. These builds will include the debugging toolbar, and will also include the built but uncompressed JavaScript, which can be used by editing a build’s index.html file.

## Deploying apps

### mulberry deploy
Creates submission-ready builds for all devices specified in a project’s config.yml. These builds do not include the debugging toolbar or the uncompressed JavaScript. 