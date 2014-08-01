---
title: Notes about Bundler
layout: page
permalink: /notes/bundler/
---

[Bundler](http://bundler.io/) is a dependency management for Ruby applications. It tracks application code and the rubygems, so that an application will always have the exact gems that it needs to run.

## Run bundler based application anywhere

Ruby application that uses bundler as its dependency management will require bundler to be prepared to run correctly.

Say we have `aeroapp` application locates on `/mnt/code/app`. And here's the `aeroapp` looks like:

{% highlight bash %}
.
|-- Gemfile
|-- Gemfile.lock
|-- lib
|   `-- aero
|       `-- command.rb
`-- bin
    `-- aeroapp
{% endhighlight %}

And the content of `bin/aeroapp`

{% highlight ruby linenos %}
#!/usr/bin/env ruby

require 'aero/command'

cmd = Aero::Command.new
cmd.run
{% endhighlight %}

To execute the app outside its directory we need to specify `Gemfile` location:

{% highlight bash %}
$ pwd
/home/subosito

$ echo $PATH
/mnt/code/app/bin:/home/subosito/bin:....

$ BUNDLE_GEMFILE=/mnt/code/app/Gemfile aeroapp
Hello aero!
{% endhighlight %}

Yes, it's not pleasant.

We need to alter the `aeroapp` to automatically locates its `Gemfile`:

{% highlight ruby linenos %}
#!/usr/bin/env ruby

require 'pathname'
bin = Pathname.new(__FILE__).realpath

ENV['BUNDLE_GEMFILE'] ||= File.expand_path('../../Gemfile', bin)

require 'bundler'
Bundler.setup
Bundler.require(:default)

require 'aero/command'

cmd = Aero::Command.new
cmd.run
{% endhighlight %}

Now we can execute `aeroapp` with:

{% highlight bash %}
$ pwd
/home/subosito

$ aeroapp
Hello aero!
{% endhighlight %}

Looks better!

