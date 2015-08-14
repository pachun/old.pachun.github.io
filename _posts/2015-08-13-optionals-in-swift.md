---
layout: post
title: "optionals in swift"
description: ""
category: 
tags: []
---

A _Box_ in swift is a value that contains one of two other values. The contained
value is either nil or non-nil.

Variables in swift can't just be of type _Box_. Boxes need to define the type of
non-nil value they may contain.

A _String Box_ may contain either nil or some string value, like "hello".
An _Int Box_ may contain either nil or some integer value, like 23.
A _Person Box_ may contain either nil or some person instance. (Poke some air
holes in that box!)

**It's important to remember that a Box is not either nil or SomeType.
A Box CONTAINS either nil or SomeType.** Reread that.

Boxes are like a singular form of data encapsulation. The world sees a box, inside
of which it knows is either nothing or a string; nothing or an
integer; nothing or a person.

![String Box](/assets/images/string-box.jpg)

[The point is, you need to open the box to know what's inside of it. Until then,
it's both values at once as far as you know.](https://en.wikipedia.org/wiki/Schr%C3%B6dinger%27s_cat)

The following defines integer and string constants in Swift. Because of Swift's
steroid-consuming, super-high-intensity, type-checking compiler, these **always,
without question, absolutely must** contain a value matching the type they were
defined as.

```swift
var myInteger: Integer = 42
var myString: String = "Hello World"
```

Here's how you can define integer and string boxes.

```swift
var myIntegerBox: Integer?
var myStringBox: String?
```

You don't have to suffix the variable names with _Box_, you need the _?_. I did
that for readability.

There are a couple ways to open boxes to check what's inside of them. The first
is called a _Box Binding_.

```swift
var myStringBox: String?

if let definiteString = myStringBox {
  println("myStringBox contains a string: \(definiteString)")
} else {
  println("myStringBox contains nil")
}
```

Another (unsafe) way to open








__box = optional__

__open = unwrap__


<script type="text/javascript">

function replaceText(){
    var theDiv = document.getElementById("post-body");
    var theText = theDiv .innerHTML;

    theText = theText.replace("boxes", "optionals");
    theText = theText.replace("Boxes", "Optionals");

    theText = theText.replace("box", "optional");
    theText = theText.replace("Box", "Optional");

    theDiv.innerHTML = theText;
}
</script>
