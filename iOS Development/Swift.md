# Basics
## Tuples

**Declaration**:
```swift
let http404Error = (404, "Not found") //no name
let http200Error = (code: 200, message: "Success") //with name
```

**Accessing:**
```swift
let (statusCode, statusMessage) = http404Error
//statusCode = 404

let (justTheStatusCode, _) = http404Error
//statusCode = 404, we dont care about the statusMessage

let statusCode = http404Error.0 
//accessing by the index in the tuple

let successStatusCode = http200Error.code
```


## Handling optionals

### Optional Binding
```swift
if let food = optionalFood {
	...
}
```

### Fall back (nil-coalescing operator (`??`)
```swift
let name: String?

print("\(name ?? "No name")")
```

### Force Unwrapping
```swift
let name: String?
print("\(name!)")

//this is the same as: 
let number = convertedNumber! 
guard let number = convertedNumber else {
	 fatalError("The number was invalid") 
 }
```


## Error Handling

```swift
func canThrowAnError() throws { 
	// this function may or may not throw an error 
}

do {
    try canThrowAnError()
    // no error was thrown
} catch {
    // an error was thrown
}
```

# Operators

## Remainder % 
``` swift
9 % 4    // equals 1
```

## Unary (minus and plus)

```swift
let three = 3
let minusThree = -three       // minusThree equals -3
let plusThree = -minusThree   // plusThree equals 3, or "minus minus three"

let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix equals -6
```

## Compound Assignment Operator
```swift
var a = 1
a += 2
// a is equal to 3 now
```

## Range operators
### Closed range operator (`...`)
```swift
for i in 1...5 {
	//goes from 1, 2, 3, 4, 5
}
```

### Half open range operator (`..<`)
```swift
for i in 1..<5 {
	//goes from 1,2,3,4, it does not go to 5
}
```

### One-sided Ranges `[x...]`
```swift
let names = ["Anna", "Alex", "Brian", "Jack"]

for name in names[2...] {
    print(name)
}
// Brian
// Jack


for name in names[...2] {
    print(name)
}
// Anna
// Alex
// Brian



let range = ...5
range.contains(7)   // false
range.contains(4)   // true
range.contains(-1)  // true
```


# Collections

![[Pasted image 20241101130229.png]]
## Array
- All elements in an array must be of the **same type**.
- The values in an array are **ordered**, each having a specific position or index.
- An array can contain duplicate values, where the same element appears more than once in different positions.

```swift

//creating empty array
var someInts: [Int] = [] 

---
//creating array with default values
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
var threeDoubles = Array(repeating: 0.0, count: 3)
--
//Adding elements
shoppingList.append("Flour")
shoppingList += ["Baking Powder"]
// shoppingList now contains 4 items
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList now contains 7 items


shoppingList[0] = "Six eggs"
// the first item in the list is now equal to "Six eggs" rather than "Eggs"


//Iterating over an array

for item in shoppingList {
    print(item)
}
```


## Sets

A _set_ stores distinct values of the same type in a collection with no defined ordering. You can use a set instead of an array when the order of items isn’t important, or when you need to ensure that an item only appears once.

- same type
- no ordering
- use when order is not important

```swift
//Creating a set
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// Prints "letters is of type Set<Character> with 0 items."

letters.insert("a")
// letters now contains 1 value of type Character


//Creating a set with an array literal
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"] 
// favoriteGenres has been initialized with three initial items


//Infering the type when creating:
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]

---
//Removing
if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}

---
//Checking if a value is contained
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// Prints "It's too funky in here."
```

### Set operations
![[Pasted image 20241101131112.png]]



# Control flow
## Repeat-while
The `repeat`-`while` loop in Swift is analogous to a `do`-`while` loop in other languages.

```swift
repeat {
   <#statements#>
} while <#condition#>
```


## `Breakthrough`

```swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// Prints "The number 5 is a prime number, and also an integer."
```


### Labeled statements

```swift
<#label name#>: while <#condition#> {
   <#statements#>
}

//example: 

gameLoop: while square != finalSquare {
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    switch square + diceRoll {
    case finalSquare:
        // diceRoll will move us to the final square, so the game is over
        break gameLoop
    case let newSquare where newSquare > finalSquare:
        // diceRoll will move us beyond the final square, so roll again
        continue gameLoop
    default:
        // this is a valid move, so find out its effect
        square += diceRoll
        square += board[square]
    }
}
print("Game over!")
```


# Functions

Functions: all except “In-Out Parameters”  

```swift

//Functions with multiple return values: use tuple
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}

let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// Prints "min is -6 and max is 109"
```

## Variadic parameters
- **Definition**: A variadic parameter accepts zero or more values of a specified type.
- **Purpose**: To specify that the parameter can be passed a varying number of input values when the function is called.
- **Syntax**: Write variadic parameters by inserting three period characters (…) after the parameter’s type name. `func arithmeticMean(_ numbers: Double...)`

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8.25, 18.75)
// returns 10.0, which is the arithmetic mean of these three numbers
```


# Closures
Closures: all except “Escaping Closures” and “Autoclosures”   

## Closue Expresion Synxtax

``` swift
{ (<#parameters#>) -> <#return type#> in
   <#statements#>
}
```

## Examples:
```swift
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
// reversedNames is equal to ["Ewa", "Daniella", "Chris", "Barry", "Alex"]


reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 })

reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )

reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )

reversedNames = names.sorted(by: { $0 > $1 } )

reversedNames = names.sorted(by: >)

```

## Trailing closures
- Trailing Closure Usage: Useful for passing long closure expressions as the final argument to a function.
- Trailing Closure Syntax: Written after the function call’s parentheses, even though it’s still an argument.
- Trailing Closure Argument Label: Not included in the function call when using trailing closure syntax.

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // function body goes here
}


// Here's how you call this function without using a trailing closure:
someFunctionThatTakesAClosure(closure: {
    // closure's body goes here
})


// Here's how you call this function with a trailing closure instead:
someFunctionThatTakesAClosure() {
    // trailing closure's body goes here
}
```


# Enumerations
### Syntax
```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}

var directionToHead = CompassPoint.west


directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}


//Default 

let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
```

### Associated values

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}

var productBarcode = Barcode.upc(8, 85909, 51226, 3)
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")


switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
```


### Raw values
Enumeration cases can have default values (raw values) instead of associated values.

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```


#### Implicitely assigned raw values
```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
//In the example above, Planet.mercury has an explicit raw value of 1, Planet.venus has an implicit raw value of 2, and so on.


```

#### Accessing raw values
```swift
let earthsOrder = Planet.earth.rawValue
// earthsOrder is 3


let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection is "west"
```


