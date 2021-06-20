---
id: 98b3c847-9ce7-4cf4-9c51-288a3f0f4698
title: Rxswift
desc: ''
updated: 1624169396306
created: 1620608125114
---

## What is `BehaviorRelay`?

- It's a wrapper around `BehaviorSubject`. Means that it wraps around `BehaviorSubject` while maintaining it's replay behavior
- It can only accept values. It can't return `error` or `completed` event
- They are guaranteed to never terminate
    - Suitable to handle UI events

[RxRelay Github link](https://github.com/ReactiveX/RxSwift/blob/main/RxRelay/BehaviorRelay.swift)

## Extending class to provide Rx behavior

- To make it available through `rx` property
- Use `ReactiveCompatible` protocol
- Example by extending `KeychainWorker` class with `ReactiveCompatible`
```swift
import Foundation
import KeychainAccess
import RxSwift

class KeychainWorker {
    
    let keychain: Keychain

    init(identifier: String) {
        self.keychain = Keychain(service: identifier)
    }
    
    func set(key: String, value: String) throws {
        try keychain.set(value, key: key)
    }

    func getValue(for key: String) throws -> String? {
        try keychain.get(key)
    }
    
    func remove(key: String) throws {
        try keychain.remove(key)
    }
    
    func removeAll() throws {
        try keychain.removeAll()
    }
}

extension KeychainWorker: ReactiveCompatible {}

extension Reactive where Base: KeychainWorker {
    func set(key: String, value: String) -> Observable<Void> {
        return Observable<Void>.create({ observer in
            do {
                try self.base.set(key: key, value: value)
                observer.onCompleted()
            } catch let error {
                observer.onError(error)
            }
            
            return Disposables.create()
        })
    }
    
    func getValue(for key: String) -> Observable<String?> {
        return Observable<String?>.create({ observer in
            do {
                let value = try self.base.getValue(for: key)
                observer.onNext(value)
                observer.onCompleted()
            } catch let error {
                observer.onError(error)
            }
            
            return Disposables.create()
        })
    }
    
    func remove(key: String) -> Observable<Void> {
        return Observable<Void>.create({ observer in
            do {
                try self.base.remove(key: key)
                observer.onCompleted()
            } catch let error {
                observer.onError(error)
            }
            
            return Disposables.create()
        })
    }
    
    func removeAll() -> Observable<Void> {
        return Observable<Void>.create({ observer in
            do {
                try self.base.removeAll()
                observer.onCompleted()
            } catch let error {
                observer.onError(error)
            }
            
            return Disposables.create()
        })
    }
}
```

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

#### Code examples on how to wrap 3rd party framework that uses delegate

- `ASAuthorizationController+Rx.swift`
```swift
@available(iOS 13.0, *)
extension ASAuthorizationController: HasDelegate {}

@available(iOS 13.0, *)
class RxASAuthorizationControllerDelegateProxy: DelegateProxy<ASAuthorizationController, ASAuthorizationControllerDelegate>, DelegateProxyType, ASAuthorizationControllerDelegate {
    
    weak public private(set) var authorizationController: ASAuthorizationController?
    
    public init(authorizationController: ParentObject) {
        self.authorizationController = authorizationController
        super.init(parentObject: authorizationController, delegateProxy: RxASAuthorizationControllerDelegateProxy.self)
    }
    
    static func registerKnownImplementations() {
        register(make: { RxASAuthorizationControllerDelegateProxy(authorizationController: $0) })
    }
}

@available(iOS 13.0, *)
extension Reactive where Base: ASAuthorizationController {
    var delegate: DelegateProxy<ASAuthorizationController, ASAuthorizationControllerDelegate> {
        RxASAuthorizationControllerDelegateProxy.proxy(for: base)
    }
    
    var didCompleteWithAuthorization: Observable<ASAuthorization> {
        delegate.methodInvoked(#selector(ASAuthorizationControllerDelegate.authorizationController(controller:didCompleteWithAuthorization:)))
            .map { parameters in
                return parameters[1] as! ASAuthorization
            }
    }
}
```

- `GidSignIn+Rx.swift`
```swift
class RxGIDSignInDelegateProxy: DelegateProxy<GIDSignIn, GIDSignInDelegate>,GIDSignInDelegate  {
    public weak private(set) var gidSignIn: GIDSignIn?
    var signInSubject = PublishSubject<GIDGoogleUser>()
    var disconnectSubject = PublishSubject<GIDGoogleUser>()
    
    init(gidSignIn: ParentObject) {
        self.gidSignIn = gidSignIn
        super.init(parentObject: gidSignIn, delegateProxy: RxGIDSignInDelegateProxy.self)
    }
    
    func sign(_ signIn: GIDSignIn!, didSignInFor user: GIDGoogleUser!, withError error: Error!) {
        if let u = user {
            signInSubject.on(.next(u))
        } else if let e = error {
            signInSubject.on(.error(e))
        }
        _forwardToDelegate?.sign(signIn, didSignInFor:user, withError: error)
    }
    
    public func sign(_ signIn: GIDSignIn!, didDisconnectWith user: GIDGoogleUser!, withError error: Error!) {
        if let u = user {
            self.disconnectSubject.on(.next(u))
        } else if let e = error {
            self.disconnectSubject.on(.error(e))
        }
        self._forwardToDelegate?.sign(signIn, didDisconnectWith: user, withError: error)
    }
    
    deinit {
        signInSubject.on(.completed)
        disconnectSubject.on(.completed)
    }
}

extension RxGIDSignInDelegateProxy :DelegateProxyType {
    static func registerKnownImplementations() {
        register { RxGIDSignInDelegateProxy(gidSignIn: $0) }
    }
    
    static func currentDelegate(for object: GIDSignIn) -> GIDSignInDelegate? {
        return object.delegate
    }
    
    static func setCurrentDelegate(_ delegate: GIDSignInDelegate?, to object: GIDSignIn) {
        object.delegate = delegate
    }
}

extension Reactive where Base: GIDSignIn {
    public var delegate: DelegateProxy<GIDSignIn, GIDSignInDelegate> {
        return self.gidSignInDelegate
    }
    
    var signIn: Observable<GIDGoogleUser> {
        let proxy = self.gidSignInDelegate
        proxy.signInSubject = PublishSubject<GIDGoogleUser>()
        return proxy.signInSubject
            .asObservable()
            .do(onSubscribed: {
                proxy.gidSignIn?.signIn()
            })
            .take(1)
            .asObservable()
    }
        
    var signOut: Observable<GIDGoogleUser> {
        let proxy = self.gidSignInDelegate
        proxy.signInSubject = PublishSubject<GIDGoogleUser>()
        return proxy.disconnectSubject
            .asObservable()
            .do(onSubscribed: {
                proxy.gidSignIn?.signOut()
            })
            .take(1)
            .asObservable()
    }
    
    private var gidSignInDelegate: RxGIDSignInDelegateProxy {
        return RxGIDSignInDelegateProxy.proxy(for: base)
    }
}


extension RxGIDSignInDelegateProxy :DelegateProxyType {
    static func registerKnownImplementations() {
        register { RxGIDSignInDelegateProxy(gidSignIn: $0) }
    }
    
    static func currentDelegate(for object: GIDSignIn) -> GIDSignInDelegate? {
        return object.delegate
    }
    
    static func setCurrentDelegate(_ delegate: GIDSignInDelegate?, to object: GIDSignIn) {
        object.delegate = delegate
    }
}

extension Reactive where Base: GIDSignIn {
    public var delegate: DelegateProxy<GIDSignIn, GIDSignInDelegate> {
        return self.gidSignInDelegate
    }
    
    var signIn: Observable<GIDGoogleUser> {
        let proxy = self.gidSignInDelegate
        proxy.signInSubject = PublishSubject<GIDGoogleUser>()
        return proxy.signInSubject
            .asObservable()
            .do(onSubscribed: {
                proxy.gidSignIn?.signIn()
            })
            .take(1)
            .asObservable()
    }
        
    var signOut: Observable<GIDGoogleUser> {
        let proxy = self.gidSignInDelegate
        proxy.signInSubject = PublishSubject<GIDGoogleUser>()
        return proxy.disconnectSubject
            .asObservable()
            .do(onSubscribed: {
                proxy.gidSignIn?.signOut()
            })
            .take(1)
            .asObservable()
    }
    
    private var gidSignInDelegate: RxGIDSignInDelegateProxy {
        return RxGIDSignInDelegateProxy.proxy(for: base)
    }
}
```