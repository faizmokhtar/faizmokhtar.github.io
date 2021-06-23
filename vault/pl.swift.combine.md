---
id: mQd3_nJM_vaiFgMITkYYb
title: Combine
desc: ''
updated: 1624462319093
created: 1624461757352
---

- declarative, reactive framework for processing asynchronous events over time
- 3 important features in Combine
  - publishers
  - operators
  - subscribers

![Combine basic flow](/assets/images/2021-06-23-23-30-57.png)
--- 

# Publishers
- type that can emit values over time (eg. to subscriber)
- can emit multiple events of these types
  - output value of generic `Output` type
  - successful completion
  - completion with an error of the publisher's `Failure` type
- come with error handling built in

- `Publisher` protocol is generic over 2 types
  - `Publisher.Output`: type of the output values of the publisher
  - `Publisher.Failure`: type of error the publisher can throw if it fails
- do not emit any values if there are no subscribers to receive the output

- emits 2 kinds of events
  - values @ elements
  - completion event
- can emit 0 or more values but only 1 completion event
- once publisher emits completion event, it's finished and can no longer emit any values.

---

#Operators
- methods declared on the `Publisher` protocol that return either the same or a new publisher
- highly decoupled and composable. So it can be combie together to implement complex logic over the execution of a single subscription
- always have input and output.
  - input; known as **upstream**
  - output; known as **downstream**

collect()
- collect values into array
- provide an easier way to transform stream of individual values from a publisher into array of those values

map(_:)
- similar as Swift's standard `map` except it operates on values emitted from a publisher

tryMap(_:)
- counterpart try operator that will take a closure that can throw an error
- will emit error downstream

flatmap()
- use to flatten multiple upstream publishers into a single downstream publisher
- basically flattens the output from all received publishers into a single publisher
- to help manage memory footprint we can specify `.flatMap(maxPublishers:)`
- default number of upstream publishers is `.unlimited`

replaceNil(with:)
- receive optional value and replace `nil` with the value specified
- basically replace `nil` value with non-`nil` value

replaceEmpty(with:)
- insert a value if a publisher completes without emitting a value

---

#Subscribers
- every subbscription ends with a subscriber
- generally do something with the emitted output or completion events
- Combine provide 2 built-in subscribers:
  - **sink**; allows us to provide closure with our code that will receive output values and completions
  - **assign**; allows us to bind the resulting output to some property on our data model or on a UI control
- we can create custom subscriber

---

#Unanswered questions?

1. What is backpressure management?
2. What is `Future`?
3. How to create `Future`?
3. What is `Subject`?
4. What is Type erasure?