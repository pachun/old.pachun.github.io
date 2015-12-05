---
layout: post
title: "Mounting a Samba Server with Ruby"
description:
category:
tags:
---

Yes, a Samba server. I work for a corporation and I had to do this today.
I'm contributing to a streamlined process
capable of integrating with several feasible synergistic solutions.

I tried four or five gems that claimed they'd _samba_
for me in a ruby-esque way. None of them worked. Installing or trying to use a
couple of them resulted in
pretty startling errors, like missing C header files. I tried resolving
those issues to no avail before ultimately resorting to scripting the shell.

Though not technically a "ruby" solution, you _can_ put this line in a ruby
script and it will block execution until the mount is complete

```ruby
system "mount_smbfs //user:pass@server/location mount_path"
```

mount_path should be a relative or absolute path to an existing folder that is
empty. When you're done, unmount with

```ruby
system "umount -f mount_path"
```

If you need to use credentials in a script you may be sharing with others, [I
recommend using the ruby gem Dotenv](https://github.com/bkeepers/dotenv). Then,
you can do

```ruby
require 'Dotenv'
Dotenv.load

user = ENV['USERNAME']
pass = ENV['PASSWORD']
system "mount_smbfs //#{username}:#{password}@server/location mount_path"
```

and put your credentials in a file called __.env__

```
USERNAME=pachun
PASSWORD=simple_jack
```

Don't forget to add __.env__ to your __.gitignore__.

Sorry you had to work with Samba! There's a special place for the people that
asked you to.
