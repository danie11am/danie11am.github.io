## [Swift] Protocol Extensions are great

... and that's an understatement. Apple prefers to call them *magical*.

Here is the take away: Use Protocol Extensions to share code between classes. 

Why? Because with Protocol Extensions you can create reuseable components *cleanly*. And read-on, you will see why it's better than some other ways.

It is all about going from this sad picture:

{<1>}![alt](/blog/content/images/2016/08/Screen-Shot-2016-08-31-at-2-32-32-PM.png)

to the happier picture below:

{<2>}![alt](/blog/content/images/2016/08/Screen-Shot-2016-08-31-at-2-34-32-PM.png)

## Quick Example

Creating a Protocol Extension is simple. First you write a protocol,

```
protocol ProtocolOne {
    func doOneThing()
} 
```

Then you create an extension that extends the protocol, 

```
extension ProtocolOne {
    func doOneThing() {
        print("Do One Thing from Protocol One")
    }
} 
```

The `doOneThing()` function here is known as the Default Implementation, because your adopting classes can call it straight away, without implementing the function itself.

That's it! That's a Protocol Extension. Here is how you use it:

```
class ClassOne: ProtocolOne {
}

let one = ClassOne()
one.doOneThing()  // <-- Magic!
```

Here, the ClassOne instance can call `doOneThing()` as if the function has been declared in the class, just because ClassOne has adopted the `ProtocolOne` protocol. This re-use is *clean*.

## The Alternatives

*"What is the big deal?"* I heard you say.

*"I have done it all before with subclassing, extensions and delegate pattern."* you say.

### Subclassing

Often started like this:

{<3>}![alt](/blog/content/images/2016/08/Subclassing---1.png)

Ended up like this:

{<4>}![alt](/blog/content/images/2016/08/Subclassing---2.png)

Does that remind you of... View Controller?

### Extensions

With extensions you add functions to one class,

{<5>}![alt](/blog/content/images/2016/08/Extensions---1.png)

And it impacts all its subclasses. 

{<6>}![alt](/blog/content/images/2016/08/Extensions---2.png)

Do all of them *really* need it? Do all of them know what they are getting? No! And it's not clean.

### Delegate Pattern

Delegate pattern is commonly used in Apple's framework. It's great but not without some downsides:

- You need to instantiate a variable.
- And initialise it.
- And create some callback functions in the classes that need the delegate.
- And calling those callback functions at the right time.

## Sample Use Cases

In my experience I have used Protocol Extensions to do the following. 

1. Showing an activity indicator in View Controller.
1. Validating text fields that are used in multiple view controllers.
1. Making a Core Data item (`NSManagedObject`) re-orderable, i.e. maintaining the value of an attribute that works as an order index.

## Take Home

Use Protocol Extensions to share code between classes. Or structs. Or enums.

