---
title: Goliath's Middlewares Sequence
layout: post
excerpt: Understanding the sequence of middlewares on Goliath during request is essential when working on it. This post is intended to show you the sequence.
---

Goliath async behaviour is interesting. When dealing with Middlewares or Aroundwares, understanding the flow of the registered middlewares within Goliath is essential when working on it.

Here's the snippet to show you about the sequence that happen during request:

```ruby
require 'goliath'

class Aroundware
  include Goliath::Rack::BarrierAroundware

  def pre_process
    $stdout.puts 'Aroundware: pre_process'
    return Goliath::Connection::AsyncResponse
  end

  def post_process
    $stdout.puts 'Aroundware: post_process'
    [status, headers, body]
  end
end

class Anotherware
  include Goliath::Rack::BarrierAroundware

  def pre_process
    $stdout.puts 'Anotherware: pre_process'
    return Goliath::Connection::AsyncResponse
  end

  def post_process
    $stdout.puts 'Anotherware: post_process'
    [status, headers, body]
  end
end

class Middleware
  include Goliath::Rack::AsyncMiddleware

  def post_process(env, status, headers, body)
    $stdout.puts 'Middleware: post_process'
    [status, headers, body]
  end
end

class Validation
  include Goliath::Rack::Validator

  def initialize(app)
    @app = app
  end

  def call(env)
    $stdout.puts 'Validation: call'
    @app.call(env)
  end
end

class SEQ < Goliath::API
  use Middleware
  use Validation
  use Goliath::Rack::BarrierAroundwareFactory, Aroundware
  use Goliath::Rack::BarrierAroundwareFactory, Anotherware

  def on_headers(env, headers)
    $stdout.puts 'APP: on_headers'
  end

  def on_body(env, data)
    $stdout.puts 'APP: on_body'
  end

  def on_close(env)
    $stdout.puts 'APP: on_close'
  end

  def response(env)
    $stdout.puts 'APP: response'
    [200, {}, []]
  end
end
```

You can copy above snippet and run locally test by yourself. Here's the output of the sequence:

```
## OUTPUT
#
# APP:          on_headers
# APP:          on_body
# Validation:   call
# Aroundware:   pre_process
# Anotherware:  pre_process
# APP:          response
# Anotherware:  post_process
# Aroundware:   post_process
# Middleware:   post_process
# APP:          on_close
#
```
