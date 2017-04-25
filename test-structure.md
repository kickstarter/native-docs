There is a standard template we use for our tests in Java and Swift. This helps us keep our test files organized so we can
easily keep track of the outputs and scripts we are testing.

## Java
```java
// { List of imports }

public final class MyViewModelTest {
  private MyViewModel.ViewModel vm;
  
  // { Alphabetized list of output test subscribers }
  
  private final TestSubscriber<Boolean> projectNameTextViewHidden = new TestSubscriber<>();
  private final TestSubscriber<String> projectNameTextViewText = new TestSubscriber<>();
  
  // { Private set up environment method at the top. }
  
  private void setUpEnvironment(final @NonNull Environment environment) {
    this.vm = new MyViewModel.ViewModel(environment);
    
    // { Alphabetized subscriptions of view model outputs to test subscribers. }
    
    this.vm.outputs.projectNameTextViewHidden().subscribe(this.projectNameTextViewHidden);
    this.vm.outputs.projectNameTextViewText().subscribe(this.projectNameTextViewText);
  }
  
  // { Alphabetized test methods. Method names should be prefixed by `test`, followed by the specific output test case. }

  @Test
  public void testProjectNameTextView() {
  
    // { Declare helpers and invoke setUpEnvironment method at top of test. }
  
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

