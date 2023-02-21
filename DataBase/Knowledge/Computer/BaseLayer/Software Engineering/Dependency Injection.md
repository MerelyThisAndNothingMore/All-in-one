#coding
# Overview
In [[Software engineering]], dependency injection is a [[Design pattern]] in which an [[Object]] or [[Function]] receives other objects or functions that it depends on. The pattern ensures that an object or function which wants to use a given service should not have to know how to construct those services.
Classes often require references to other classes. For example,  a `Car` class might need a reference to an `Engine` class. These required classes are called dependencies, and in this example the `Car` class is dependent on having an instance of the `Engine` class to run.
Without dependency injection, representing a `Car` that creates its own `Engine` dependency in code looks like this:
```kotlin
class Car {
	private val engine = Engine()

	fun start() {
		engine.start()
	}
}

fun main(args: Array) {
	val car = Car()
	car.start()
}
```
And with dependency injection, the code looks like this:
```kotlin
class Car(private val engine: Engine) {
	fun start() {
		engine.start()
	}
}

fun main(args: Array) {
	val engine = Engine()
	val car = Car(engine)
	car.start()
}
```

