---
layout: post
title: "addUIInterruptionMonitorWithDescription Not Working"
description: ""
category: 
tags: []
---

A lot of people have said that you need to interact with the app via an
`app.tap()` in order to trigger completion of the authorization prompt:

{% highlight swift %}
addUIInterruptionMonitorWithDescription("Authorization Prompt") {
    $0.buttons["Allow"].tap()
    return true
}
app.tap()
{% endhighlight %}

If you're interacting with your app to bring up the authorization prompt, then
this works.

If your app opens an authorization prompt immediately upon launch, that approach
may seem like it's doing nothing. Try adding another `app.tap()`:

{% highlight swift %}
 addUIInterruptionMonitorWithDescription("Authorization Prompt") {
    $0.buttons["Allow"].tap()
    return true
}
app.tap()
app.tap()
{% endhighlight %}

I tried it on a hunch and it worked for me on my macbook and 5k imac.
