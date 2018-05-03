---
title: "Grouping array in Swift"
layout: post
date: 2018-05-02 22:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- foundation
- ios
- tips
- swift
star: true
category: blog
author: Bun Chu
description: There are the problem I found out when parsing data from service response.
---

```
// For exactly reference to only one value when accessing by Dictionany. Normally, the return type is the value type
class Box<A> {
    var value: A
    init(_ val: A) {
        self.value = val
    }
}

public extension Sequence {
    func group<U: Hashable>(by key: (Iterator.Element) -> U) -> [U:[Iterator.Element]] {
        var categories: [U: Box<[Iterator.Element]>] = [:]
        for element in self {
            let key = key(element)
            if case nil = categories[key]?.value.append(element) {
                categories[key] = Box([element])
            }
        }
        var result: [U: [Iterator.Element]] = Dictionary(minimumCapacity: categories.count)
        for (key,val) in categories {
            result[key] = val.value
        }
        return result
    }
}

struct People {
    let name:String
    let age:Int
}

let people = [ People(name: "Chung", age: 28), People(name: "Hieu", age: 24), People(name: "Bun Chu", age: 28) ,People(name: "Zun", age: 24)]

let groupByAge = people.group { $0.age }

```

### Referential Definition: 

1. [Sequence in Swift - Array type](https://developer.apple.com/documentation/swift/sequence)
2. [IteratorProtocol - Custom for for-in loop](https://developer.apple.com/documentation/swift/iteratorprotocol)
3. [Optional Pattern - if case](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Patterns.html)
4. [Dictionary - minimumCapacity](https://developer.apple.com/documentation/swift/dictionary/1539319-init)

