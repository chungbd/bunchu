---
title: "Injection Plugin for Xcode"
layout: post
date: 2017-05-18 22:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- injection
- xcode
- auto-layout
category: blog
author: bunchu
description: Injection in Xcode
# jemoji: '<img class="emoji" title=":ramen:" alt=":ramen:" src="https://assets.github.com/images/icons/emoji/unicode/1f35c.png" height="20" width="20" align="absmiddle">'
---

## Mục đích:

- Nó sẽ giả lập lại chế độ debug giống như trên nền tảng React Native. Nghĩa là bạn chỉ cần bấm một tổ hợp phím(ở đây là 'Ctrl + ='), toàn bộ những thay đổi của bạn trên code sẽ được cập nhập trên Simulator - phần mềm giả lập thiết bị.

## Cài đặt: 
Mình sẽ trình bày cho việc sử dụng Injection trên Xcode và AppCode(cái này mới thịnh hành)
### Xcode
1. Unsign Xcode: nghĩa là bạn sẽ phải tạo ra một phiên bản khác cho Xcode, để phục vụ quá trình injection.
- Tải hoặc git clone [https://github.com/johntmcintosh/xcunsign](https://github.com/johntmcintosh/xcunsign)
- chạy trực tiếp script để tiến hành unsign.
Ví dụ: mình sử dụng phiên bản Xcode 8.2.1
![Markdowm Image]({{site.url}}/assets/post/2017/injection/unsign.png)

2. Cài đặt Injection plugin: có 2 cách 

### AppCode:
## Tham khảo:

1. [https://github.com/johnno1962/injectionforxcode](https://github.com/johnno1962/injectionforxcode)
2. [https://johntmcintosh.com/blog/2016/10/03/code-injection-ios](https://johntmcintosh.com/blog/2016/10/03/code-injection-ios)
3. [https://johntmcintosh.com/blog/2016/09/30/xcode8-extensions](https://johntmcintosh.com/blog/2016/09/30/xcode8-extensions)

4. [https://github.com/johnno1962/InjectionApp](https://github.com/johnno1962/InjectionApp)