# Structures and Classes

Idetity operators

It can sometimes be useful to find out whether two constants or variables refer to exactly the same instance of a class. To enable this, Swift provides two identity operators:

```swift
// Identical to (===)

// Not identical to (!==)


if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```


# Properties
## Global and Local Variabesl

- **Global Variable Definition**: Defined outside of any function, method, closure, or type context.
- **Local Variable Definition**: Defined within a function, method, or closure context.

## Static

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
}
enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 6
    }
}
class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}




print(SomeStructure.storedTypeProperty)
// Prints "Some value."
SomeStructure.storedTypeProperty = "Another value."
print(SomeStructure.storedTypeProperty)
// Prints "Another value."
print(SomeEnumeration.computedTypeProperty)
// Prints "6"
print(SomeClass.computedTypeProperty)
// Prints "27"
```

# Methods

## Instance Methods
Instance methods are functions that belong to instances of a particular class, structure, or enumeration. 

## Type Methods
Instance methods are called on instances of a type, while type methods, indicated by static or class, are called on the type itself.
```swift
class SomeClass {
    class func someTypeMethod() {
        // type method implementation goes here
    }
}
SomeClass.someTypeMethod()
```


## Initialization

```swift
//more than one init
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}


let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius is 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius is 0.0
```


# Optional chaining

```swift
if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// Prints "Unable to retrieve the address."



let johnsAddress = Address()
johnsAddress.buildingName = "The Larches"
johnsAddress.street = "Laurel Street"
john.residence?.address = johnsAddress


if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// Prints "John's street name is Laurel Street."
```

# Error Handling

## Using Enums
```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}

throw VendingMachineError.insufficientFunds(coinsNeeded: 5)

```

## Ways of handling errors in swift: 

- **Error Handling Methods**: Propagate error, use do-catch statement, handle as optional value, or assert no error.
- **Propagate Error**: Pass error from function to calling code.
- **Do-Catch Statement**: Handle error within a specific block of code.
```swift
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8
do {
    try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
    print("Success! Yum.")
} catch VendingMachineError.invalidSelection {
    print("Invalid Selection.")
} catch VendingMachineError.outOfStock {
    print("Out of Stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
} catch {
    print("Unexpected error: \(error).")
}
// Prints "Insufficient funds. Please insert an additional 2 coins."
```
- **Optional Value Handling**: Treat error as an optional value.
If an error is thrown while evaluating the try? expression, the value of the expression is nil
```swift
func someThrowingFunction() throws -> Int {
    // ...
}


let x = try? someThrowingFunction()


let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}

---

func fetchData() -> Data? {
    if let data = try? fetchDataFromDisk() { return data }
    if let data = try? fetchDataFromServer() { return data }
    return nil
}
```
- **Assert No Error**: Assume error will not occur.


## How to throw
```swift
func canThrowErrors() throws -> String

func cannotThrowErrors() -> String
```

# Async

•	Asynchronous Function Definition: A function that can be suspended while executing, waiting for something.
•	Asynchronous Function Indication: Marked with the `async` keyword in its declaration.

```swift
func listPhotos(inGallery name: String) async -> [String] {
    let result = // ... some asynchronous networking code ...
    return result
}


let photoNames = await listPhotos(inGallery: "Summer Vacation")
let sortedNames = photoNames.sorted()
let name = sortedNames[0]
let photo = await downloadPhoto(named: name)
show(photo)


```

# Type Casting

- **Type Casting Definition:** A way to check the type of an instance or treat an instance as a different superclass or subclass.
- **Type Casting Implementation**: Implemented with the `is` and `as` operators in Swift.
- **Operator Function**: The is operator checks the type of a value, while the as operator casts a value to a different type.
## Checking type
```swift
var movieCount = 0
var songCount = 0


for item in library {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}

print("Media library contains \(movieCount) movies and \(songCount) songs")
// Prints "Media library contains 2 movies and 3 songs"
```


## Downcasting
```swift
for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}


// Movie: Casablanca, dir. Michael Curtiz
// Song: Blue Suede Shoes, by Elvis Presley
// Movie: Citizen Kane, dir. Orson Welles
// Song: The One And Only, by Chesney Hawkes
// Song: Never Gonna Give You Up, by Rick Astley
```

## Type Casting for Any and AnyObject

```swift
var things: [Any] = []


things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })


for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called \(movie.name), dir. \(movie.director)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}


// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called Ghostbusters, dir. Ivan Reitman
// Hello, Michael
```


# Extensions

•	Extension Functionality: Adds **new functionality** to existing types.
•	Target Types: 
	- `Classes`
	- `structures`
	- `enumerations`
	- `protocols`
•	Retroactive Modeling: Extends types without access to the original source code.

### Extension Syntax
Declare extensions with the extension keyword:

```swift
extension SomeType {
    // new functionality to add to SomeType goes here
}
```

#### Adding protocols to a type
```swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // implementation of protocol requirements goes here
}
```

### Computed properties
```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
print("One inch is \(oneInch) meters")
// Prints "One inch is 0.0254 meters"
let threeFeet = 3.ft
print("Three feet is \(threeFeet) meters")
// Prints "Three feet is 0.914399970739201 meters"
```


### Methods
```swift
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}

3.repetitions {
    print("Hello!")
}
// Hello!
// Hello!
// Hello!
```

# Protocol

- Protocol Definition: A blueprint for methods, properties, and other requirements.
- Protocol Adoption: **Classes, structures, or enumerations** can adopt a protocol to provide an implementation.
- Protocol Conformance: A type conforms to a protocol if it satisfies the protocol’s requirements.

```swift
protocol SomeProtocol {
    // protocol definition goes here
}

struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}
```


# Generics

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

# Access Control
In Swift, access control modifiers determine the visibility of code elements (like properties, methods, classes, etc.) from other parts of a program. Here’s how they work:

### 1. `public`
- **Visibility:** Accessible from any file or module (even outside the current module).
- **Usage:** Used for elements intended to be used across different modules (e.g., frameworks or libraries).
- **Example:**
  ```swift
  public class MyClass {
      public var publicProperty = 0
      public func publicMethod() {}
  }
  ```

### 2. `internal` (default)
- **Visibility:** Accessible only within the current module (e.g., an app or framework).
- **Usage:** Often used for most internal code, as it provides access only within the module, preventing outside access.
- **Example:**
  ```swift
  class MyInternalClass {
      var internalProperty = 0 // implicitly internal
      func internalMethod() {}  // implicitly internal
  }
  ```

### 3. `fileprivate`
- **Visibility:** Accessible only within the same Swift file.
- **Usage:** Useful for grouping related code within a file, limiting access to that file only.
- **Example:**
  ```swift
  fileprivate class FilePrivateClass {
      fileprivate var filePrivateProperty = 0
      fileprivate func filePrivateMethod() {}
  }
  ```

### 4. `private`
- **Visibility:** Accessible only within the enclosing scope (like a class, struct, or extension).
- **Usage:** Best for hiding implementation details completely from outside the scope, even within the same file.
- **Example:**
  ```swift
  class PrivateClass {
      private var privateProperty = 0
      private func privateMethod() {}
  }
  ```

---

### `private(set)` and `fileprivate(set)` Variants
Sometimes, you want a property to be readable everywhere allowed by its access level but only writable in a limited scope. The `private(set)` and `fileprivate(set)` modifiers let you control that:

- **`private(set)`**: Makes the property read-only outside the enclosing scope, but writable within it.
- **`fileprivate(set)`**: Similar, but writable within the file.

**Example with `private(set)`**:
```swift
public class Example {
    public private(set) var readOnlyProperty = 0 // Writable within Example, read-only elsewhere
    func increment() {
        readOnlyProperty += 1
    }
}
```

**Example with `fileprivate(set)`**:
```swift
fileprivate class Example {
    fileprivate fileprivate(set) var readOnlyProperty = 0 // Writable within this file
    func increment() {
        readOnlyProperty += 1
    }
}
```

This setup is useful for properties that need controlled modification within specific areas but should be immutable elsewhere.

# Classes vs Structs

| Feature               | **Class**                          | **Struct**                         |
| --------------------- | ---------------------------------- | ---------------------------------- |
| **Inheritance**       | Supports inheritance               | No inheritance                     |
| **Type**              | Reference type                     | Value type                         |
| **Mutability**        | Mutable even with `let`            | Immutable with `let`               |
| **Deinitializers**    | Yes (`deinit`)                     | No                                 |
| **Identity (===)**    | Yes, to compare instance identity  | No identity, only value comparison |
| **Memory Management** | ARC (Automatic Reference Counting) | Not reference-counted              |
| **Type Methods**      | Can override class methods         | Static methods, no overrides       |
| **Initializers**      | Custom initializers required       | Automatic memberwise initializer   |

In general, you should use **structs** for simple, lightweight types where value semantics are.

# MVVM

- **Model**: This represents the data or business logic of the app. It is responsible for managing and processing data.
- **View**: The UI that the user interacts with. In SwiftUI, this is a struct that defines the appearance and layout of the screen.
- **ViewModel**: Acts as a bridge between the Model and the View. It handles the logic and state of the View, exposing data from the Model in a format that the View can easily consume.

### Example: A Simple To-Do App in SwiftUI

#### Step 1: The Model
The `Task` struct represents a to-do item.

##### Items to consider:


``` swift
struct Task {
    let id: UUID = UUID()
    var title: String
    var isCompleted: Bool
}
```

#### Step 2: The ViewModel
The ViewModel will manage an array of `Task` objects and provide functionality to add new tasks or toggle their completion status.

##### ==Template==
``` swift
class ViewModel: ObservableObject {
	@Published var publishedVar = ... // object that will be used by the view
	
}
```

```swift
import Combine

class TaskViewModel: ObservableObject {
    // Published property to notify the view of any changes
    @Published var tasks: [Task] = []
    
    // Function to add a new task
    func addTask(title: String) {
        let newTask = Task(title: title, isCompleted: false)
        tasks.append(newTask)
    }
    
    // Function to toggle the completion status of a task
    func toggleTaskCompletion(task: Task) {
        if let index = tasks.firstIndex(where: { $0.id == task.id }) {
            tasks[index].isCompleted.toggle()
        }
    }
}
```

#### Step 3: The View
The view will display the list of tasks and allow the user to add new tasks or mark them as completed.

##### ==Template==
``` swift
struct ____: View {
	@StateObject viewModel
}
```

``` swift
import SwiftUI

struct TaskView: View {
    // Reference to the ViewModel
    @StateObject var viewModel = TaskViewModel()
    @State private var newTaskTitle: String = ""
    
