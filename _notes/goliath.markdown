---
title: Notes about Goliath
layout: page
permalink: /notes/goliath/
---

[Goliath](http://goliath.io/) is an asynchronous Ruby web server powered by EventMachine reactor, a high-performance HTTP parser and Ruby 1.9 runtime.

## Configuration structure

By default configuration for the Goliath app is ruby configuration file with the same name as your API and resides on `config` directory.

{% highlight bash %}
.
|-- app.rb
`-- config
    `-- app.rb
{% endhighlight %}

Say we set Moped connection session called `moped` inside our configuration `config/app.rb`:

{% highlight ruby %}
require 'moped'

config['moped'] = EM::Synchrony::ConnectionPool.new(size: 4) do
  Moped::Session.new(['localhost:27017'], database: 'aeroapp')
end
{% endhighlight %}

Now we can access `moped` directly on our application `app.rb`.

{% highlight ruby %}
class App < Goliath::API
  def response(env)
    [200, {}, moped.inspect]
  end
end
{% endhighlight %}

The output will be something like this:

```
<EventMachine::Synchrony::ConnectionPool:0x00000003a14f48 @reserved={}, @available=[
    <Moped::Session seeds=["localhost:27017"] database=none>,
    <Moped::Session seeds=["localhost:27017"] database=none>,
    <Moped::Session seeds=["localhost:27017"] database=none>,
    <Moped::Session seeds=["localhost:27017"] database=none>
], @pending=[]>
```

### Application within a module

For the application within a module you can use format `config/module_class.rb` for your configuration, as you can see on Goliath `server.rb` source:

{% highlight ruby %}
# https://github.com/postrank-labs/goliath/blob/v1.0.3/lib/goliath/server.rb#L118

def load_config(file = nil)
  api_name = api.class.to_s.gsub('::', '_').gsub(/([^_A-Z])([A-Z])/,'\1_\2').downcase!
  # .. truncated ..
end
{% endhighlight %}

So for below app:

{% highlight ruby %}
module Aero
  class App < Goliath::API
  end
end
{% endhighlight %}

The configuration will be:

{% highlight bash %}
.
|-- app.rb
`-- config
    `-- aero_app.rb
{% endhighlight %}

