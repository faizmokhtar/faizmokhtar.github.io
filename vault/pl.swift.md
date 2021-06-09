---
id: fa931ef7-fdfd-4ae8-a185-3911c958b82a
title: Swift
desc: ''
updated: 1622534937877
created: 1622534819456
---

# Cheatsheet

- To check if all array elements satisfy a certain condition

```swift
let conditions = [true, true, false, true]
let isAllTrue = conditions.isAllSatisfy { $0 == true } // false
```