
When writing inputs and outputs in both Java and Swift view models, there is a convention of what types the member variables should be used to express our outputs declaratively.

## Java
All inputs member variables are `PublishSubject` types that are invoked directly when needed from the view. The input setter function will ping its appropriate member variable with the appropriate input parameter and have a `void` return type.

```java
private final PublishSubject<Project> projectClicked = PublishSubject.create();

@Override public void projectClicked(final @NonNull Project project) {  
    projectClicked.onNext(project);
};
```

An output member variable may be a `BehaviorSubject` or `Observable`, depending on the action of the output. The public getter will always be an `Observable`.

For one-off actions that have no inverse, such as showing a UI element or starting a new activity, use `Observable`.
```java
private final Observable<Project> showProject;

@Override public Observable<Project> showProject() { return showProject; }
```

Directly assign the appropriate matching input to that output `Observable`. This allows for us to test the interaction and have concise, declarative code. 
```java 
showProject = projectClicked;
```

For outputs that require more logic, use `BehaviorSubject`.
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