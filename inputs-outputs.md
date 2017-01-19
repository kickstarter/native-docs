
There is a convention of what types our inputs and outputs should be, and how they should be named, in both Java and Swift view models.

## Java
### Inputs
All input variables are `PublishSubject` types that are invoked directly when needed from the view. The input setter function will ping its appropriate member variable with the appropriate input parameter and have a `void` return type.

```java
private final PublishSubject<Project> projectClicked = PublishSubject.create();

@Override public void projectClicked(final @NonNull Project project) {  
    projectClicked.onNext(project);
};
```
Note that the input variable and its function have the same name.

### Outputs
An output variable may be a `BehaviorSubject` or `Observable`, depending on the action of the output. The public getter will always be an `Observable`.

We are still learning about when, exactly, `Observable` can be used in place of `BehaviorSubject`. Ideally, all outputs can be `Observable`s, but there are edge cases which require `BehaviorSubject` outputs.

For one-off actions that have no inverse, such as showing a UI element or starting a new activity, use `Observable`.
```java
private final Observable<Project> showProject;

@Override public Observable<Project> showProject() { return showProject; }
```

Note that the output variable and its function have the same name.

Directly assign the appropriate matching input to that output `Observable`. This allows for us to test the interaction and have concise, declarative code. 
```java 
showProject = projectClicked;
```

For outputs that require more complex logic, use `BehaviorSubject`.
```java
private final BehaviorSubject<List<Project>> showRecommendations = BehaviorSubject.create();

@Override public Observable<List<Project>> showRecommendations() {
    return showRecommendations;
}
```

```java
project
  .flatMap(this::relatedProjects)
  .compose(zipPair(rootCategory))
  .compose(bindToLifecycle())
  .subscribe(showRecommendations::onNext);
```


## Swift
### Inputs
All input variables `MutableProperty` types that are invoked directly when needed from the view. The input setter function will set its appropriate member variable with the appropriate input parameter.

```swift
fileprivate let projectTappedProperty = MutableProperty<Project?>(nil)
public func projectTapped(_ project: Project) {
    self.projectTappedProperty.value = project
}
```
Note that the input variable is named with a `Property` suffix to clarify that it is a `MutableProperty`. Its function is the name of the action.

If an input value is void, use the following syntax:

```swift
fileprivate let viewDidLoadProperty = MutableProperty()
public func viewDidLoad() {
    self.viewDidLoadProperty.value = ()
}
```

### Outputs
All output variables are `Signal` types with `NoError` as the second error parameter.

```swift
public let goToDiscovery: Signal<DiscoveryParams, NoError>
```

Note that Xcode will display an error on your output until it is instantiated in the view model's `init()`. You may set the output to `.empty` if you wish to continue without implementing, i.e. for testing purposes.

```swift
public let goToDiscovery: Signal<DiscoveryParams, NoError> = .empty
```

## Naming conventions
Use clear, concise, and literal names for all inputs and outputs. For example, if a button tap is your input, name your input `buttonTapped`. If the output is an error message that displays a dialog, name your output `showErrorMessageDialog`. Ideally, the view should be able to infer directly via variable name what 1. triggers the input action and 2. what an output should do. There should be as little inferred knowledge as possible in regards to what an input and an output do.
