---
title: "Protocol in Swift"
layout: post
date: 2017-07-06 22:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Foundation
- Swift
- Protocol
star: true
category: blog
author: Bun Chu
description: Protocol in Swift
---

## Khái niệm:

* protocol /Noun /ˈprəʊtəkɒl/: A set of rules governing the exchange or transmission of data between devices. 

trong Tiếng việc có nghĩa là giao thức, định chuẩn

> A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality.

> A blueprint is a reproduction of a technical drawing, documenting an architecture or an engineering design, using a contact print process on light-sensitive sheets.

Nói một cách ngắn ngọn, protocol là giống như việc bạn định nghĩa trước hành vi, cấu trúc của một viewcontroller trong swift, rộng hơn là kiến trúc của toàn ứng dụng. 

VD: Ta có hai màn hình A và B, từ màn hình A sang màn hình B, 

## Cú pháp - Syntax:

```
    protocol MyProtocol {
        // protocol definition goes here
    }
```
It is the same as declarations of class, structure and enumeration

A protocol could have a super protocol, it means its inheribility 

```
    protocol MyProtocol, FirstProtocol, SecondProtocol {
        // protocol definition goes here
    }
```

## Rằng buộc về thuộc tính - Property Requirements .

Một protocol có thể yêu cầu đối tượng thể hiện bởi Protocol đó cung cấp một thuộc tính chỉ với một tên biến và loại dữ liệu của biến đó.

Không phân biệt nó là **thuộc tính lưu trữ(stored property)** hay **thuộc tính được tính toán(Computed property)**.

Ta có ví dụ cho hai trường hợp trên như sau:
```
protocol FullNamed {
    var firstName:String { set get }
    var lastName:String { set get }
    
    var fullName:String { get }
}


struct Person:FullNamed {
    var firstName: String
    var lastName: String
    
    var fullName: String {
        return "\(lastName) \(firstName)"
    }
}

let person = Person(firstName: "Benjamin", lastName: "Bui")

```

Ta khai báo một lớp Person mà có rằng buộc biến được khai báo từ protocol FullNamed. Với firstName và lastName là hai thuộc tính có thể gán lại được giá trị và
lấy được giá trị.

**Note:**
> error: variable with a setter must also have a getter
 
* **set** không bao giờ đứng một mình trong **protocol**, với **get** thì ngược lại, có thể đứng một mình.

## Rằng buộc về phương thức trên trong protocol - Method requirements
The following example will extend the above protocol with specific instance methods.

```
protocol FullNamed {
    var firstName:String { set get }
    var lastName:String { set get }
    
    func getFullName() -> String
}

struct Person:FullNamed {
    var firstName: String
    var lastName: String
    
    init(firstName:String,lastName:String) {
        self.firstName = firstName
        self.lastName = lastName
    }
    
    func getFullName() -> String {
        return "\(lastName) \(firstName)"
    }
}
let person = Person(firstName: "Benjamin", lastName: "Bui")
print(person.getFullName())

/* output 
 Bui Benjamin
 
 */

```

## Rằng buộc phương thức thay đổi - Mutating Method Requirements

It mean you will declare a function in struct or enumeration where you can change value of property in, because you can not change value of property in normal function.

With protocol, to do this you have to add prefix **mutating**

```
protocol FullNamed {
    var firstName:String { set get }
    var lastName:String { set get }
    
    mutating func setName(withAnotherFullname fullname:FullNamed)
}

struct Person:FullNamed {
    ...
    mutating func setName(withAnotherFullname fullname: FullNamed) {
        firstName = fullname.firstName
        lastName = fullname.lastName
    }
}
```

## Continue updating


## Protocol là một loại dữ liệu - Protocols as Types

Because it is a type, you can use a protocol in many places where other types are allowed, including:
* As a parameter type or return type in a function, method, or initializer
* As the type of a constant, variable, or property
* As the type of items in an array, dictionary, or other container

## Reference: 
1. [Protocols - Apple](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html)