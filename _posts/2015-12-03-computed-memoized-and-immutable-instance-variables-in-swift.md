---
layout: post
title: "Computed, Memoized, and Immutable Instance Variables in Swift"
description: ""
category: 
tags: []
---

__FlashcardSide__ represents either the "prompt" or "answer" side of a
flashcard:

```swift
struct FlashcardSide {
    let name: String
    let path: String

    init(name: String, path: String) {
        self.name = name
        self.path = path
    }

    func saveDropboxImageToDocuments() { ... }

    func image() -> UIImage? {
        if let Url = NSURL(string: path) {
            if let data = NSData(contentsOfURL: Url) {
                return UIImage(data: data)
            }
        }
        return nil
    }
}
```

The body of __saveDropboxImageToDocuments__ has been left out. Assume that
once it's called, image data exists on the local filesystem at __path__.

Let's instantiate the prompt (question) side of a flashcard:

```swift
// assume name and path are defined
var promptFace = FlashcardSide(
    name: name,
    path: path
)
```

A copy of the flashcard's image is nil now.

```swift
promptFace.image() // nil
promptFace.saveDropboxImageToDocuments()
promptFace.image() // <UIImage: 0x7fe613e5ed00>
```

A lot of objects start asking the __FlashcardSide__ instance for its image.

```swift
promptFace.image() // <UIImage: 0x7fb7a3624290>
```

This is bad because every call instantiates a new _UIImage_.

My first thought was to create an optional instance variable with the same name,
cache the image and conditional-code my way towards a some sort of memoization.

```swift
import UIKit

struct FlashcardSide {
    let name: String
    let path: String

    init(name: String, path: String) {
        self.name = name
        self.path = path
    }

    func saveDropboxImageToDocuments() {  }

    var image: UIImage?
    func image() -> UIImage? {
        if let nonNilImage = image {
            return nonNilImage
        } else if let Url = NSURL(string: path) {
            if let data = NSData(contentsOfURL: Url) {
                image = UIImage(data: data)
                return image
            }
        }
        return nil
    }
}
```

To which, Xcode barks __Invalid redeclaration of 'image()'__

![invalid redeclaration](/assets/images/invalid_redeclaration.png)

Now I could just change the variable name:q





In my head: "classes in Swift are references, so this should be fine" but that
was dumb. Regardless of value vs reference identity, a new instance is being
created.

method and either received __nil__ or an image representing the face of the
flashcard. A problem arose when I decided to add panning, pinch-to-zoom, and
double-tap to zoom to the image. Several objects were asking my _FlashcardSide_
instance for it's image.





I'm all for accessor methods because sometimes you need to create a seam and
couple setters and getters with actions. What I disagree with until someone
explains it well enough to me is disguising behavior as data, because that's
what computed properties are. They are accessors to a variable that are also
coupled to and perform additional behavior which is hidden from the person
writing the code through the ommission of a set of opening and closing
parenthesis:  mary.run() mary.run

Disguising se
