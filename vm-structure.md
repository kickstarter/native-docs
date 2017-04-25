There is a standard template we use for our view models in both Java and Swift. This helps us not worry about stylistc 
decisions and have a well-defined place to put protocols, methods, interfaces, etc.

## Java
```java
// {List of imports}

public interface MyViewModel {

  interface Inputs {
    /** {Alphabetized list of input functions with documentation} */
    
    /** Call to configure the view model with a project. */
    void configureWith(Project project);

    /** Call when the next page has been invoked. */
    void nextPage();
  }
  
  interface Outputs {
    /** {Alphabetized list of output observables with documentation} */
  
    /** Emits the creator name to be displayed. */
    Observable<String> creatorNameTextViewText();
  }
  
  final class ViewModel extends ActivityViewModel<MyActivity> implements Inputs, Outputs {
  
    // {Constructor of the view model at the top.}
    
    public ViewModel(final @NonNull Environment environment) { 
      super(environment);
       
      this.creatorNameTextViewText = this.project.map(p -> p.creator.name);
    }
    
    // {Private helper methods (optional) before implementation of interfaces.}
    // {Private helpers should be `static`; they should be passed all the data they need to do work}
    
    private static @NonNull String helper() {
      return "Hello";
    }
    
    // {Implementation of interfaces at the bottom of the view model.}
  
    // {Alphabetized declaration of all input `PublishSubject`s.}
    
    private final PublishSubject<Void> nextPage = PublishSubject.create();
    private final PublishSubject<Project> project = PublishSubject.create();
    
    // {Alphabetized declaration of all output observables.}
    
    private final Observable<String> creatorNameTextViewText;
    
    // {Declaration of inputs/outputs}
    
    public final Inputs inputs = this;
    public final Outputs outputs = this;
    
    // {Alphabetized declaration of all input functions.}
    
    @Override public void configureWith(final @NonNull Project project) {
      this.project.onNext(project);
    }
    @Override public void nextPage() {
      this.nextPage.onNext(null);
    }

    // {Alphabetized declaration of all output functions.}
    
    @Override public @NonNull Observable<String> creatorNameTextViewText() {
      return this.creatorNameTextViewText;
    }
  }
}
```

Some notes:

* The ViewModel for an activity should be the name of the activity appended with ViewModel, e.g. `MessagesActivity` has a `MessagesViewModel`
* The ViewModel for a view holder should be the name of the view holder appended with HolderViewModel, e.g. `MessageViewHolder` has a `MessageHolderViewModel`
* It is acceptable to remove new lines between input and output functions.
* Always use `this` when refering to instance variables.


## Swift

```swift
// {Alphabetized list of imports}
import Models
import ReactiveCocoa
import Result

public protocol ViewModelInputs {
  // {Alphabetized list of input functions with documentation}
  
  /// Call with the project supplied to the view.
  func configureWith(project project: Project)
  
  /// Call when the view loads.
  func viewDidLoad()
}

public protocol ViewModelOutputs {
  // {Alphabetized list of output signals with documentation}
  
  /// Emits the creator's name.
  var creatorName: Signal<String, NoError> { get }
}

public protocol ViewModelType {
  var inputs: ViewModelInputs { get }
  var outputs: ViewModelOutputs { get }
}

public final class ViewModel: ViewModelType, ViewModelInputs, ViewModelOutputs {

  // {Constructor of the view model at the top.}
  
  public init() {
    // {Assign all outputs in terms of the inputs.}
   
    self.creatorName = self.projectProperty.ignoreNil().map { $0.creator.name }
  }
  
  // {Implementation of interfaces at the bottom of the view model.}
  
  // {Alphabetized declaration of all input functions and the `MutableProperty`s that back them.}
  
  private let projectProperty = MutableProperty<Project?>(nil)
  public func configureWith(project project: Project) {
    self.projectProperty.value = project
  }
  private let viewDidLoadProperty = MutableProperty()
  public func viewDidLoad() {
    self.viewDidLoadProperty.value = ()
  }
  
  // {Alphabetized declaration of all output signals}
  
  public let creatorName: Signal<String, NoError> { get }
  
  // {Declaration of inputs/outputs}
  
  public var inputs: ViewModelInputs { return self }
  public var outputs: ViewModelOutputs { return self }
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
  private let inputProperty = MutableProperty("")
  ```
* It is acceptable to remove new lines between input functions and mutable properties.

* Always use `self` when refering to instance variables.
