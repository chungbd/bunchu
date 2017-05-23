---
title: "Protocol in Swift"
layout: post
date: 2017-05-27 22:00
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

VD: tại màn hình khởi tạo Splash đầu tiên, sẽ lấy thông tin A tại local, dùng thông tin đấy để quyết định sang màn hình B hoặc C. Nếu thiếu thông tin A thì app sẽ request lên service để lấy…

## Syntax: