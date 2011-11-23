# Public JavaScript APIs

The Mulberry JavaScript framework provides a number of useful methods that you can use when creating a custom application using the Mulberry toolkit. 

Note that the `mulberry` “namespace” is just an alias to the `toura` namespace; all `mulberry.*` methods can be accessed as `toura.*` as well, and they are generally defined in the framework code as `toura.*`.

## Creating Custom Components, Capabilities, and Data Sources

- `mulberry.component(name, prototype)`
- `mulberry.capability(name, prototype)`
- `mulberry.datasource(name, prototype)`

## Routing

- `mulberry.route(route, handler, isDefault)`
- `mulberry.routes(routesArray)`
- `mulberry.app.Router.go(hash)`
- `mulberry.app.Router.home()`
- `mulberry.app.Router.back()`

## Page Creation

- `mulberry.app.PageFactory.createPage(pageData)`

## User Interface

- `mulberry.app.UI.set(prop, val)`
- `mulberry.app.UI.showPage(page)`
- `mulberry.app.UI.viewport`
- `mulberry.app.UI.currentPage`

## PhoneGap/Device

- `mulberry.app.PhoneGap.network`
  - isReachable()
- `mulberry.app.PhoneGap.notification`
  - alert()
- `mulberry.app.PhoneGap.browser`
  - url(url)
  - getBrowser()

## Config

- `mulberry.app.Config.get(key)`
- `mulberry.app.Config.set(key, value)`

## Device Storage

- `mulberry.app.DeviceStorage.get(key)`
- `mulberry.app.DeviceStorage.set(key, value)`
- `mulberry.app.DeviceStorage.drop()`

## Utilities

- `mulberry.tmpl(string, dataObject)`
- `mulberry.haml(hamlTemplateString)`

# Useful pub/sub topics

## Application state

- `/app/deviceready`
- `/app/ready`
- `/app/started`
- `/router/handleHash/after`
- `/node/view`

## User Interface

- `/window/resize`
- `/button/menu`