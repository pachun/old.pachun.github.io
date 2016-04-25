---
layout: post
title: "How I Learn"
description: ""
tags: ["learning", "major league pong"]
---

In 2012 I was a sophomore in college. Over winter break I
decided to learn how to write iOS apps. Fortunately, I'd learned Objective-C a
couple years earlier but I still had to get my feet wet with Xcode and the iOS
SDK. During that break I learned a bit of game development and also plain
iOS development to give myself a solid springboard for whatever I decided I
liked more.

When I got back to class, I had to take my "CS capstone" class, _Software Design
and Documentation_. We called it SD&D. It's a capstone class because
it was the last class in the CS track at my school. At the time, I thought it
was an easy A blowoff. However, to my surprise, I'm still realizing things I learned in that
class.

There was a loose curriculum; we were instructed to group ourselves into collections of
three to five and invent a programming project to work on. I was in the class with three guys
from my fraternity and we all lived in the same house, which made grouping
together a good deal for us. Then someone had a great idea for our programming
project: Major League Pong.

Every semester, our house (I refer to my fraternity as our/the "house" often; apologies
for the duplicate terminology) held an in-house beer pong tournament
one night per week. Every game had a referee that kept a scoresheet for that
game.

![Major League Pong Scoresheet](/assets/images/mlp.jpg)

After each week, the league commissioner (AKA the guy whose job it was to get the
keg and do drunk number crunching), would calculate everyone's OPP (overall 
point percentage) by hand and throw it up on a static page local to the house
network.

Here's how OPP is calculated (it's on a per-player basis).

{% highlight ruby %}
[
  number_of_game_winning_cup_hits * 3
  +
  number_of_cups_hit_that_resulted_in_a_rerack * 2
  +
  number_of_remaining_cup_hits
]

/

total_shots_taken
{% endhighlight %}

See the scoresheet image for a helpful key; regular cup hits are numbers without a container, rerack cup hits are circled,
and game winning cups are enclosed in a triangle. (We always did reracks with 6 and 3
cups remaining, so hitting the 4th or 7th cups would get you double-points in the
numerator of your OPP.)

We would average OPPs by team and such to get a broader picture of who
was trending up and who really, really sucked. We held one of these tournaments
every semester, selecting new teams each time.

The professors of SD&D allowed us to work on this for our project, which made it
way more fun than it would have been otherwise. We were our own audience and it was great!
We got to use the toolchains we wanted and support the devices we
cared about. Josh wrote the web app and API in Rails, I wrote the iOS app using
Objective-C, James wrote the Android app using whatever those guys use, and Rory
did all the paperwork for the class. Rory, I am sorry.

I think one of the first things I learned was how to model things well. We even got
to come up with some cool terminology:

* A _League_ is a group of players. We had one league for our house in practice,
but the idea was that other houses may want to use MLP too.

* A _Season_ is a semester's long tournament among a group of players,
belonging_to a league

* A _Team_ is a collection of three players, belonging_to a season

Working on an app which we were all an active and caring audience for made the learning fun. The domain for
this app necessitated learning how to think in terms of the entire stack. How
should concept X be modeled? Which parts should the API serve to which clients?
How should it be displayed once the information is sent over the wire?

Since 2012, I've rewritten MLP (the whole app and sometimes just the front or
back end), tens of times. It's
a good domain space that I understand well and makes me feel good to work on.
If you can discover a similar app idea and iterate over it to learn new tools
, I encourage you to do so. There are always gotchas when learning how to
use new tools. Writing this app usually led me to many of those
gotchas, which is the first step in learning how to overcome them.

And there are still dark corners in this app's map; gotchas that I haven't
figured out yet. Like, what if another house wants to play by
different rules? What if they don't roll balls back if everyone on a team hits a
cup?
What if they don't do automatic reracks when six and three cups remain like
we do? What if they fight for the balls after every shot like a
bunch of drunk idiots instead of taking turns like us civil drunks?
Decisions around these things really mess with the mechanics of how the game
model is structured. Here's how we did it.

A _Game_ `has_many` _Round_ objects, each of which encapsulates two
_Turn_ objects. _Turn_ objects also `belong_to` _Team_ instances. _Turns_ have
[1, 3] _Shot_ objects, depending on how many players per team. Each of those
shots also `belong_to` a _Player_, in the same way that _Turns_ `belong_to`
_Team_ objects.

That's pretty hard to follow in text, so take another look at the
scoresheet image for clarity and scroll down a bit for a narration.

![Major League Pong Scoresheet](/assets/images/mlp.jpg)

The whole scoresheet is a _Game_.

Horizontal rows are _Round_s containing two _Turn_s.

_Turn_s are half of each horizontal row and each belongs to a _Team_.

Vertical columns are _Shot_s belonging to a _Player_.

---

I learned how to use Xcode with storyboards by writing MLP. I learned how to
write RESTFUL web APIs with Sinatra by writing MLP. I must've rewritten the iOS
client in ruby using [Rubymotion](http://rubymotion.com) at least five times. If you look at README
examples for gems I've written on Github, you can see in most of them that I was
thinking of the models from MLP.
