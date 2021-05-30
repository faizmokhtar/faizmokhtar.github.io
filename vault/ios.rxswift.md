---
id: 98b3c847-9ce7-4cf4-9c51-288a3f0f4698
title: Rxswift
desc: ''
updated: 1622363588057
created: 1620608125114
---

## What is `BehaviorRelay`?

- It's a wrapper around `BehaviorSubject`. Means that it wraps around `BehaviorSubject` while maintaining it's replay behavior
- It can only accept values. It can't return `error` or `completed` event
- They are guaranteed to never terminate
    - Suitable to handle UI events

[RxRelay Github link](https://github.com/ReactiveX/RxSwift/blob/main/RxRelay/BehaviorRelay.swift)

## RxSwift and delegate pattern

- Checkout `Reactive.swift`, `DelegateProxy.swift` & `DelegateProxyType.swift` files
- `DelegateProxy` object creates a fake delegate object which will proxy all the data received into dedicated observables
    - create a proxy to drive all data from the delegate to observables
    - map 1-to-1 relationship, single protocol will correspond to a single observable that returns the given data

### Delegate pattern without return type
- Use `sentMessage(Selector)` or `methodInvoked(Selector)`

### Delegate pattern with return type
- Wrapping a delegate with return type are very hard task
    - Delegate methods with a return type are not meant for observation, but customization of the behavior.
    - Defining an automatic default value that would work in any case is a non-trivial task.
    - Best solution is to forward the call to a classic implementation of the delegate
- Use `public static func installForwardDelegate(_ forwardDelegate: AnyObject, retainDelegate: Bool, onProxyForObject object: AnyObject) -> Disposable`
