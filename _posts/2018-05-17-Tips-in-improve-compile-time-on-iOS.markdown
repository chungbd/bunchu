---
title: "Tips in improving compile time on iOS"
layout: post
date: 2018-05-17 22:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- iOS
- Tips
- Compile time
star: true
category: blog
author: Bun Chu
description: There are the tips I found out when trying to make the compile time on iOS more quickly.
---

I feel angry and wasteful with my time while have to wait Xcode finish compiling code.

So I want to solve my issue and found out some amazing way. Usually, I take 5 minutes from scratch to complete the app building. After my customization, it's only 2 minutes.

## Fix all warning appear while building the app

It's the simple way for you and me when you don't have any thing about making it shorter.
![Markdowm Image]({{site.url}}/assets/post/2018/compile-time/compile-time1.png)


## [Build Time Analyzer for Xcode](https://github.com/RobertGummesson/BuildTimeAnalyzer-for-Xcode)

It's a good tool for code optimization. We also learn how Xcode compile code faster with clear declaration in closure, function.

![](https://raw.githubusercontent.com/RobertGummesson/BuildTimeAnalyzer-for-Xcode/master/Screenshots/screenshot.png)


## [Add SWIFT_WHOLE_MODULE_OPTIMIZATION in User Setting](https://jobs.zalando.com/tech/blog/improving-swift-compilation-times-from-12-to-2-minutes/?gh_src=4n3gxh1)

I tried this way with amazing about compile time, decrease from 4 minutes to 2 minutes. I recommend this way to you.

##  Some where you can consult their way

1. [https://github.com/fastred/Optimizing-Swift-Build-Times](https://github.com/fastred/Optimizing-Swift-Build-Times)