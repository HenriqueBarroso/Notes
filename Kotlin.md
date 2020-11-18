# Kotlin notes

## Introduction

### Hello World

#### Package
Package is optional, be default it goes to the default package if none is specified

```Kotlin
package my.company.namespace
```

#### Main
The entry point to a Kotlin application is the `main` function.  In versions older than 1.3  we needed specify `main` with `Array<String>` parameters. But it's no longer required.

```kotlin
// Versions older than 1.3
fun main(args: Array<String>) {
	...
}

// Version 1.3 or later
fun main() {
	...
}
```

### Functions

#### Local functions
Function can have local functions inside
```kotlin
fun main() {
	fun hello() = "Hello World"
	
	println(hello())
}
```

#### Default parameters values and named arguments
```kotlin
// A simple function that returns Unit (No return value)
// If no return type is specified it will always be Unit.
// we can also call this function with differnt parameter order.
// eg: printMessage(prefix = "ERROR", name = "Henrique")
fun printMessage(name: String, prefix: String = "Info"): Unit {
	println("[$prefix] - $name")
}

fun multiply(x: Int, y: Int): Int {
	return x * Y
}
// This is a one-liner function, that returns an Int type
fun sumValues(x: Int, y: Int) = x + y
```

#### Infix functions
Functions marked with `infix` can be called without the `.` or parentheses in the call.
Some rules are required:

- They must be member or extension functions
- They cannot have a default value and not accept variable number or arguments
- They must have a single parameter 

```Kotlin
fun main() {
	infix fun Int.plus(value: Int) = this + value
	println(1 plus 2)
	
	infix fun String.onto(other: String) = Pair(this, other)  
	val myPair = "McLaren" onto "Lucas"  
	println(myPair)

	val sophia = Person("Sophia")  
	val claudia = Person("Claudia")  
	sophia likes claudia
}

class Person(val name: String) {  
    private val likedPeople = mutableListOf<Person>()  
    infix fun likes(other: Person) { likedPeople.add(other) }  
}
```

#### Operator functions

Some functions can be used as operators.

```Kotlin
operator fun Int.times(str: String) = str.repeat(this)
println(2 * "Bye ") // "Bye Bye"

operator fun String.times(n: Int) = this.repeat(n)  
println("Alright " * 3) // "Alright Alright Alright
```

#### Vararg parameters

When specified, a `vararg` allows to pass a N parameters to a function by separating it by commas. Behind the scenes this is basically an `Array<T>` .

```kotlin
fun companies(vararg companies: String) {  
    for (c in companies) println(c)  
}  
companies("Apple", "Microsoft", "Intel") // Apple Microsoft Intel

fun printWithPrefix(vararg messages: String, prefix: String) {  
    for (m in messages) println("$prefix: $m")  
}  
  
printWithPrefix("henrique", "john", "levio", prefix = "Name")
```

### Null Safety

Kotlin doesn't allow the assignment of `null`. If we need a variable that can be null we need to specify it with `?` symbol.

```kotlin
var neverNull: String = "This can't never be null"
var maybeNull: String? = "Maybe this can be null a some point"
```

#### Working with nulls 

When working with code that maybe be nullable we can do the following:

```kotlin
val maybeString: String? = null  
  
if (maybeString != null && maybeString.isNotEmpty()) {  
    println(maybeString)  
} else {  
    println("This is a nullable string")  
}
```

### Classes

Classes can be created using the `class`  keyword and a name eg: `class Person`. Both header and body are optional.

```kotlin
class Person  
  
class City(val country: String, var name: String)  
  
fun main() {  
  
    val person = Person()  
    val city = City("Portugal", "Lisbon")  

    city.name = "Lisboa"
}
```

### Generics

Generics allows to reuse common logic that can be used for different types.

```kotlin
class MutableStack<E>(vararg items: E) {  
    private val elements = items.toMutableList()  
  
    fun push(element: E) = elements.add(element)  
  
    fun peek(): E = elements.last()  
  
    fun pop(): E = elements.removeAt(elements.size -1)  
  
    fun isEmpty() = elements.isEmpty()  
  
    fun size() = elements.size  
  
  override fun toString(): String = "Mutable(${elements.joinToString()})"  
}  
  
fun main() {  
  
    var os = MutableStack<String>("MacOS", "Linux", "Windows")  
    println(os.toString())  
  
    var age = MutableStack<Int>(1, 2, 3, 5)  
    println(age)  
  
}
```

#### Generic functions

It is also possible to create generic functions if their logic is independent of a specific type.

```kotlin
fun <E> mutableStackOf(vararg elements: E) = MutableStack(*elements)

val myStack = mutableStackOf("One", "Two", "Three")
```

### Inheritance

Class inheritance in Kotlin works as like any other OO language. However a few notes:

- All classes are `final` by default
- All class methods are also `final` by default
- Overriding methods require the `override` modifier

```kotlin
open class Animal {  
    open fun sayHello() {  
        TODO("to be implemented")  
    }  
}  
  
class Dog: Animal() {  
    override fun sayHello() {  
        println("woof woof")  
    }  
}  
  
fun main() {  
    val dog: Animal = Dog()  
    dog.sayHello()  
}
```

#### Inheritance with class constructor parameters

If you want to use a parameterised constructor of the superclass when creating a subclass, provide the constructor arguments in the subclass declaration.

```kotlin
open class Person(val name: String, val sex: String) {  
    fun sayHello() {  
        println("$name is a $sex")  
    }  
}  
  
class Female(name: String): Person(name = name, "female")  
class Male(name: String): Person(name = name, "male")  
  
fun main() {  
    val person1 = Female("Anna")  
    person1.sayHello()  
  
    val person2 = Male("John")  
    person2.sayHello()  
}
```
