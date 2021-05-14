---
id: 98b3c847-9ce7-4cf4-9c51-288a3f0f4698
title: Rxswift
desc: ''
updated: 1620608715534
created: 1620608125114
---

## What is `BehaviorRelay`?

- It's a wrapper around `BehaviorSubject`. Means that it wraps around `BehaviorSubject` while maintaining it's replay behavior
- It can only accept values. It can't return `error` or `completed` event
- They are guaranteed to never terminate
    - Suitable to handle UI events

[RxRelay Github link](https://github.com/ReactiveX/RxSwift/blob/main/RxRelay/BehaviorRelay.swift)