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

## Rằng buộc khởi tạo - Initializer Requirements


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


## Protocol là một loại dữ liệu - Protocols as Types

Because it is a type, you can use a protocol in many places where other types are allowed, including:
* As a parameter type or return type in a function, method, or initializer
* As the type of a constant, variable, or property
* As the type of items in an array, dictionary, or other container

## Sự ủy quyền - Delegation:

It is a design pattern that enables a class or structure to delegate some of its responsibilities to an instance of another type.

We are a example about modeling a people in love

```
protocol InLovePeople {
    var lover:String { get set }
    
    func startTheRelationship()
}

protocol InLovePeopleDelegate {
    func peopleDidGoOutTogether(people:InLovePeople)
    func peopleDidStopRelationship(people:InLovePeople)
    func peopleWillMakeWedding(people:InLovePeople)
}


class InLoveMan:InLovePeople {
    var lover: String
    
    var delegate:InLovePeopleDelegate?
    
    init(loverName:String) {
        lover = loverName
    }
    
    func startTheRelationship() {
        if lover == "Not Good" {
            delegate?.peopleDidStopRelationship(people: self)
        }
    }
}

class PeopleTracker:InLovePeopleDelegate {
    
    func peopleWillMakeWedding(people: InLovePeople) {
        print("We will be so happy!")
    }
    
    func peopleDidGoOutTogether(people: InLovePeople) {
        print("It's a good sign!")
    }
    
    func peopleDidStopRelationship(people: InLovePeople) {
        print("Poor you!")
    }
}


let man = InLoveMan(loverName: "Su Su")
let tracker = PeopleTracker()
man.delegate = tracker
man.startTheRelationship()

```

**Note:**
Because a delegate isn't required in order to do action like as its instance, the delegate property is defined as an optional 

## Thêm tương thích giao thức với một mở rộng - Adding Protocol Conformance with an Extension.

* You can extend an existing type to adopt and conform to a new protocol, even if you do not have access to the source code for the existing type. 
* Extensions can add new properties, methods, and subscripts to an existing type.

```
protocol InLovePeople {
    var lover:String { get set }
    
    func startTheRelationship()
}

extension InLoveMan: InLovePeople {
    var faceExpression: String {
        return "It look like confident and enjoyable with life"
    }
}
```

### Việc khai báo làm theo với một sự mở rộng - Declaring Protocol Adoption with an Extension.

* If a type already conforms to all of the requirements of a protocol, but has not yet stated that it adopts that protocol, you can make it adopt the protocol with an empty extension:

```
class InLoveMan {
    var lover: String
    
    var delegate:InLovePeopleDelegate?
    
    init(loverName:String) {
        lover = loverName
    }
    
    func startTheRelationship() {
        if lover == "Not Good" {
            delegate?.peopleDidStopRelationship(people: self)
        }
    }
}

class InLoveMan:InLovePeople { }
```

**Note:**
> Types do not automatically adopt a protocol. They must always explicitly declare their adoption of the protocol. 

## Việc kế thừa của giao thức - Protocol Inheritance:

A protocol can inherit one or more other protocols and can add further requirements on top of the requirements it inherits.

## Các giao thức chỉ giành cho lớp - Class-Only Protocols:

You can limit protocol adoption to class types (and not structures or enumerations) by adding the AnyObject protocol to a protocol’s inheritance list.

```
protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
    // class-only protocol definition goes here
}
```

## Sự kết hợp giữa các giao thức - Protocol Composition

It mean you can define a temporary local protocol that has the combined requirements of all protocols in the composition.

It has the form *SomeProtocol & AnotherProtocol*

```
protocol InLovePeople {
    var lover:String { get set }
    
    func startTheRelationship()
}

protocol InPassionPeople {
    var profession:String { get set }
    
    func startThePassion()
}

struct HappyPerson: InLovePeople, InPassionPeople  {
    var lover:String
    var profession:String

    ...
}

func wantToBeHappyPerson(likeThis person:InLovePeople & InPassionPeople) {
    print("Yes, I want to be like \(person.lover) \(person.profession)")
}

```

## Kiểm tra biến loại protocol - Checking for Protocol Conformance

You can use the *is* and *as* operators to check for Protocol conformance, and to cast to a special protocol.

```
let object = HappyPerson(lover:"Her",profession:"Designer")

if let workLover = object as? InPassionPeople {
    print("He is awesome!")
}

```

## Rằng buộc tùy chọn cho giao thức - Optional Protocol Requirements:
Optional requirements for protocol do not have to do implemented by types that conform to the protocol.
Opitional requirements are prefixed by the *optional* modifier as part of the protocol's definition and you can write code that interoperates with Objective-C. 
Both the protocol and the optional requirement must be marked with the *@obj* attribute.

**Note:** *@obj* protocols can be adopted only by classes that inherit from Objective-C classes or other *@obj* classes 

```
@objc protocol HappyPersonDataSource {
    @objc optional func goAhead()
    @objc optional var objective: String { get }
}

## 
```

## Protocol Extensions
Protocols can be extended to provide method and property implementations to conforming types. This allows you to define behavior on protocols themselves, rather than in each type's individual conformance or in a global function.

```
extension InLovePeople {
    func isThinkingAboutThatPerson() -> bool
}

```
### Default Implementations
You can use protocol extensions to provide a default implementation to any method or computed property requirement of that protocol.

### Adding Constraints to Protocol Extensions

When you define a protocol extension, you can specify constraints that conforming types must satisfy before the methods and properties of the extension are available. You write these constraints after the name of the protocol you’re extending using a generic **where** clause.

```
extension Collection where Iterator.Element: InLovePeople {

}
```

## Reference: 
1. [Protocols - Apple](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html)