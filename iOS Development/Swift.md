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