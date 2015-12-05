---
layout: post
title: "Value Objects in Ruby"
description: ""
category: 
tags: []
---

There are two types of objects: reference and value. The difference between
these two types of objects is how they evaluate equality.


Reference objects evaluate equivalence by comparing _where_ their data is stored
in RAM. (Do they _refer_ to the same location?)

Value objects evaluate equivalence by comparing _what_ is stored in RAM.

# Reference Objects

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

Both __nick__ and __another_nick__ are _People_ with the same _name_ attribute
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
same location in memory. The effect of this is that changing one _will_ change the
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

Ruby objects are reference types by default.
The difference between reference and value types is how they evaluate equality.
It follows that you can contrive value objects in ruby by overriding the
equivalence operator __:==__ of reference types.

```ruby
[1] pry(main)> class Person
[1] pry(main)*   attr_accessor :name, :age
[1] pry(main)*
[1] pry(main)*   def ==(other_person)
[1] pry(main)*     self.class == other_person.class &&
[1] pry(main)*       @name == other_person.name &&
[1] pry(main)*       @age == other_person.age
[1] pry(main)*   end
[1] pry(main)* end
=> :==

[2] pry(main)> john = Person.new
=> #<Person:0x007fe48285d8b0>

[3] pry(main)> john.name = "john"; john.age = 31
=> 31

[4] pry(main)> other_john = Person.new
=> #<Person:0x007fe4819445e0>

[5] pry(main)> other_john.name = "john"; other_john.age = 31
=> 31

[6] pry(main)> john == other_john
=> true
```

Another implied property of value objects is that they're immutable, so change
those attr\_accessor's to attr\_reader's and

![emeril](/assets/images/emeril.jpg)

you've got yourself a value object.


