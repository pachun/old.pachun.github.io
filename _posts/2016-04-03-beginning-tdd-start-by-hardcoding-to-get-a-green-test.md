---
layout: post
title: "Beginning TDD: Start by Hardcoding to get a Green Test"
description: ""
category: 
tags: []
---

Write a test which contrives some state of the world and then makes an
assertion. We'll be test driving an even number function in psuedo code as an
example:

```ruby
it "returns true when passed an even number" do
  is_even = even?(2)
  expect(is_even).to be(true)
end
```

* We're testing the `even?` function
* The state of the world is the `2` you're passing into it

---

Now, hardcode your test to return the value you expected to be returned, given
your contrived state of the world (`2`):

```ruby
def is_even?(num)
  return true
end
```

Yes, your code isn't calculating a return value contingent on your contrived
state of the world. Yes, at this point your test is a tautology.

But that's OK! Not only is it OK, it's **good**.

It's OK because harding the response is the easiest way to make your test go
green and smart people do the easiest thing to get the job done.

That's OK because every state of the world is a super set of your contrived state
of the world. Obviously this is why your test is green.

It's **good** because you haven't defined any other expected behavior in
another test.

If you expect new outcomes given new input you have to say so in
a test. **Otherwise, you'll end up with a green test suite and a broken code
base because you made an untested assumption about the code. Assumptions need to be
validated routinely.**

My point is that if, given this is our only test:

```ruby
it "returns true when passed an even number" do
  is_even = even?(2)
  expect(is_even).to be(true)
end
```

and we write this code to make it pass:

```ruby
def is_even?(num)
  (num % 2) == 0
end
```

Then we're in trouble because six months from now, after the name of this function has
been changed and 10 new people are maintaining the code, it looks totally
different. Different to the point where its purpose is no longer obvious. Its
name isn't `is_even?()` anymore and it's 20 lines long now.

Diane comes along, with the perfectly valid intention of adding some new great
feature to our app. In the process of doing so, she TDD's her feature and
changes what used to be named our `is_even?()` function to have this line at the
bottom:

```ruby
return true
```

She runs the test and they all pass. However, back when you added
your feature without properly driving it out with tests, you added code
contingent upon the assumption that this function returned false when passed an
odd number:

```ruby
def qualifies_for_refund?(customer)
  return (
    customer.is_gold_member? ||
    is_even?(customer.some_statistic)
  )
end
```

Because you wrote code contingent upon an untested assumption, Diane was able to
invalidate your assumption even though she properly test drove her feature. Now
every customer qualifies for a refund regardless of the world's state. Big bug.

Obviously, you will fall into this trap occasionally. It happens. As you get
better at testing and you learn new strategies it will happen less.

As a general rule of thumb, you should not be able to delete any code without it
making a test fail. If code can be deleted without a test failing, it either
contains assumptions that aren't being validated routinely by your test suite, or
it's cruft and it's doing nothing. Either way, you don't want it.

---

Anyway, when we last left our code, we were here:

```ruby
it "returns true when passed an even number" do
  is_even = even?(2)
  expect(is_even).to be(true)
end
```

```ruby
def is_even?(num)
  return true
end
```

Now let's write our other, long awaited test:

```ruby
it "returns false when passed an odd number" do
  is_even = even?(1)
  expect(is_even).to be(false)
end
```

Only *now* do we need to make our test contingent upon the value of the passed
in variable.

Only now *should* we make our test contingent upon the value of the passed
in variable.

```ruby
def is_even?(num)
  (num % 2) == 0
end
```

*Now* we're done.

I've had very smart people without TDD or agile backgrounds argue fiercely with
me over this. After writing the "*returns true when passed an even number*" test,
they want to lop their untested assumptions into the code. They want to also
make is_even?(num) return false if num is odd.

One person in particular said to me:

> It's dangerous to hardcode tests to make them green. It's dangerous because
> I could clone the code, write a failing test, make it pass, and then I may think the
> feature is done because all the tests pass and push the code up.

This argument is horrible, but that's probably because I haven't communicated
the point of tests well enough to this person.

**Green tests are not an indication of feature completion.** When you cloned the
code, all the tests passed. This logic is bad.

**Green tests indicate that all of the assumptions which you decided to
test in your code are still valid.**