    var body: some View {
        VStack {
            // Input field for adding a new task
            HStack {
                TextField("Enter task", text: $newTaskTitle)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                Button(action: {
                    viewModel.addTask(title: newTaskTitle)
                    newTaskTitle = "" // Clear the text field after adding
                }) {
                    Text("Add")
                }
            }.padding()
            
            // List of tasks
            List(viewModel.tasks) { task in
                HStack {
                    Text(task.title)
                    Spacer()
                    Button(action: {
                        viewModel.toggleTaskCompletion(task: task)
                    }) {
                        Image(systemName: task.isCompleted ? "checkmark.circle.fill" : "circle")
                    }
                }
            }
        }
    }
}
```

### How It Works:

1. **Model**: The `Task` struct is the data layer that represents a to-do item with a title and completion status.
2. **ViewModel**: The `TaskViewModel` class manages the list of tasks. It also provides methods to add new tasks and toggle their completion status. The ==`@Published`== property wrapper is used to notify the View whenever the task list changes.
3. **View**: The `TaskView` struct is the UI. It binds to the `TaskViewModel` using ==`@StateObject`==, observing changes to the `tasks` array. The UI includes a `TextField` for entering new tasks and a `List` to display the tasks, with buttons for toggling the completion status.

This separation allows the ViewModel to handle the business logic (adding and updating tasks) while the View focuses solely on displaying data. The ViewModel doesn’t need to know about the UI, making it easy to test and reuse.

###  Note on `@StateObject` and `@ObservedObject`

- ==`@StateObject`== is used by the parent view.
- ==`@ObservedObject`== is used by the child view.

--- 


# Property Wrappers in `SwiftUI`
Here’s a list of **SwiftUI property wrappers** with brief descriptions of their use:

### 1. **@State**
- **Description**: Stores mutable state **local to a view**. Triggers UI updates when the value changes.
- **Use case**: Simple, local state (e.g., toggle switches, counters).

### 2. **@Binding**
- **Description**: Creates a two-way connection to mutable state from a parent view or another source. Allows a child view to **read and modify** the parent's state.
- **Use case**: Pass data that needs to be changed by a child view (e.g., a slider adjusting a parent’s value).

### 3. **@ObservedObject**
- **Description**: Watches an **ObservableObject** for changes and refreshes the view when the observed object’s published properties change.
- **Use case**: Track data from **external objects** that might be shared between views (e.g., a shared data model).

### 4. **@StateObject**
- **Description**: Creates and manages an instance of an **ObservableObject**. This view **owns** the object for its lifetime.
- **Use case**: When a view **creates** and manages an observable data source, like a ViewModel.

### 5. **@EnvironmentObject**
- **Description**: Retrieves an **ObservableObject** from the environment and watches it for changes. Ideal for sharing data across multiple views.
- **Use case**: App-wide or multi-view shared state (e.g., app settings, user session).

### 6. **@Environment**
- **Description**: Accesses values from the SwiftUI environment, such as system settings (e.g., `colorScheme`, `locale`).
- **Use case**: Read system-related values like device orientation or accessibility settings.

### 7. **@Published**
- **Description**: Declares a property inside an **ObservableObject** that announces changes to any observing views or objects.
- **Use case**: Use inside **ObservableObject** classes to publish changes to the state.

### 8. **@FetchRequest**
- **Description**: Fetches data from **Core Data** and automatically updates the view when the fetched data changes.
- **Use case**: Display and update Core Data entities in real time.

### 9. **@AppStorage**
- **Description**: Reads and writes data to **UserDefaults** using a key.
- **Use case**: Store and retrieve small persistent values (e.g., user preferences).

### 10. **@SceneStorage**
- **Description**: Stores small amounts of state that should persist between app launches or scene sessions, using **scene-specific storage**.
- **Use case**: Store view state or data that needs to persist when the app is backgrounded.

### 11. **@GestureState**
- **Description**: Tracks the state of a gesture, resetting to its initial value when the gesture ends.
- **Use case**: Handle gestures (e.g., dragging, pinching).

### 12. **@Namespace**
- **Description**: Creates a shared namespace for matching animations between views.
- **Use case**: Enables **matched geometry effects** for animations between different views.

---

# Property Observers
In Swift, **property observers** are used to monitor and respond to changes in a property’s value. They allow you to run custom code before or after a property changes.

There are two types of property observers:
1. **willSet**: Called **before** the property’s value is changed. The new value is available as `newValue`.
2. **didSet**: Called **after** the property’s value is changed. The old value is accessible via the property, but `didSet` doesn't pass the old value explicitly.

### Syntax for Property Observers:

```swift
var property: Type {
    willSet {
        // Code that runs before the property is set
        print("About to set \(property) to \(newValue)")
    }
    didSet {
        // Code that runs after the property is set
        print("Did set \(property) from \(oldValue) to \(property)")
    }
}
```

### Example:
Here’s an example with a `score` property that uses `willSet` and `didSet`:

```swift
var score: Int = 0 {
    willSet {
        print("Score is about to change from \(score) to \(newValue)")
    }
    didSet {
        print("Score changed from \(oldValue) to \(score)")
    }
}
```

### Explanation:
- **willSet**: This block executes right before the value of `score` is updated, and it provides the new value as `newValue`.
- **didSet**: This block runs right after the value of `score` is updated, with access to the old value through `oldValue`.

### Use Cases:
1. **Logging or Debugging**: Track changes to important properties.
2. **Validation or Adjustment**: Modify or validate a property after it changes (e.g., limiting a value to a certain range).
3. **UI Updates**: Trigger actions when properties change, like updating a UI element in response to a value change (though in SwiftUI, `@State` and other property wrappers are preferred for UI-bound state).

Property observers work for any type of property except lazy properties, and they are ideal for non-SwiftUI contexts where you need to react to changes in non-observable properties.

---

# TypeAlias

In Swift, a **typealias** is a way to create a new name for an existing type. It allows you to define a more readable or convenient name for complex types, making your code easier to understand.

### Simple definition
==way to create a new name for an existing type==
### Simple Explanation:
- Think of a typealias as a **nickname** for a type. Instead of always writing the full name of a type, you can use a shorter or more descriptive name.
  
### Syntax:
```swift
typealias NewName = ExistingType
```

### Example:
Let’s say you have a complex type for a closure (a block of code):

```swift
typealias CompletionHandler = (Bool, String) -> Void
```

Now, instead of writing the full closure type every time you want to use it, you can just use `CompletionHandler`:

```swift
func performAction(completion: CompletionHandler) {
    // Perform some action
    completion(true, "Success!")
}
```

### Benefits:
1. **Improves Readability**: Makes your code easier to read and understand, especially when     dealing with complex types.
2. **Reduces Repetition**: Avoids having to write long type definitions multiple times.
3. **Clarifies Purpose**: You can give a meaningful name that explains what the type represents.

### Summary:
A **typealias** is a tool in Swift that helps you create a simpler or more meaningful name for an existing type, making your code clearer and easier to work with.



# Forms

## My Stuff
Things I learned after following the tutorial from professor Liddle. 

`Forms` are really good to help place elements in the screen. 

```swift
Form {
	  Section {
		  ...
		  Button {
			  //action
		  } label: {
			  Label("Report", systemImage: "square.and.arrow.up")
		  }
	  }

	  Section("Header Title") {
		  List {
			  ForEach() {
			  
			  }
		  }
	  }
	
}
```

Something like this code creates something like this: 
![[Pasted image 20241001182023.png]]

## ChatGPT stuff
In SwiftUI, forms are essential for gathering user input and displaying structured information. You can use forms to create organized interfaces, such as data entry screens or settings pages. Combining forms with lists, sections, and other SwiftUI elements allows you to build interactive, well-structured user interfaces. Below are some key use cases and examples:

### 1. **Basic Form with TextField, Toggle, and Slider**
SwiftUI `Form` can easily group input fields. A typical use case is creating settings or profile forms.

```swift
import SwiftUI

struct SettingsView: View {
    @State private var username: String = ""
    @State private var notificationsEnabled: Bool = true
    @State private var volume: Double = 50.0

    var body: some View {
        Form {
            TextField("Username", text: $username)

            Toggle("Enable Notifications", isOn: $notificationsEnabled)

            Slider(value: $volume, in: 0...100) {
                Text("Volume")
            }
        }
    }
}
```
This simple form allows you to input text, toggle a switch, and adjust a slider.

---

### 2. **Form with Sections**
To better organize a form, you can use `Section`. Sections allow you to group form elements logically, with optional headers and footers.

```swift
import SwiftUI

struct ProfileFormView: View {
    @State private var firstName: String = ""
    @State private var lastName: String = ""
    @State private var email: String = ""
    @State private var newsletter: Bool = false

    var body: some View {
        Form {
            Section(header: Text("Personal Info")) {
                TextField("First Name", text: $firstName)
                TextField("Last Name", text: $lastName)
            }

            Section(header: Text("Contact Info")) {
                TextField("Email", text: $email)
                Toggle("Subscribe to Newsletter", isOn: $newsletter)
            }
        }
    }
}
```
Each section can have a header or footer, making the form more structured and readable.

---

### 3. **Form with Lists**
You can integrate lists into forms to display dynamic content such as a list of selectable items.

```swift
import SwiftUI

