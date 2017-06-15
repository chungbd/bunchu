---
title: "Injection Plugin for Xcode"
layout: post
date: 2017-05-22 19:30
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

## I - Mục đích:
![](https://raw.githubusercontent.com/johnno1962/injectionforxcode/master/documentation/images/injected.gif)

- Nó sẽ giả lập lại chế độ debug giống như trên nền tảng React Native. Nghĩa là bạn chỉ cần bấm một tổ hợp phím(ở đây là 'Ctrl + ='), toàn bộ những thay đổi của bạn trên code sẽ được cập nhập trên Simulator - phần mềm giả lập thiết bị.

## II - Cài đặt: 
Mình sẽ trình bày cho việc sử dụng Injection trên Xcode và AppCode(cái này mới thịnh hành)
### Xcode
1. Unsign Xcode: nghĩa là bạn sẽ phải tạo ra một phiên bản khác cho Xcode, để phục vụ quá trình injection.
- Tải hoặc git clone [https://github.com/johntmcintosh/xcunsign](https://github.com/johntmcintosh/xcunsign)
- Chạy trực tiếp script để tiến hành unsign.
Ví dụ: mình sử dụng phiên bản Xcode 8.2.1
![Markdowm Image]({{site.url}}/assets/post/2017/injection/unsign.png)
![Markdowm Image]({{site.url}}/assets/post/2017/injection/unsign_1.png)

2. Cài đặt Injection plugin: có 2 cách 

    A. Tải file zip hoặc clone [https://github.com/johnno1962/injectionforxcode](https://github.com/johnno1962/injectionforxcode) - Mình sử dụng cách này. 
    - Trong thư mục InjectionPluginLite, mở file .xcodeproj
    - Thực hiện build the scheme InjectionPluginLite
    
    B. Tải bản beta từ trang cá nhân của tác giả [http://johnholdsworth.com/injection.html](http://johnholdsworth.com/injection.html)

[Tham khảo](https://johntmcintosh.com/blog/2016/10/03/code-injection-ios)
### AppCode:

* Tải file jar mới nhất cho AppCode [tại đây](https://raw.githubusercontent.com/johnno1962/InjectionApp/master/InjectionAppCode/Injection.jar)
* Vào Preference -> Plugins -> Install plugin from disk -> Link to dowloaded jar file

**Note:**
- Thật sự là việc cài đặt này phụ thuộc rất nhiều vào việc cô có thương bạn ngày hôm đó không. 
- Mình đã mất một ngày ngồi chỉ để tìm cách làm sao cho nó chạy được trên AppCode và Xcode. 
- Mình khuyên các bạn sử dụng Xcode vì nó free và không gây nhiều lỗi như AppCode(thật sự là chả giá rất nhiều cho nó)

## II - Sử dụng:
* Trong một class của một View Controller bất gì của một màn hình nào đó. 

### **Cách 1:** sử dụng bên trong View Controller

```
    func injected() {
        print("I've been injected: \(self)")
    }
```
* Thực hiện built và chạy trên Simulator.
* Khi View Controller trên hiện ra. Nhấn tổ hợp phím 'Ctrl + ='. Một đoạn log sẽ được hiện ra dưới Debug area như bên dưới. Nghĩa là bạn đã cài đặt và sử dụng thành công.

![Markdowm Image]({{site.url}}/assets/post/2017/injection/unsign_2.png)

* Mỗi lần sử dụng thao tác này, code sẽ chạy vào hàm injected, thực hiện dòng lệnh nếu có thay đổi code. Thay thử vài thao tác cơ bản để thấy sự khác biệt.
```
    func injected() {
        print("I've been injected: \(self)")
        view.backgroundColor = .darkText
    }
```
![Markdowm Image]({{site.url}}/assets/post/2017/injection/unsign_3.png)
* Vào nhớ Cmd + S để save lại trạng thái trước khi thử.

### **Cách 2:** Sử dụng một view controller có nhiệm vụ điều hướng đến viewcontroller cần inject code
* Khởi tạo một navigation controller có root controller là TestingInjectionVC.
* Trong TestingInjectionVC.swift, khởi tạo một bộ quan sát với định danh là INJECTION_BUNDLE_NOTIFICATION. Khi nhận được notification từ định danh trên, navigation hiện tại sẽ push đến injected view controller.

![Markdowm Image]({{site.url}}/assets/post/2017/injection/injectionNoticationName.png)
![Markdowm Image]({{site.url}}/assets/post/2017/injection/injectionNotication.png)

**Note:**
- Với mình, cách xử lý thứ 2 hiệu quả hơn cho những màn hình cần thiết lại giao diện, theo ý đồ của mình đó là view luôn được chạy lại.
- Với cách 1, mình chỉ sử dụng khi muốn kiểm chứng với một vài chi tiết nhỏ thay đổi như màu, text conent.

## III - Sự tồn tại của Plugin này và trường phái chống lại Interface Builder
* Về công dụng, Plugin này khiến khích bạn lập trình giao diện bằng code nhiều hơn. Nó làm bạn biết sự thay đổi của từng dòng code lên giao diện trong vòng 2 giây
* Đối nghịch lại khi bạn sử dụng trên Interface Builder(một kiểu lập trình kéo thả giao diện, rất hợp với các designer muốn code giao diện), bạn sẽ phải rebuild lại ứng dụng để thấy sự thay đổi, quá trình đó thực hiện trong vòng 20 giây
* Sự đối nghịch trên càng lớn hơn với những dự án lớn nhiều màn hình và kéo dài nhiều năm. 

Một vài link để tham khảo về ưu nhược điểm khi sử dụng Interface Builder:
* [IB Free: Living Without Interface Builder and Loving It](https://www.raizlabs.com/dev/2016/08/ib-free-living-without-interface-builder/)
* [Life without Interface Builder](https://blog.zeplin.io/life-without-interface-builder-adbb009d2068)
* [Which is better for iOS apps: storyboards or programmatic development?](https://www.quora.com/Which-is-better-for-iOS-apps-storyboards-or-programmatic-development)
* [Does the Facebook iOS app use storyboards?](https://www.quora.com/Does-the-Facebook-iOS-app-use-storyboards)

## Tham khảo:

1. [https://github.com/johnno1962/injectionforxcode](https://github.com/johnno1962/injectionforxcode)
2. [https://johntmcintosh.com/blog/2016/10/03/code-injection-ios](https://johntmcintosh.com/blog/2016/10/03/code-injection-ios)
3. [https://johntmcintosh.com/blog/2016/09/30/xcode8-extensions](https://johntmcintosh.com/blog/2016/09/30/xcode8-extensions)
4. [https://github.com/johnno1962/InjectionApp](https://github.com/johnno1962/InjectionApp)
5. [http://johnholdsworth.com/injection.html](http://johnholdsworth.com/injection.html)