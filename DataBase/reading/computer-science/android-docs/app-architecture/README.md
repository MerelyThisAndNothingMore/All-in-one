# App Architecture

As android apps grow in size, it's important to define an architecture that allows the app to scale, increases the app's robustness, and makes the app easier to test.

## Common Architectural Principles

### Separation of concerns

It's a common mistake to write all your code in an Activity or a Fragment. These UI-based classes should only contain logic that handles UI and operating system interactions.

### Drive UI from data models

### Single source of truth

The SSOT is the owner of the data, and only the SSOT can modify or mutate it.

### Unidirectional Data Flow

In UDF, state flows in only one direction.

## Recommended app architecture

<img src="../../../../.gitbook/assets/image (3).png" alt="" data-size="original">

A typical architecture has three layers.&#x20;

### UI Layer

#### UI layer architecture

![](<../../../../.gitbook/assets/image (6).png>)

The term UI refers to UI elements such as activities and fragments that display the data, independent of what APIs they use to do this(Views or Jetpack Compose).

The UI state is an immutable snapshot of the details needed for the UI to render.

![](<../../../../.gitbook/assets/image (1).png>)

![](<../../../../.gitbook/assets/image (2).png>)

####

## Manage dependencies between components

Classes in your app depend on other classes in order to function properly. There are two popular design patterns to gather the dependencies of a class:

* Dependency Injection (DI)
* Service Locator

They both allow dependencies definition without object construction.

