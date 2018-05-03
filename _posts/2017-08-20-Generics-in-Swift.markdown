---
title: "Generics in Swift"
layout: post
date: 2017-08-20 22:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- foundation
- ios
- generics
- swift
star: true
category: blog
author: Bun Chu
description: Generics is a abstract concept, but it's useful when design the structure of the project.
---

```
Generic code enables you to write flexible, reusable functions and types that can work with any type, subject to requirements that you define. You can write code that avoids duplication and expresses its intent in a clear, abstracted manner.
```

## Introduction

There are some significant notes from me while I am reading the book **Clean Code** of the author *Robert C.Martin*

##  Meaningfull Names

### Use Intention-Revealing Names

```
Bad Code

int d; //elapsed time in days

```

```
Good Code

int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```



## Summary:
* Those solution is simple with normal knowledge about Swift. You can try on another solution as RxSwift, ReSwift or using an ELM inspired architecture.

[1]: [Modelling state in Swift - SWIFT BY SUNDELL](https://www.swiftbysundell.com/posts/modelling-state-in-swift)

