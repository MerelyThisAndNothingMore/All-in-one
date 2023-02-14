# Guide to app architecture

## UI layer

The UI is a visual representation of the application state as retrieved from the data layer.The UI layer is the pipeline that converts application data changes to a form that the UI can present and then displays it.

![](<../../../../.gitbook/assets/image (7).png>)

* The ViewModel holds and exposes the state to be consumed by the UI. The UI state is application data transformed by the ViewModel
* The ViewModel handles the user actions and updates the state.

### UI events

UI events are actions that should be handled in the UI layer.

## Domain layer

## Data layer

##
