There is a standard template we use for our tests in Java and Swift. This helps us keep our test files organized so we can
easily keep track of the outputs and flows we are testing.

## Java
```java
// { List of imports }

public final class MyViewModelTest extends KSRobolectricTestCase {
  private MyViewModel.ViewModel vm;
  
  // { Alphabetized declaration of output test subscribers }
  
  private final TestSubscriber<Boolean> projectNameTextViewHidden = new TestSubscriber<>();
  private final TestSubscriber<String> projectNameTextViewText = new TestSubscriber<>();
  
  // { Private set up environment method at the top }
  
  private void setUpEnvironment(final @NonNull Environment environment) {
    this.vm = new MyViewModel.ViewModel(environment);
    
    // { Alphabetized subscribing of view model outputs to test subscribers }
    
    this.vm.outputs.projectNameTextViewHidden().subscribe(this.projectNameTextViewHidden);
    this.vm.outputs.projectNameTextViewText().subscribe(this.projectNameTextViewText);
  }
  
  // { Alphabetized test methods }

  @Test
  public void testProjectNameTextView() {
  
    // { Declare helpers and invoke setUpEnvironment method at top of test }
  
    final Project project = ProjectFactory.project().toBuilder().name("My Cool Project").build();
    
    setUpEnvironment(environment());
    
    // { Test script with clear and concise comments if needed }
    
    // Start the view model with a project.
    this.vm.intent(new Intent().putExtra(IntentKey.PROJECT, project));
    
    this.projectNameTextViewHidden.assertValues(false);
    this.projectNameTextViewText.assertValues(project.name);
  }
}
```


## Swift
```swift
// { Alphabetized list of imports }

internal final class MyViewModelTests: TestCase {
  fileprivate let vm: MyViewModelType = MyViewModel()

  // { Alphabetized list of output test observers }

  fileprivate let projectName = TestObserver<String, NoError>()
  fileprivate let projectNameHidden = TestObserver<Bool, NoError>()
  
  // { Override setUp() function at the top }
  
  override func setUp() {
    super.setUp()
    
    // { Alphabetized observing of view model outputs to test observers }
    
    self.vm.outputs.projectName.observe(self.projectName.observer)
    self.vm.outputs.projectNameHidden.observe(self.projectNameHidden.observer)
  }
  
  // { Alphabetized test methods }
  
  func testProjectName() {
  
    // { Declare helpers at top of test }
    
    let project = Project.template |> Project.lens.name .~ "My Cool Project"
    
    // { Test script with clear and concise comments if needed }
    
    self.vm.inputs.project(project)
    
    self.projectName.assertValues([project.name])
    self.projectNameHidden.assertValues([false])
  }
}
```
