There is a standard template we use for our view models in both Java and Swift. This helps use not worry about stylistc 
decisions and have a well-defined place to put protocols, methods, etc.

## Java

## Swift

```swift
// {Alphabetized list of imports}
import Models
import ReactiveCocoa
import Result

internal protocol ViewModelInputs {
  // {Alphabetized list of input functions with documentation}
  
  /// Call with the project supplied to the view.
  func configureWith(project project: Project)
  
  /// Call when the view loads.
  func viewDidLoad()
}

internal protocol ViewModelOutputs {
  // {Alphabetized list of output signals with documentation}
  
  /// Emits the creator's name.
  var creatorName: Signal<String, NoError> { get }
}

internal protocol ViewModelType {
  var inputs: ViewModelInputs { get }
  var outputs: ViewModelOutputs { get }
}

internal final class ViewModel: ViewModelType, ViewModelInputs, ViewModelOutputs {

  // {Constructor of the view model at the top.}
  
  internal init() {
    // {Assign all outputs in terms of the inputs.}
   
    self.creatorName = self.projectProperty.ignoreNil().map { $0.creator.name }
  }
  
  // {Implementation of interfaces at the bottom of the view model.}
  
  // {Declaration of all input functions and the `MutableProperty`s that back them.}
  
  private let projectProperty = MutableProperty<Project?>(nil)
  internal func configureWith(project project: Project) {
    self.projectProperty.value = project
  }
  private let viewDidLoadProperty = MutableProperty()
  internal func viewDidLoad() {
    self.viewDidLoadProperty.value = ()
  }
  
  // {Declaration of all output signals}
  
  internal let creatorName: Signal<String, NoError> { get }
  
  // {Declaration of inputs/outputs}
  
  internal var inputs: ViewModelInputs { return self }
  internal var outputs: ViewModelOutputs { return self }
}

// {Private helper methods (optional) at the bottom of the file.}

private func helper() -> String {
  return "Hello"
}
```

Some notes:

* Private `MutableProperty`s back all of our inputs. 
  * If the input doesn’t take arguments you can use 
  ```swift
  private let inputProperty = MutableProperty()
  ```
  * If the input does take an argument, then you must declare the property as being generic over an optional type with its
initial value set to `nil`:
  ```swift
  private let inputProperty = MutableProperty<Project?>(nil)
  ```
  * An exception to this is when there is an acceptable default, depending on what the input means for the problem you 
  are modeling.
  ```swift
  private let inputProperty = MutableProperty<String>("")
  ```
* It is acceptable to remove new lines between input functions and mutable properties.