struct TaskFormView: View {
    @State private var tasks: [String] = ["Task 1", "Task 2", "Task 3"]
    @State private var selectedTask: String?

    var body: some View {
        Form {
            Section(header: Text("Tasks")) {
                List(tasks, id: \.self) { task in
                    Text(task)
                        .onTapGesture {
                            selectedTask = task
                        }
                }
            }

            if let selectedTask = selectedTask {
                Section(header: Text("Selected Task")) {
                    Text("You selected \(selectedTask)")
                }
            }
        }
    }
}
```
Here, a `List` is embedded within the `Form`, allowing the user to select a task. The selected task is then shown in another section.

---

### 4. **Form with Picker**
A `Picker` is often used in forms to provide options for selection.

```swift
import SwiftUI

struct PickerFormView: View {
    @State private var selectedColor: String = "Red"
    let colors = ["Red", "Green", "Blue"]

    var body: some View {
        Form {
            Section(header: Text("Select a Color")) {
                Picker("Favorite Color", selection: $selectedColor) {
                    ForEach(colors, id: \.self) {
                        Text($0)
                    }
                }
                .pickerStyle(SegmentedPickerStyle()) // Optional, but you can change styles
            }

            Section(header: Text("Selected Color")) {
                Text("You selected: \(selectedColor)")
            }
        }
    }
}
```
A `Picker` allows the user to select from a list of options and works seamlessly within a form.

---

### 5. **Form with Dynamic Lists and Edit Functionality**
SwiftUI forms can also contain editable lists, allowing users to add or delete items dynamically.

```swift
import SwiftUI

struct EditableListView: View {
    @State private var items: [String] = ["Item 1", "Item 2", "Item 3"]
    @State private var newItem: String = ""

    var body: some View {
        Form {
            Section(header: Text("Items")) {
                List {
                    ForEach(items, id: \.self) { item in
                        Text(item)
                    }
                    .onDelete(perform: deleteItem)
                }

                TextField("New Item", text: $newItem)
                Button("Add Item") {
                    addItem()
                }
            }
        }
    }

    private func addItem() {
        if !newItem.isEmpty {
            items.append(newItem)
            newItem = ""
        }
    }

    private func deleteItem(at offsets: IndexSet) {
        items.remove(atOffsets: offsets)
    }
}
```
In this example, the user can dynamically add new items and remove existing ones from the list.

---

### 6. **Advanced Form with Navigation Links**
Combining a `Form` with `NavigationView` lets you link form rows to detailed views or other screens.

```swift
import SwiftUI

struct NavigationFormView: View {
    @State private var profileName: String = ""

    var body: some View {
        NavigationView {
            Form {
                Section {
                    TextField("Profile Name", text: $profileName)
                    NavigationLink(destination: DetailView()) {
                        Text("Go to Details")
                    }
                }
            }
            .navigationTitle("Profile Form")
        }
    }
}

struct DetailView: View {
    var body: some View {
        Text("Detail Screen")
            .navigationTitle("Details")
    }
}
```
With `NavigationLink`, you can move from one screen to another while interacting with the form.

---

### Summary
Forms in SwiftUI can be combined with lists, sections, pickers, and even navigation elements to create powerful user interfaces. Key patterns include:

- Organizing input fields with `Section`.
- Embedding `List` for dynamic content.
- Using `Picker` for selections.
- Supporting dynamic user actions like adding and deleting items.
- Navigating between different views using `NavigationLink`.

These tools help you build interactive and structured forms tailored to various use cases in SwiftUI.


# Things I learned by following the Calculator tutorial: 

I can change the name of the app and the icon in the 'Target' options. 
	- That's where I also select a version, an app category and a build. 

### Concurrency

A function marked with `async` can be called from a `Task`, which will run the function in another thread. 

#### Summary:
1. declare `async` function
2. call it from inside a `Task` block
3. call it with a `await` prefix
#### Example: 
```swift
    func handleButtonTap(for buttonSpec: ButtonSpec) {
        if preferences.soundIsEnabled {
            Task {
                await soundPlayer.playSound(named: "Click2.m4a")
            }
        }
    }

	mutating func playSound(named soundName: String) async {
        guard let path = Bundle.main.path(forResource: soundName, ofType: nil) else {
            return
        }

        do {
            player = try AVAudioPlayer(contentsOf: URL(fileURLWithPath: path))
            player?.play()
        } catch {
            // Ignore -- the sound just won’t play
        }
    }

```

### Model access from the view model

To access `model` properties from the `viewModel` I can create those properties as a `computed variable` in my `vm`. 
#### Example
```swift
struct Model {
	var persistedProperty ...
}

class ViewModel: ... {
	var model ...

	// MARK: - Model Access
	var property: Type {
		model.persistedProperty
	}
	
}
```

### Multiple carets

`ctrl + shift` to insert multiple carets in the editor. 

### Understanding `@Published` in the ViewModel

> [!NOTE]
> My original question was how does the `CalculatorButton` is notified of the changes happening in the `CalculatorViewModel` when the vm was not an observed property of the `CalculatorButton`. This is what ChatGPT said. 
> 

By ChatGPT: 

In your code, the synchronization between the `CalculatorButton` views and the `CalculatorViewModel` occurs because of SwiftUI's reactive nature and the use of the `@Bindable` and `@Observable` attributes.

Here’s how it works step by step:

### 1. **`@Observable` and `@Bindable`**:
- **`@Observable`**: When you mark a class with `@Observable`, SwiftUI is aware that this class can be observed for changes. Even though you haven't marked any specific properties with `@Published`, SwiftUI is still tracking the object itself for changes.
  
  When a property or method inside `CalculatorViewModel` changes, SwiftUI will know to recompute any views that depend on it, as long as those views use the `@Bindable` property to refer to the view model.

- **`@Bindable`**: This is used in your view (`CalculatorView`), and it automatically subscribes to changes in `CalculatorViewModel`. This is how the `CalculatorButton` receives updates.

### 2. **The `CalculatorButton` and ViewModel Relationship**:
In the `CalculatorButton` view, you are using `let calculatorViewModel: CalculatorViewModel`. Even though it is a constant reference, SwiftUI will still track changes in the view model because it is an `@Observable` object.

The `backgroundColor` property inside `CalculatorButton` depends on the `calculatorViewModel.activeSymbol` property:
```swift
private var backgroundColor: Color {
    buttonSpec.symbol == calculatorViewModel.activeSymbol ? buttonSpec.type.foregroundColor : buttonSpec.type.backgroundColor
}
```

If `calculatorViewModel.activeSymbol` changes, SwiftUI will automatically re-render the `CalculatorButton` views that rely on this property. This keeps the buttons synchronized with the view model.

### 3. **Why Synchronization Works Without `@Published` Properties**:
Although you don’t have any `@Published` variables, SwiftUI can still trigger updates because the entire `CalculatorViewModel` is `@Observable`. If any properties or methods inside that view model change, SwiftUI will react to those changes, triggering the necessary updates to the views using `calculatorViewModel`.

In short:
- **`@Observable` on `CalculatorViewModel`**: SwiftUI listens for changes to the view model object.
- **`@Bindable` in `CalculatorView`**: Connects the view to the view model, so any changes to the model result in updates to the views.
- **SwiftUI’s automatic updates**: If a property like `calculatorViewModel.activeSymbol` changes, SwiftUI automatically re-renders any views (like `CalculatorButton`) that depend on it.

This design works without explicitly marking properties as `@Published`, but if you start working with more specific properties (like updating numbers or operations), you might want to add `@Published` for more fine-grained control over updates.


### `private` setter in a variable

```swift
private(set) var accumulator: Double? = 0
//Now this variable is Read-only (getter)
```

### Simplifying views

```swift
var internalView: some View {
	 ...
} //for views without any variables required from the outside

