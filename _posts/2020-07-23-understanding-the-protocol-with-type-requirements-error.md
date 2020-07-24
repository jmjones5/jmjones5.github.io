---
layout: post
title: Understanding the protocol with type requirements error
categories: [Swift, Generics]
---

So you're probably here because of this error: `protocol 'CustomProtocol' can only be used as a generic constraint because it has Self or associated type requirements`. Or maybe you're just here because you've had this issue before and if you haven't I'm sure you'll hit it eventually. For the longest time I didn't have much understanding of what this error message was telling me. This left me investigating a problem for far longer than I probably should have, so the goal here is to understand what this error is and hopefully get on the road to fixing it faster.

> TLDR: The protocol uses an [associated type](#associated-types) or the [Self type](#self-type) within it's definition meaning you can't use it as a standalone type like you would with other protocols.

## Breaking it down

The best place to start is to break up the error message into its three parts and try to understand them individually. We have an expressed limitation `can only be used as a generic constraint` and two possible causes `Self requirements` or `associated type requirements`.

## Associated Types

Lets start with the potential causes of this error message. To understand both of these you need to have an understanding of what `Associated Types` are. A good starting place to get a grasp of what `Associated Types` are is Paul Hudson's bitesize article [What is a protocol associated type?](https://www.hackingwithswift.com/example-code/language/what-is-a-protocol-associated-type), but to summarise, associated types are a way to make a protocol generic. Meaning we can specify types on conformance rather than defining it within the protocol. Swift can infer types on protocol conformance too, so although you may not have explicitly given a type an associated type may still exist like so:

```swift
// swift 5.1
protocol CustomProtocol {
    associatedtype T
    func description(for item: T) -> String
}

struct ConformingType: CustomProtocol {
    func description(for item: Int) -> String {
        "\(item)"
    }
}
```

In the above example, the associated type `T` has been inferred by the compiler because we've specified `Int` in the description function parameters.

Here we begin to understand why this error can be hard to interpret in the cases when you're conforming to a protocol and it's inferring associated types.

## Self Type

We've covered the case where you'd have `associated type requirement`, now what about the `Self requirement`? Well this one is a lot more straight forward to explain, basically with all protocols you have the ability to reference the conforming type's type. Here's an example:

```swift
// swift 5.1
protocol CustomProtocol {
    func appendedDescription(for item: Self) -> String
}

extension Int: CustomProtocol {
    func appendedDescription(for item: Self) -> String {
        "\(self), \(item)"
    }
}
```

In the above example you can see that we've created a protocol where the parameter type signature is whatever type the conforming type is. You can think of this like another associated type, just one that's implicitly created for your use.

## What are generic constraints?

Now we've cleared up what the `Self or associated type requirements` are, let's understand what the limitation `can only be used as a generic constraint` means when using these language features. 

Generic constraints is anywhere where you add a requirement on a generic type. A requirement could be inheriting from a specific class, conforming to a particular protocol or protocol composition. To clear this up a little lets look at some examples of generic constraints.

Generic function with generic constraints:
```swift
// swift 5.1
func verify<T: Equatable>(for lhs: T, rhs: T) -> Bool
```

In the above, you can see that type `T` is constrained to conforming to `Equatable` so a type that does not conform to `Equatable` when used as a parameter to this function will throw a compile time error.

Protocol extensions with generic constraints:
```swift
// swift 5.1
extension CustomProtocol where Self == Int {
func appendedDescription(for item: Self) -> String
}
```

Swift allows you to extend protocols, aka protocol oriented programming. However, you can go a step further with generic constraints to only extend conforming types that meet those constraints. As shown in the above example, we only add the function `appendedDescription` to `Int`'s which conform to `CustomProtocol`. Pretty neat. 😎

Protocol associated type with generic constraints:
```swift
// swift 5.1
protocol CustomProtocol {
    associatedtype T: Equatable
	    mutating func append(_ item: T)
}
```

Another use case for generic constraints is a constraint on a `associated type` like in the example above. This will throw a compile time error when a conforming type does not conform to `Equatable`.

As swift goes further down this road we get more and more places [where generic constraints can be used](https://github.com/apple/swift-evolution/blob/master/proposals/0267-where-on-contextually-generic.md). If you're interested in digging into what you can do with generic constraints you can take a look at the [swift docs](https://docs.swift.org/swift-book/LanguageGuide/Generics.html).

## Why can’t we use it like a normal protocol?

So now we understand what the message is telling us, why can't we use a protocol with an associated type like a normal protocol? The short answer is because the compiler just doesn't support it _yet_. However, it does look like it’s on the horizon for this to be implemented.

The issue comes down to the compiler not being able to understand what the type is because there is missing type information, which is only provided when you know what the generic type’s concrete type is.

If you'd like to dig in and try to understand protocols with associated types further, I recommend watching [Alex Gallagher’s talk](https://www.youtube.com/watch?v=XWoNjiSPqI8).

