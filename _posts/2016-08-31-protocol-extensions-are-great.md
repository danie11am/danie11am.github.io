## [Swift] Protocol Extensions are great

... and that's an understatement. Apple prefers to call them *magical*.

Protocol Extensions is a Swift language feature not available in Objective-C. It allow us to organise reusable code in a way that is in many cases cleaner than some other existing available options.

## Example

Declare a protocol and add a default implementation for its function.

```
protocol SomeProtocol {
    func doSomething()
} 

extension SomeProtocol {
    func doSomething() {
        print("An implementation of doing something")
    }
} 
```

Classes now get the feature simply by adopting the protocol. 

```
class SomeClass: SomeProtocol {
}

let one = SomeClass()
one.doSomething()
```


## Sample use cases

In my experience I have used protocol extensions to do the following. 

1. Showing an activity indicator in View Controller.
1. Validating text fields that are used in multiple view controllers.
1. Making a Core Data item (`NSManagedObject`) re-orderable, i.e. maintaining the value of an attribute that works as an order index.


## The alternatives

Why is using Protocol Extensions more suitable in many cases?

### Subclassing

Subclassing lets you add reusable code to the parent class so that it can be used by subclasses. 

When adding shared functionality through parent class, sometimes the added feature are not required by all the subclasses, but we don't have the time to keep refactoring, as a result parent class gets big over time. Often, we start coding or using a subclass without full knowledge of the states and behaviour of the parent classes, leading to unintended consequences.


### Extensions

With extensions you add functions to one class and they become available to all instances and subclasses.

Unlike subclassing, extensions only adds behaviour but does not add states to the shared code, which is safer compared to subclassing. However, in many cases, not all instances/subclasses of the class require the new functionality.


### Delegate Pattern

In delegate pattern you set up a variable and assign some tasks to be done by it. This is commonly used in Apple's framework. It is great but using it requires more work. You need to instantiate a delegate variable, initialise and maintain it. Often, also write callback functions for the delegate to call back. Sometimes, using protocol extensions may achieve the same goal more easily.


## Conclusion

Protocol Extensions is a clean way to write re-usable code to be shared between classes/structs/enums. Remember this option when the need arises next time.