func anotherInternalView(for parameter) -> some View {
	  ...
} //for views that need info from the outside
```

# `@Observable` macro

Introduced in iOS17, it is used for simplifying the `view model` creation and binding process.
## How to use:

### 1. Define the view model with the `@Observable` macro. 

```swift
@Observable
class ____ViewModel {
	 ...
}
```

### 2. Declare the view model with `@State` in the view
```swift
struct ____View: View {
	@State var vm = ____ViewModel()
	...
}
```

### [Store observable objects](https://developer.apple.com/documentation/swiftui/state#Store-observable-objects)

You can also store observable objects that you create with the [`Observable()`](https://developer.apple.com/documentation/Observation/Observable()) macro in `State`

for example:

```swift
@Observable
class Library {
    var name = "My library of books"
    // ...
}


struct ContentView: View {
    @State private var library = Library()


    var body: some View {
        LibraryView(library: library)
    }
}
```

A `State` property always instantiates its default value when SwiftUI instantiates the view. For this reason, avoid side effects and performance-intensive work when initializing the default value. For example, if a view updates frequently, allocating a new default object each time the view initializes can become expensive. Instead, you can defer the creation of the object using the [`task(priority:_:)`](https://developer.apple.com/documentation/swiftui/view/task(priority:_:)) modifier, which is called only once when the view first appears:

```swift
struct ContentView: View {
    @State private var library: Library?


    var body: some View {
        LibraryView(library: library)
            .task {
                library = Library()
            }
    }
}
```

Delaying the creation of the observable state object ensures that unnecessary allocations of the object doesn’t happen each time SwiftUI initializes the view. Using the [`task(priority:_:)`](https://developer.apple.com/documentation/swiftui/view/task(priority:_:)) modifier is also an effective way to defer any other kind of work required to create the initial state of the view, such as network calls or file access.

#### Note

It’s possible to store an object that conforms to the [`ObservableObject`](https://developer.apple.com/documentation/Combine/ObservableObject) protocol in a `State` property. However the view will only update when the reference to the object changes, such as when setting the property with a reference to another object. The view will not update if any of the object’s published properties change. To track changes to both the reference and the object’s published properties, use [`StateObject`](https://developer.apple.com/documentation/swiftui/stateobject) instead of [`State`](https://developer.apple.com/documentation/swiftui/state) when storing the object.

### [Share `@Observable` state objects with subviews](https://developer.apple.com/documentation/swiftui/state#Share-observable-state-objects-with-subviews)

To share an [`Observable`](https://developer.apple.com/documentation/Observation/Observable) object stored in `State` with a subview, pass the object reference to the subview. SwiftUI updates the subview anytime an observable property of the object changes, but only when the subview’s [`body`](https://developer.apple.com/documentation/swiftui/view/body-8kl5o) reads the property. For example, in the following code `BookView` updates each time `title` changes but not when `isAvailable` changes:

```swift
@Observable
class Book {
    var title = "A sample book"
    var isAvailable = true
}


struct ContentView: View {
    @State private var book = Book()


    var body: some View {
        BookView(book: book)
    }
}


struct BookView: View {
    var book: Book


    var body: some View {
        Text(book.title)
    }
}
```

`State` properties provide bindings to their value. When storing an object, you can get a [`Binding`](https://developer.apple.com/documentation/swiftui/binding) to that object, specifically the reference to the object. This is useful when you need to change the reference stored in state in some other subview, such as setting the reference to `nil`:

```swift
struct ContentView: View {
    @State private var book: Book?


    var body: some View {
        DeleteBookView(book: $book)
            .task {
                book = Book()
            }
    }
}


struct DeleteBookView: View {
    @Binding var book: Book?


    var body: some View {
        Button("Delete book") {
            book = nil
        }
    }
}
```

However, passing a [`Binding`](https://developer.apple.com/documentation/swiftui/binding) to an object stored in `State` isn’t necessary when you need to change properties of that object. For example, you can set the properties of the object to new values in a subview by passing the object reference instead of a binding to the reference:

```swift
struct ContentView: View {
    @State private var book = Book()


    var body: some View {
        BookCheckoutView(book: book)
    }
}


struct BookCheckoutView: View {
    var book: Book


    var body: some View {
        Button(book.isAvailable ? "Check out book" : "Return book") {
            book.isAvailable.toggle()
        }
    }
}
```

If you need a binding to a specific property of the object, pass either the binding to the object and extract bindings to specific properties where needed, or pass the object reference and use the [`Bindable`](https://developer.apple.com/documentation/swiftui/bindable) property wrapper to create bindings to specific properties. For example, in the following code `BookEditorView` wraps `book` with `@Bindable`. Then the view uses the `$` syntax to pass to a [`TextField`](https://developer.apple.com/documentation/swiftui/textfield) a binding to `title`:

```swift
struct ContentView: View {
    @State private var book = Book()


    var body: some View {
        BookView(book: book)
    }
}


struct BookView: View {
    let book: Book


    var body: some View {
        BookEditorView(book: book)
    }
}


struct BookEditorView: View {
    @Bindable var book: Book


    var body: some View {
        TextField("Title", text: $book.title)
    }
}
```



# Gestures
## OnTapGesture
In Swift, particularly with **SwiftUI**, a **tap gesture** is used to detect taps or clicks on a view. You can attach this gesture to almost any view to handle touch or click events. The `onTapGesture` modifier is the most common way to implement this gesture.

### Basic Usage
Here's a simple example of how you would use `onTapGesture` in SwiftUI:

```swift
Text("Tap me!")
    .padding()
    .onTapGesture {
        print("Text was tapped!")
    }
