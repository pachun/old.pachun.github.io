---
layout: post
title: "value objects in ruby"
description: ""
category: 
tags: []
---

There are two types of objects: reference and value. The difference between
these two types of objects is how they evaluate equality.


Reference objects evaluate equivalence by comparing _where_ their data is stored
in RAM. (Do they _refer_ to the same location?)

Value objects evaluate equivalence by comparing _what_ is stored in RAM.

# reference objects

By default, classes you create in ruby are reference objects.

```ruby
[1] pry(main)> class Person
[1] pry(main)*   attr_accessor :name
[1] pry(main)*   def initialize(name)
[1] pry(main)*     @name = name
[1] pry(main)*   end
[1] pry(main)* end
=> :initialize

[2] pry(main)> nick = Person.new("nick")
=> #<Person:0x007ff381885438 @name="nick">

[3] pry(main)> another_nick = Person.new("nick")
=> #<Person:0x007ff3818060c0 @name="nick">

[4] pry(main)> nick == another_nick
=> false
```

Both __nick__ and __another_nick__ are _People_ with the same name attribute
value ("nick"), but their equivalence evaluates to false because their data is stored at
different points in memory. The main effect is that changing one will not change
the other.

```ruby
[7] pry(main)> another_nick.name = "a whole new nick"
=> "a whole new nick"

[8] pry(main)> nick.name
=> "nick"
```

Reference object equivalence evaluates to true when both objects refer to the
same data in memory. THe effect of this that changing one _will_ change the
other.

```ruby
[5] pry(main)> the_same_nick = nick
=> #<Person:0x007ff381885438 @name="nick">

[6] pry(main)> nick == the_same_nick
=> true

[7] pry(main)> the_same_nick.name = "new identity"
=> "new identity"

[8] pry(main)> nick.name
=> "new identity"
```

#value objects


