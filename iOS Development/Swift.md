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