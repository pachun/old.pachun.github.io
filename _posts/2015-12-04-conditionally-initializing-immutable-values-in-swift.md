---
layout: post
title: "Conditionally Initializing Immutable Values in Swift"
description: ""
category: 
tags: []
---

_update_:

Xcode 6.3 Beta / Swift 1.2 release notes state:

> let constants have been generalized to no longer require immediate initialization. The new rule is that a let constant must be initialized before use (like a var), and that it may only be initialized: not reassigned or mutated after initialization.

> This enables patterns like:

> {% highlight swift %}
> let x: SomeThing
> if condition {
>     x = foo()
> } else {
>     x = bar()
> }
> use(x)
> {% endhighlight %}

> which formerly required the use of a var, even though there is no mutation taking place. (16181314)

---

Immutability is great if you can swing it, but sometimes you run into tricky
situations, like conditionally initializing an immutable value at the current
scope.

{% highlight swift %}
func initializeImmutable() {
    if 1 > 2 {
        let immutable = "Swift is broken"
    } else {
        let immutable = "Swift works"
    }
    print(immutable)
}
{% endhighlight %}

Xcode barks:

> error: use of unresolved identifier 'immutable'
>    print(immutable)

Maybe you'll sacrifice immutibility.

{% highlight swift %}
func initializeImmutable() {
    var mutable: String
    if 1 > 2 {
        mutable = "Swift is broken"
    } else {
        mutable = "Swift works"
    }
    print(mutable)
}
{% endhighlight %}

Huzzah! Nope. Immutability is great and we _can_ swing it this time. There are
several ways to accomplish this with immutability, but here's what I've decided
I like:

{% highlight swift %}
func initializeImmutable() {
    let immutable = { () -> String in
        if 1 > 2 {
            return "Swift is broken"
        } else {
            return "Swift works"
        }
    }()
    print(immutable)
}
{% endhighlight %}

I like this approach because

* It solves the problem. It acheives the goal of maintaining immutability.
* It visually separates assignment and logic in a pleasing way.

---

Another way to solve the problem could look like this:

{% highlight swift %}
func initializeImmutable() {
    let immutable = newImmutable()
    print(immutable)
}

func newImmutable() -> String {
    if 1 > 2 {
        return "Swift is broken"
    } else {
        return "Swift works"
    }
}
{% endhighlight %}

I'm all for breaking things up and separation of concerns, but at a certain
point in lower level languages, you have to recognize when you're going too
crazy with it, which is why I prefer the inline closure approach.

Working with ruby a lot tempted me down this latter
path, but ruby is a much higher level language and so typically the
number of conditional-wrapping-methods in ruby will be lower than in Swift. If
you wrapped all your conditionals in methods in Swift, you'd likely end up with
an undue amount of methods leading to arguably less readable code.
