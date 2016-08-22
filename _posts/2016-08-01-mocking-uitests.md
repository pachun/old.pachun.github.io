---
layout: post
title: "TDDing Your Swift App: Injecting Dependencies in UITests"
description: ""
category: 
tags: []
---

[Here's a gif of what a tiny iOS app does](http://i.imgur.com/opT2jnr.gif).

...[here's the code](https://github.com/pachun/inject-dependencies-to-test-drive-swift-apps/tree/e3b9c9f4ee78dfad71a24a75c12d5bb5e4c937bb).

... [and here's the only test](https://github.com/pachun/inject-dependencies-to-test-drive-swift-apps/blob/e3b9c9f4ee78dfad71a24a75c12d5bb5e4c937bb/inject-dependencies-to-test-drive-swift-appsUITests/DaysSinceTests.swift).

If you can delete a single line of code from the app without breaking
the test, I did something wrong. [Please email me](mailto:nick@pachulski.me).

# As a user

# when I tap the plus button

# I only want to see 10 most recent rows

```swift
let app = XCUIApplication()
app.launch()

XCTAssertEqual(app.cells.count, 0)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 1)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 2)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 3)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 4)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 5)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 6)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 7)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 8)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 9)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 10)
XCTAssert(app.cells.elementBoundByIndex(9).staticTexts["0"].exists)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 10)
XCTAssert(app.cells.elementBoundByIndex(9).staticTexts["1"].exists)
```

[UITests are very slow and this test takes over 18 seconds to run on my
machine](https://github.com/pachun/inject-dependencies-to-test-drive-swift-apps/blob/52eace7e3bbc8b3ccf5c30a22bcb61bdbf2d5b41/inject-dependencies-to-test-drive-swift-appsUITests/DaysSinceTests.swift).

![slow-uitest](http://i.imgur.com/wOMexgK.png)

# There's a better way.

```swift
let app = XCUIApplication()
app.launch()

XCTAssertEqual(app.cells.count, 0)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 1)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 2)
XCTAssert(app.cells.elementBoundByIndex(1).staticTexts["0"].exists)
app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 2)
XCTAssert(app.cells.elementBoundByIndex(1).staticTexts["1"].exists)
```

This test runs in under 9 seconds on my machine:

![less-slow-uitest](http://i.imgur.com/TirQs0l.png)

# It's OK

It's ok that we changed the max number of rows shown from 10 to 2 for our test.
We can test:

1. the table view works properly with a max row size of 2
2. the table view has a max row size of 10 when the UI tests aren't running

```swift
let app = XCUIApplication()
app.launch()

// tap the plus button 9 times

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 10)
XCTAssert(app.cells.elementBoundByIndex(9).staticTexts["0"].exists)

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 10)
XCTAssert(app.cells.elementBoundByIndex(9).staticTexts["1"].exists)
```

```swift
let app = XCUIApplication()
app.launch()

// tap the plus button once

app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 2)
XCTAssert(app.cells.elementBoundByIndex(1).staticTexts["0"].exists)
app.buttons["Add"].tap()
XCTAssertEqual(app.cells.count, 2)
XCTAssert(app.cells.elementBoundByIndex(1).staticTexts["1"].exists)
```

They're the same.

Because the test for the behavior of the table view is the same whether the max
number of rows shown is 2 or 10, we can test the behavior works correctly in the
UITest given there's 2 rows (to save time) and elsewhere test that the actual
number of rows shown (not while uitesting) is 10.

# Make the UITest pass

```swift
func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    if daysSinceArray.count > 2 {
        return 2
    } else {
        return daysSinceArray.count
    }
}
```





* we can make it pass with ... code block ...
* yes, i changed the requirements to speed up the test and now 
the true requirement isn't being validated.

# Quick Aside

Just because all the tests are passing does not mean your feature is done. Tests
validate the behavior you've defined in them. If you haven't yet defined all the
behavior needed to complete your user story in a test, you're not done with your
feature.

All the tests passed when you cloned the repository before you made any changes.
This argument is bad and I'm tired of hearing it.

