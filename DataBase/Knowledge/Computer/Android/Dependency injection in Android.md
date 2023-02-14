There are two major ways to do [[Dependency Injection]] in Android:
- Constructor Injection. You pass the dependencies of a class to its constructor.
- Field Injection, with which dependencies are instantiated after the class is created. The code looks like below:
```kotlin
class Car {
	lateinit var engine: Engine

	fun start() {
		engine.start()
	}
}

fun main(args: Array) {
	val car = Car()
	car.engine = Engine()
	car.start()
}
```