
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

```kotlin
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

```kotlin
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

## Control Flow


### When
Instead of the normal `where`, kotlin uses `when`.

#### When statement

```kotlin
fun main() {  
    cases("Hello")  // "Greeting"
    cases(1)  		// "One"
    cases(0L)  		// "Long"
    cases(MyClass())  // "Not a string"
    cases("hello")   // "Unknown"
}  
  
fun cases(obj: Any) {  
    when (obj) {  
        1 -> println("One")  
        "Hello" -> println("Greeting")  
        is Long -> println("Long")  
        !is String -> println("Not a string")  
        else -> println("Unknown")  
    }  
}  
  
class MyClass
```

#### When expression

```kotlin
fun main() {  
    println(whenAssign("Hello"))  	// 1
    println(whenAssign(3.4))  		// 42
    println(whenAssign(1))  		// "One"
    println(whenAssign(MyClass()))  // 42
}  
  
fun whenAssign(obj: Any): Any {  
    var result = when (obj) {  
        1 -> "One"  
	   "Hello" -> 1  
	   is Long -> false  
	  else -> 42  
  }  
  
  return result  
}  
  
class MyClass
```


### Loops

#### `for` loop

```kotlin
val cakes = listOf("carrot", "cheese", "chocolate")  
  
for (cake in cakes) {  
    println("Yummy, it's a $cake cake!")  
}
```
#### `while` and `do-while`

```kotlin
fun eatACake() = println("Eat a Cake")  
fun bakeACake() = println("Bake a Cake")  
  
fun main() {  
  var cakesEaten = 0  
  var cakesBaked = 0  
  
  while (cakesEaten < 5) {                    // 1  
	    eatACake()  
        cakesEaten ++  
    }  
  
    do {                                        // 2  
	    bakeACake()  
        cakesBaked++  
    } while (cakesBaked < cakesEaten)  
  
}
```

### Iterators

Iterators can be implemented in classes by using the `iterator` operator in them.

```kotlin
class Animal(val name: String)  
  
class Zoo(val animals: List<Animal>) {  
    operator fun iterator(): Iterator<Animal> {  
        return animals.iterator()  
    }  
}  
  
fun main() {  
    val zoo = Zoo(listOf(Animal("Zebra"), Animal("Lion")))  
    for (animal in zoo) {  
        println("Watch out, it's a ${animal.name}")  
    }  
  
}
```

### Ranges

Ranges can be used in `for` loops

```kotlin
  
// iterates over a range from 0 - 3 (inclusive)  
for (i in 0..3) print(i)  
  
// iterates from 2 - 8 with 2 increments  
for (i in 2..8 step 2) print(i)  
  
// iterates over a range in reverse order  
for (i in 3 downTo 0) print(i)  
  
// chars are also supported  
for (c in 'a'..'d') print(c)  
  
// also supported reverse and step  
for (c in 'z' downTo 's' step 2) print(c)
```

but they can be also very useful in `if` statements

```kotlin
val x = 2  
if (x in 1..5) print ("x is in range")  
  
if (x !in 6..10) print("xis not in range from 6 to 10")
```

### Equality checks

Kotlin uses ``==`` for structural comparison and `===` for referential comparison.

As an eg: `a == b` is translated in Kotlin into `if(a == null) b == null else a.equals(b)`

```kotlin
val authors = setOf("Shakespeare", "Hemingway", "Twain")
val writers = setOf("Twain", "Shakespeare", "Hemingway")

println(authors == writers)   // 1
println(authors === writers)  // 2
```
1. returns `true` because `author.equals(writers)` and `Sets` ignore element order.
2. returns `false` because they are distinct references.



## Special Classes

### Data Classes

Data classes makes it easy to create classes that are used to store values (Entities, DTOs, etc..). They are provided with methods that help copying, copying and using in instances in collections.

```kotlin
data class User(val name: String, val id: Int)

val user1 = User("Anna", 1)
```

We can easily view the string representation by printing it as a string.

```kotlin
println(user1) // same as user1.toString() prints: User(name=Anna, id=1)
```


We can use `copy` to automatically instantiate another class with the same parameters
```kotlin
val user2 = user1.copy() // New instance with the same data

val user3 = user1.copy(id = 4) // We can change the values
```
Data classes also provided an auto-generated methods `componentN()` to get values in the order of the declaration

```kotlin
user1.component1() // Anna
user1.component2() // 1
```

### Enum classes

Enum classes are used to represent a finite set of distinct values. Such as directions, statuses, modes, etc..

```kotlin
enum class State {
	IDLE, RUNNING, FINISHED
}

val state = State.RUNNING
```
Enums can also contains methods like other classes.

```kotlin
enum class Color(val rgb: Int) {  
    RED(0xFF0000),  
    GREEN(0x00FF00),  
    BLUE(0x0000FF),  
    YELLOW(0xFFFF00);  
  
    fun containsRed() = (this.rgb and 0xFF0000 != 0)  
}  
  
val color = Color.YELLOW  
color.containsRed() // false
```

### Sealed classes

Sealed classes lets us restrict the inheritance of it. Once we declared a `sealed` class, it can only be subclassed from inside the same file where the `sealed` class was declared.

```kotlin
sealed class Vehicle(val wheels: Int)  

class Car(val brand: String): Vehicle(4) // subclassing from the same file
```

### Object keyword

#### Object expression
In Kotlin we can create anonymous instances, that have no particular class.
This is also useful when trying to create **Singleton**  pattern. It uses no class, no constructor and it's `lazy`. This means it will only be created when accessed, otherwise it will never be created.

```kotlin
val myHouse = object {  
  var address: String = "Vienna"  
  var rooms: Int = 3  
  var hasGarden: Boolean = false  
} 
println(myHouse.address) // "Vienna"
```

#### Object declaration

We can also use it as a declaration. It isn't an expression and can't be used in a variable assignment. Can only be used to access it's members directly.

```kotlin
object Home {  
    fun openDoor(passcode: String) {  
        println("Opening front doo with code $passcode")  
    }  
}

Home.openDoor("A4F277") // This is when the object is created and method called
```

#### Companion Objects

A companion object inside a class is like `static` using methods. Notice that it's not possible for companion objects to access the class methods/parameters.

```kotlin
class Person(var name: String) {  
    companion object Job {  
        fun getJobName(jobName: String) {  
            println("This person got a job as $jobName")  
        }  
    }  
}

Person.getJobName("Tester")
```
