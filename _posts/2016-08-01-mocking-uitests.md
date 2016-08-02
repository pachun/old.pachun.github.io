---
layout: post
title: "TDDing Your Swift App: Injecting Dependencies"
description: ""
category: 
tags: []
---

[Here's a small
application](https://github.com/pachun/inject-dependencies-to-test-drive-swift-apps/tree/e3b9c9f4ee78dfad71a24a75c12d5bb5e4c937bb).

Here's what the existing functionality looks like:

<img src="http://i.imgur.com/opT2jnr.gif" style="width:200px">

---

The only test:

    import XCTest

    class DaysSinceTests: XCTestCase {

        func test_LaunchingTheApp_ShowsADaysSinceTable() {
            let app = XCUIApplication()
            app.launch()

            XCTAssert(app.navigationBars["Days Since"].exists)
            XCTAssertEqual(app.tables.count, 1)
            XCTAssertEqual(app.cells.count, 0)
            XCTAssert(app.buttons["Add"].exists)

            app.buttons["Add"].tap()

            XCTAssertEqual(app.cells.count, 1)
            XCTAssert(app.cells.elementBoundByIndex(0).staticTexts["0"].exists)

            app.buttons["Add"].tap()

            XCTAssertEqual(app.cells.count, 2)
            XCTAssert(app.cells.elementBoundByIndex(0).staticTexts["1"].exists)
            XCTAssert(app.cells.elementBoundByIndex(1).staticTexts["0"].exists)
        }
    }

If you can delete a
single line of code from the example app without breaking that test, I did something wrong.
[Please email me](mailto:nick@pachulski.me).