```

### Parameters
`onTapGesture` comes with an optional parameter that lets you specify how many taps to recognize before triggering the action.

- **count**: Specifies the number of taps required to recognize the gesture.

Example for detecting a double-tap (two taps):

```swift
Text("Double-tap me!")
    .padding()
    .onTapGesture(count: 2) {
        print("Text was double-tapped!")
    }
```

### Common Use Cases
- **Buttons**: Replace buttons when you need more customized views that should respond to taps.
- **Image interactions**: For images or icons that users might need to tap for interaction.
- **Custom gestures**: If you need more control over gesture recognition.

### Combining with Other Gestures
You can also combine `onTapGesture` with other gestures like `longPressGesture` or `dragGesture` for more complex interactions.

```swift
Text("Tap or long press me!")
    .onTapGesture {
        print("Tapped!")
    }
    .onLongPressGesture {
        print("Long pressed!")
    }
```

Let me know if you'd like to explore any specific gesture handling details or examples!

# Extensions

Example from the Concentration game:
```swift
extension Array where Element: Identifiable {
	 func firstIndex(matching targetElement: Element) -> Int? {
		 for index in self.indices {
			 if self[index].id == targetElement.id {
				 return index
			 }
		 }
		 
		 return nil
	 }
}
```



# Rendering computed properties in the View, from the ViewModel

In SwiftUI, the view can absolutely render a computed property from the ViewModel, and that computed property can, in turn, return another computed property from the model. This works because SwiftUI doesn't care whether the data being rendered is from a `stored property` or a `computed property`—it just needs something to display.

> [!Important Note]
> ### **Note**: `model` needs to be `struct`, not a `class`
> - **Structs** in Swift are value types, meaning that when a `struct` is modified, a new copy is created. This causes SwiftUI to detect the change because SwiftUI tracks changes by checking for differences in the values of `structs`.
> - **Classes** are reference types, meaning that when you modify properties within a class, SwiftUI does not automatically know the class instance has changed unless the property itself is marked with `@Published`. 

### Here's how it works:

1. **ViewModel**: The ViewModel typically conforms to `ObservableObject`, and you will mark it with `@Published` for any properties that need to notify the view when they change. If you want to expose a computed property, that's fine because SwiftUI will re-render whenever the underlying data changes.

2. **Model**: The Model is usually just a plain Swift class or struct. It can also have computed properties, and the ViewModel can expose those to the view as well.

### Example

```swift
import SwiftUI

// Model with a computed property. Model needs to be a **struct**
struct UserModel {
    var firstName: String
    var lastName: String

    var fullName: String {
        return "\(firstName) \(lastName)"
    }
}

// ViewModel exposing the model's computed property
class UserViewModel: ObservableObject {
    @Published var user: UserModel

    init(user: UserModel) {
        self.user = user
    }

    // Computed property in the ViewModel
    var displayName: String {
        return user.fullName
    }
}

// SwiftUI View using the computed property
struct ContentView: View {
    @ObservedObject var viewModel: UserViewModel

    var body: some View {
        VStack {
            Text(viewModel.displayName) // Rendering the computed property from the ViewModel
        }
    }
}

// Sample usage
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView(viewModel: UserViewModel(user: UserModel(firstName: "John", lastName: "Doe")))
    }
}
```

### How it works:

- **Model (`UserModel`)**: Has a computed property `fullName` that combines `firstName` and `lastName`.
- **ViewModel (`UserViewModel`)**: Exposes a computed property `displayName` that returns the `fullName` from the model.
- **View (`ContentView`)**: Renders the `displayName` from the ViewModel.

SwiftUI will automatically update the view whenever the `@Published` `user` changes, causing the `displayName` to update and re-render.

## Note. **Do not mark `computed properties` with @Published**
No, a computed property **cannot** directly be marked with `@Published` in Swift. This is because `@Published` is used to observe changes to stored properties, and computed properties do not store values—they dynamically calculate them based on other data.

However, if the computed property depends on one or more `@Published` stored properties, the view will still automatically update whenever those underlying properties change. This is because SwiftUI observes changes in the `@Published` properties, and when those change, the computed property is recalculated.

![[Pasted image 20241016130559.png]]

### Example:

```swift
import SwiftUI

class UserViewModel: ObservableObject {
    @Published var firstName: String = "John"
    @Published var lastName: String = "Doe"

    // Computed property depending on @Published properties
    var fullName: String {
        return "\(firstName) \(lastName)"
    }
}

struct ContentView: View {
    @ObservedObject var viewModel: UserViewModel

    var body: some View {
        VStack {
            Text(viewModel.fullName) 
            // This will update when firstName or lastName change
            
            TextField("First Name", text: $viewModel.firstName)
            TextField("Last Name", text: $viewModel.lastName)
        }
        .padding()
    }
}
```

### Key points:
- The `@Published` properties `firstName` and `lastName` are stored properties.
- The computed property `fullName` is automatically recalculated and re-rendered in the view whenever `firstName` or `lastName` changes.
- Even though `fullName` cannot be marked with `@Published`, it still participates in view updates due to the changes in `@Published` properties.

If you need to notify SwiftUI about changes that are not based on stored `@Published` properties, you should consider using a stored property combined with some logic that updates the view explicitly.


# How to use `ViewModifier`

The `ViewModifier` protocol in SwiftUI is used to encapsulate a reusable piece of code that applies modifications to a view or group of views. This helps in making your view code more modular and reusable, as well as reducing the need to repeatedly apply the same set of modifications across multiple views.

Here’s a breakdown of how to use the `ViewModifier` protocol:

### 1. Defining a ViewModifier

To create a custom modifier, you define a struct that conforms to the `ViewModifier` protocol and implement its `body` function. This function returns the modified view.

Here's an example:

```swift
struct CustomModifier: ViewModifier {
    func body(content: Content) -> some View {
        content
            .padding()
            .background(Color.blue)
            .cornerRadius(10)
            .shadow(radius: 5)
    }
}
```

In this example:
- `content` represents the original view that the modifier will be applied to.
- The modifier adds padding, a background color, rounded corners, and a shadow to the content.

### 2. Applying a ViewModifier

You apply the `ViewModifier` to a view using the `.modifier()` function.

```swift
struct ContentView: View {
    var body: some View {
        Text("Hello, World!")
            .modifier(CustomModifier())
    }
}
```

### 3. Making It More Flexible

You can make the `ViewModifier` flexible by allowing parameters to be passed in, so it can be customized for different uses.

For example, a `HighlightModifier` that takes a color as an argument:

```swift
struct HighlightModifier: ViewModifier {
    var color: Color
    
