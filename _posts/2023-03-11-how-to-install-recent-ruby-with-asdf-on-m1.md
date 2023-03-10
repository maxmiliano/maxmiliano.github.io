---
title: "How to Install Ruby Version 3.2 using ASDF on M1 Macbook (and probably on M2, too)"
#subtile: ""
author: Max
tags: 
  - ASDF
  - tools
  - Ruby
  - Quick Tip
categories:
  - Tips
comments: true
related: true
date: 2023-03-11 07:31 +0100
last_modified_at: 2023-03-11 07:31 +0100
tagline: "Yeah! This was my first stumble while starting with ASDF!"
layout: single
classes: wide
excerpt: "Quick tip (actually a workaround) on how to install recent Ruby version using ASDF"
header:
  teaser: assets/images/HelloMax_angry_panda_bear_using_a_laptop_with_and_error_message_22cffd33-c049-4144-919f-c9fc89d4368c.png
  overlay_image: assets/images/HelloMax_angry_panda_bear_using_a_laptop_with_and_error_message_22cffd33-c049-4144-919f-c9fc89d4368c.png
  overlay_filter: 0.65
---

If you're having trouble installing Ruby version 3.2 using ASDF on your M1 Macbook, you're not alone. The standard commands `asdf plugin add ruby` and `asdf install ruby 3.2.1` may not work and can return an error message indicating that you need to install `psych` and other dependencies that you may already have installed. Don't worry though, there's a simple workaround that can help you get around this issue.

First, try adding the following environment variable to your terminal:

~~~sh
$ export RUBY_CONFIGURE_OPTS="--with-zlib-dir=$(brew --prefix zlib) --with-openssl-dir=$(brew --prefix openssl@3) --with-readline-dir=$(brew --prefix readline) --with-libyaml-dir=$(brew --prefix libyaml)"
~~~

Once you've added this environment variable, try running the install command again:

~~~sh
asdf install ruby 3.2.1
~~~

This should help you successfully install Ruby version 3.2 using ASDF on your M1 Macbook. Keep in mind that this workaround may become obsolete as the issue is fixed in the future, but for now, this is a quick and easy solution that you can use.

More details about this may be found in [this issue](https://github.com/asdf-vm/asdf-ruby/issues/328 ){:target="_blank"} from `asdf-ruby` repo.
{: .notice--info}
