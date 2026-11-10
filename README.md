# Navigator
LabVIEW implementation of an app navigator.

## Quickstart
An example has been included showing a basic `Viewport` that contains a Sub Panel, and 2 `NavigatableViews` pages.

Within the main application, the Navigator should be initialised with [Routes](#routes), the Sub Panel reference that will be used by the Navigator to display `Views`, and an [InitialRoute](#initialroute) can be provided. The `Dispose` method should be called when the app is shutdown.

![Main Viewport VI LabVIEW block diagram](Images\image.png)

The [Routes](#routes) must be defined on initialisation.

![Example Route Definition VI LabVIEW block diagram](Images\image-1.png)

Each `NavigatableView` should inherit from the base [NavigatableView](#navigatableview) class. The [Launch](#launch) VI must be overridden to provide the VIReference, and to inject the NavigationEvents for use within the `View`.

![NavigatableView Launcher VI LabVIEW block diagram](Images\image-5.png)

The stop event must e registered in the `View` event handler and must terminate the event handling loop. The [NavigatorEvents](#navigatorevents) can be used within the `View` to navigate to different pages within the app.

![NavigatableView UI VI LabVIEW block diagram](Images\image-3.png)

## Routes
Routes are defined on initialisation. They consist of a [Name](#name), [Path](#path), and [Builder](#builder).

### Name
This is an alias given to the route which is used rather than defining the path every time navigation occurs.

### Path
This is the path to the [NavigatableView](#navigatableview) UI that will be displayed in the Sub Panel.

### Builder
This is the initialised [NavigatableView](#navigatableview).

### InitialRoute
The initial route the Sub Panel should display is given on initialisation.

## Stack
The Stack is an ordered array of [Routes](#routes).

## NavigatorEvents

### Go
When a route is navigated to using `Go`, the Stack is cleared, and the route is added to the empty stack. The app navigates to this page, and since the Stack has been cleared, [Pop](#pop) will no longer navigate to the previous page.

### Push
When a route is navigated to using `Push`, the route is added to the existing stack. The app navigates to this page, and since the Stack has not been cleared, [Pop](#pop) will navigate to the previous page.

### Pop
When a route is navigated to using `Pop`, the last route on the Stack is removed. The app navigates to the previous page.

## NavigatableView
This is an abstract class that allows a `View` (page) to be navigatable using the [NavigatorEvents](#navigatorevents).

### Launch
The [NavigatorEvents](#navigatorevents) are input, allowing the `View` to generate events to trigger navigation. The UI should then be launched, and the VI reference output. This will be displayed in the Sub Panel.

### GetStopEvent
The `View` UI must register its stop event to allow the Navigator to stop the current `View` when it navigates to another page.

### Shutdown
This can be overridden to define any shutdown actions required.