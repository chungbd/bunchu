---
title: "Modelling state in Swift"
layout: post
date: 2017-07-12 22:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- swift
- foundation
- design pattern
star: true
category: blog
author: Bun Chu
description: how to model and deal with state
---

## Some source code of truth and improvement from it

```
class Enemy {
    var health = 10
    var isInPlay = false
}
we have to update two above properties concurrently because when *health = 0*, *isInPlay = true*
```

at somewhere in our code, we have some logic to handle:

```
func enemyDidTakeDamage() {
    if enemy.health <= 0 {
        enemy.isInPlay = false
    }
}
```
or

```
func performSpecialAttack() {
    for enemy in allEnemies {
        enemy.health = 0
    }
}
// they forgot to update isInPlay

```
There is a solution or one way of solving this problem, and to make sure that to automatically update the *isInPlay* property, using a *didSet* on the *health* property

```
class Enemy {
    var health = 10 {
        didSet { putOutOfPlayIfNeeded() }
    }

    // Important to only allow mutations of this property from within this class
    private(set) var isInPlay = true

    private func putOutOfPlayIfNeeded() {
        guard health <= 0 else {
            return
        }

        isInPlay = false
        remove()
    }
}
```
## Making states exclusive

```
struct Video {
    let url: URL
    var downloadTask: Task?
    var file: File?
    var isPlaying = false
    var progress: Double = 0
}
```

```
with above structure, detecting state of video is so complex as below example

if let downloadTask = video.downloadTask {
    // Handle download
} else if let file = video.file {
    // Perform playback
} else {
    // Uhm... what to do here? 🤔
}
```
There is a way
```
struct Video {
    enum State {
        case willDownload(from: URL)
        case downloading(task: Task)
        case playing(file: File, progress: Double)
        case paused(file: File, progress: Double)
    }

    var state: State
}
```

```
The way to improve comprehension of state for play
extension Video {
    struct PlaybackState {
        let file: File
        var progress: Double
    }
}

/// Use it in example 
case playing(PlaybackState)
case paused(PlaybackState)
```

## Applying it in reality: 

```
class VideoPlayerViewController: UIViewController {
    var video: Video {
        // Every time the video changes, we re-render
        didSet { render() }
    }

    fileprivate lazy var actionButton = UIButton()

    private func render() {
        renderActionButton()
    }

    private func renderActionButton() {
        let actionButtonImage = resolveActionButtonImage()
        actionButton.setImage(actionButtonImage, for: .normal)
    }

    private func resolveActionButtonImage() -> UIImage {
        // The image for the action button is declaratively resolved
        // directly from the video state
        switch video.state {
            // We can easily discard associated values that we don't need
            // by simply omitting them
            case .willDownload:
                return .wait
            case .downloading:
                return .cancel
            case .playing:
                return .pause
            case .paused:  
                return .play
        } 
    }

    func render() {
        renderActionButton()
        renderVideoSurface()
        renderNavigationBarButtonItems()
        ...
    }
}
```

```
private extension VideoPlayerViewController {
    func handleStateChange() {
        switch video.state {
        case .willDownload(let url):
            // Start a download task and enter the 'downloading' state
            let task = Task.download(url: url)
            task.start()
            video.state = .downloading(task: task)
        case .downloading(let task):
            // If the download task finished, start playback
            switch task.state {
            case .inProgress:
                break
            case .finished(let file):
                let playbackState = Video.PlaybackState(file: file, progress: 0)
                video.state = .playing(playbackState)
            }
        case .playing:
            player.play()
        case .paused:
            player.pause()
        }
    }
}
```

```
// This way you need to do something very specific that only affects a certain state. 

extension VideoPlayerViewController {
    override func viewDidDisappear(_ animated: Bool) {
        super.viewDidDisappear(animated)

        // Ideally, we'd like an API like this, that let's us cancel any ongoing
        // download task without having to write a huge switch statement
        video.downloadTask?.cancel()
    }
}
extension Video {
    var downloadTask: Task? {
        guard case let .downloading(task) = state else {
            return nil  
        }

        return task
    }
}
```



## Summary:
* Those solution is simple with normal knowledge about Swift. You can try on another solution as RxSwift, ReSwift or using an ELM inspired architecture.

[1]: [Modelling state in Swift - SWIFT BY SUNDELL](https://www.swiftbysundell.com/posts/modelling-state-in-swift)