    func body(content: Content) -> some View {
        content
            .padding()
            .background(color)
            .cornerRadius(10)
    }
}
```

Then you apply it like this:

```swift
struct ContentView: View {
    var body: some View {
        Text("Important")
            .modifier(HighlightModifier(color: .yellow))
    }
}
```

### 4. Using View `Extensions` for Convenience

A common practice is to create a convenience extension on `View` so that you can apply the modifier more cleanly.

```swift
extension View {
    func highlight(with color: Color) -> some View {
        self.modifier(HighlightModifier(color: color))
    }
}
```

Now, you can apply the modifier like this:

```swift
struct ContentView: View {
    var body: some View {
        Text("Important")
            .highlight(with: .yellow)
    }
}
```

### Benefits of `ViewModifier`:
- **Reusability**: You can define complex styling and reuse it across your app.
- **Separation of concerns**: Styling is kept separate from your view logic.
- **Readability**: It improves the readability of your views by simplifying complex view hierarchies.

In summary, the `ViewModifier` protocol allows you to encapsulate view modifications, making your SwiftUI code more modular and reusable!

# `fileprivate`

In Swift, `fileprivate` is an access control modifier that restricts the visibility of a class, function, property, or method to the file in which it is defined. This means that anything marked with `fileprivate` **can only be accessed and used within the same file.** It is more restrictive than `internal` (the default level), which allows access across the entire module, but less restrictive than `private`, which confines access to the scope of a type or its extensions within the same file.

For example, `fileprivate` is useful when you want to hide implementation details from other files while still allowing different parts of the same file to interact. This can be helpful in organizing code when multiple types or extensions are defined within a single file and need to share access to some internal components.

# Animation
## Differences between Implicit and Explicit
The key difference between **implicit** and **explicit** animations in SwiftUI is how and when the animations are triggered and controlled.

### Implicit Animations
In **implicit animations**, SwiftUI automatically handles the animation ==when a state change occurs==. You simply specify that a property should be animated when its value changes, and SwiftUI animates the change for you. You don’t have to explicitly tell SwiftUI when to start the animation.

- **How it works:** You attach the `.animation()` modifier to a view, and whenever a state change that affects that view occurs, SwiftUI will automatically animate the transition from the old state to the new state.
- **Control:** You have less direct control over *when* the animation starts—it's automatically triggered by state changes.
- **Where to use:** You can use implicit animations when you want to animate simple changes like resizing, opacity, or movement, without needing complex coordination.

Example:

```swift
struct ImplicitAnimationView: View {
    @State private var isScaled = false

    var body: some View {
        Circle()
            .frame(width: isScaled ? 200 : 100, height: isScaled ? 200 : 100)
            .animation(.easeInOut(duration: 1), value: isScaled) // Implicit animation
            .onTapGesture {
                isScaled.toggle() // Tapping triggers the animation
            }
    }
}
```

Here, changing the `isScaled` state automatically triggers the animation attached to the `Circle`. The change in size is animated without having to call any animation explicitly.

### Explicit Animations
**Explicit animations** require you to manually define when and how the animation should start by wrapping the property change inside a `withAnimation` block. This gives you more control over *when* the animation is triggered and allows you to handle more complex animation scenarios.

- **How it works:** You use the `withAnimation {}` closure to define precisely when the animation should occur. Inside this block, you perform any state changes you want to animate.
- **Control:** You have explicit control over when and how the animation runs. SwiftUI won’t animate the change unless it's inside the `withAnimation` block.
- **Where to use:** Explicit animations are ideal when you want to animate multiple state changes simultaneously or need more control over timing and execution.

Example:

```swift
struct ExplicitAnimationView: View {
    @State private var isScaled = false

    var body: some View {
        Circle()
            .frame(width: isScaled ? 200 : 100, height: isScaled ? 200 : 100)
            .onTapGesture {
                withAnimation(.easeInOut(duration: 1)) { // Explicit animation
                    isScaled.toggle() // Manually triggering the animation
                }
            }
    }
}
```

Here, the size change is only animated when wrapped inside the `withAnimation` closure. Without that block, SwiftUI would immediately apply the state change without any animation.

### Key Differences
| Feature              | Implicit Animation                                      | Explicit Animation                                      |
|----------------------|---------------------------------------------------------|---------------------------------------------------------|
| **Trigger**           | Automatically triggered by state changes                | Manually triggered by `withAnimation {}` block           |
| **Control**           | Less control over when the animation starts             | Full control over the timing of the animation            |
| **Use Case**          | Simple animations like resizing, moving, or opacity     | More complex or multi-state animations                   |
| **How to Implement**  | Add `.animation()` modifier to a view                   | Use `withAnimation {}` block to wrap the state changes   |

### When to Use Each
- **Implicit animations** are great for simple, declarative UI changes where you just want to automatically animate when properties change.
- **Explicit animations** give you control over exactly when animations should start, making them better suited for complex animations or when you need to synchronize multiple changes.

By understanding these two types, you can decide which approach fits your animation needs!



# Controlling tappable area using `.contentShape` view modifier

- I used `.contentShape` to fix the issue with dragging the cells for the Tools reordering in the BYU app.

## When to use?
If you add a tap gesture to a primitive SwiftUI view such as `Text` or `Image`, the whole view becomes tappable. If you add a tap gesture to a container SwiftUI view, such as `VStack` or `HStack`, then SwiftUI only adds the gesture to the parts of the container that have something inside – **large parts of the stack are likely to be untappable.**

If this is what you want then the default behavior is fine. However, if you want to change the shape of **hit tests – the area that responds to taps –** then you should use the `contentShape()` modifier with the shape you want.

For example, this code creates a `VStack` containing an image, a spacer, and some text, then uses the `contentShape()` modifier to make the whole stack tappable rather than just the image and text:

```swift
VStack {
    Image(systemName: "person.circle").resizable().frame(width: 50, height: 50)
    Spacer().frame(height: 50)
    Text("Paul Hudson")
}
.contentShape(Rectangle())
.onTapGesture {
    print("Show details for user")
}
```

from: https://www.hackingwithswift.com/quick-start/swiftui/how-to-control-the-tappable-area-of-a-view-using-contentshape

https://www.hackingwithswift.com/img/books/quick-start/swiftui/how-to-control-the-tappable-area-of-a-view-using-contentshape-1~dark.mp4

# Live Activities

https://medium.com/@marwa.diab/animated-timer-in-swiftui-part-2-e1245d7ebe7f


