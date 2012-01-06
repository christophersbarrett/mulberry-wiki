The Mulberry toolkit is made up of three distinct pieces of functionality: the Mulberry Application Framework; the Mulberry Command Line Interface; and the Mulberry Builder. Together, these three pieces help developers create, customize, and test applications using familiar web technologies, then deploy those applications as native mobile experiences.

## The Mulberry Application Framework

Location: app/toura/

The Mulberry Application Framework provides the underlying architecture for building applications that will work without the benefit of a remote server to serve up each page in the application — these types of applications are often referred to as “single-page applications.” 

### Creating the User Interface

In the framework, each page displays a unit of content; we refer to a unit of content as a “node.” Nodes are displayed using page templates, which specify the layout of the page, the components to be displayed on the page, and how the components should interact with the page and with each other. These interactions are described via “capabilities.”

Template definitions are authored in YAML, which is compiled to JSON that can be parsed by the framework. When the framework displays a page, it combines the data about the node that is being displayed with the template definition that is associated with the node, and creates a page controller. The page controller is responsible for setting up the layout of the page, placing the components in the layout, providing the components with the data from the node, and brokering communication between the components.

Each component encapsulates a unit of functionality, and components can be reused across multiple page templates. When a component receives data to display, it manipulates that data as required, and then uses the data to populate a template. The template is the DOM representation of the component. Once the DOM representation is created, a component can add event listeners to react to user interaction. A component can also expose an API of “setter” methods, allowing the page controller to send it new data as required. Ideally, components simply receive data and announce user interaction; they should not communicate directly with a remote resource, or with the data layer of the application. 

The Mulberry application framework comes with a selection of built-in templates, components, and capabilities, but it also allows developers to author their own. Developers can create unique experiences by following clear patterns and using simple APIs, all without worrying about the underlying “plumbing” of their application. 


### Creating Custom Functionality

In addition to providing clear patterns for creating and connecting user interface components, the Mulberry application framework also provides tools and features that facilitate more complex application development:

- a Router that can be used to specify custom state management functions
- APIs for persisting data locally, so applications will work without a network connection
- over-the-air updates, allowing applications to occasionally poll a remote server for new data, and store any new data that is found so it will be available for offline use
- structured, modular markup that allows for easy design customization


## The Mulberry Command Line Interface (CLI)

Location: mulberry/bin/mulberry

The CLI is a set of helpers that makes it easy to scaffold, develop, and test an application. Specifically, it provides the ability to 

- rapidly spin up a new application
- create nodes and their associated resources and data
- create skeleton files for page templates, components, and capabilities
- serve the application locally for rapid in-browser iteration
- create test builds that can run on simulators and (properly provisioned) devices
- create production builds that can be submitted to app stores

## The Mulberry Builder

Location: lib/builder.rb

The Mulberry Builder takes content and customizations from a given “content source,” and combines it with the Mulberry Application Framework to create a functional application for a specified target environment (referred to as the “build target”). 

A Mulberry project — as created by the Mulberry CLI — is one example of a content source, but the builder can consume any content source that exposes an interface via a Build Helper. A Build Helper must provide an interface as defined by the base Build::BuildHelper class (lib/builder/build_helper.rb). This interface allows the Builder to interrogate the Build Helper in order to access the content and customizations provided by the content source.

Separately, the Builder must be provided with information about the target environment for which it is building the application. Based on that information, the builder creates a project directory using a project template (lib/builder/project_templates/), and then populates the project directory with the Mulberry Application Framework files and the content and customizations from the content source. 

The output of the Builder is a project directory that can be compiled if necessary, and then executed in the target environment. 