---
title: "Application Lifecycle in iOS"
layout: post
date: 2017-06-16 22:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Foundation
- Swift
- Lifecycle
star: true
category: blog
author: Bun Chu
description: Application Lifecycle in iOS
---

## Loading Application
1. The user has just turned on his phone, and no applications are running expect for those that belong to the operating system.
2. Your application is **not running.** 
3. After the user taps your app's icon, **Springboard** - the part of the OS that operates the Home screen of iOS - launches your app.
4. Your app, and the shared libraries it needs to execute, is loaded into memory while Springboard animates your *Default.png* on the screen
5. Eventually, your app begins execution, and your *application delegate* receives the appropriate notification.
6. When your application is running and in the foreground, **it is in the active state.**

## Stopping Application
1. On iOS, users tend to only use any given application for a few seconds before returning their phones to their pockets.
- After the user has put away your app by pressing the Home button on her iPhone or iPad, your application enters **the background state**
- Typically, apps have 10 seconds to complete any database saves or other long-running tasks(though applications can request additional time from the OS).
2. When all the background processing is complete, the application finally becomes **suspended state**.
- While **suspended**, applications remain in memory but may not execute code. 
- The state of your application is persisted. 
- If the user opens your application while it is suspended, it begins execution exactly where it left off.
3. If memory becomes low, the OS can kill your app while it is in the suspended state. The user can also manually terminal your app from multitasking tray.
- Once terminated, applications **return to their initial state of not running.**

## Awaking Application
1. If the user receives a calendar alert, opens the multitasking tray, or gets a phone call, your application can be put into **the inactive state.**
2. The user can open your application without tapping its icon on the Home screen.
- If your application receives local or push notifications, or if it is registered for custom URL scheme handing, the user can open it in any number of ways.

## Reference
1. [Chater 1 - iOS UICollectionView - The complete guide, 2nd Edition]()