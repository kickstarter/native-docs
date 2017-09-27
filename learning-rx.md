The native apps at Kickstarter are built using a Model View ViewModel (MVVM) pattern. 
These ViewModels leverage Functional Reactive Programming (FRP) to allow for us to not only keep
logic out of our views, but also test our ViewModels.

We leverage FRP by using [RxJava](https://github.com/ReactiveX/RxJava) in our Android app and 
[ReactiveSwift](https://github.com/ReactiveCocoa/ReactiveSwift) in our iOS app.

## General resources
Here are some of our favorite resources for learning FRP:
* An [intro blog post by Dan Lew](http://blog.danlew.net/2017/07/27/an-introduction-to-functional-reactive-programming/)
* [RxMarbles](http://rxmarbles.com/) to help visualize different rx operators
* [Why Functional Programming Matters](http://weblog.raganwald.com/2007/03/why-why-functional-programming-matters.html) 
blog post
* [Learn You a Haskell for Great Good!](http://learnyouahaskell.com/chapters): you don't need to program in Haskell to
work on our apps, but this book is a great resource for deeper understanding of our often-used operators

## Android specific
* [FRP on Android with RxJava](http://mttkay.github.io/blog/2013/08/25/functional-reactive-programming-on-android-with-rxjava/)

## iOS specific
* [Lisa & Gina's TDD talk](https://www.youtube.com/watch?v=EpTlqx6NjYo) at Functional Swift Conference, 2016 
