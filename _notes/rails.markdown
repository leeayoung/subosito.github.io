---
title: Notes about Ruby on Rails
layout: page
permalink: /notes/rails/
---

[Ruby on Rails](http://rubyonrails.org/) is open source web framework built using Ruby. It's optimized to speed up the development and programmer happiness.

## Rescue from Exceptions

You can set Rails to handle exceptions automatically. For example, here's how to handle `CanCan::AccessDenied` exception:

{% highlight ruby %}
class ApplicationControoler < ActionController::Base
  rescue_from CanCan::AccessDenied do |exception|
    render :text => exception.message, :status => 403, :layout => false
  end
end
{% endhighlight %}

Or, you can use regular method for that:

{% highlight ruby %}
class ApplicationControoler < ActionController::Base
  rescue_from CanCan::AccessDenied, :with => :forbidden_handler

  def forbidden_handler(exception)
    render :text => exception.message, :status => 403, :layout => false
  end
end
{% endhighlight %}

## Useful helpers

### Get the current namespace

On the view helper, you can write:

{% highlight ruby linenos %}
class Aero::HelloHelpers
  def current_namespace
    controller.class.name.split('::').first
  end
end
{% endhighlight %}

Or, if you want to put it on `controller`, you can write:

{% highlight ruby linenos %}
class Aero::HelloController < ApplicationController
  def index
    @namespace = self.class.name.split('::').first
  end
end
{% endhighlight %}

### Locale based orientation helpers

{% highlight ruby %}
module Aero::OrientationHelpers
  def current_locale
    I18n.locale.to_s
  end

  def rtl_locales
    %w(ar ckb fa he ug)
  end

  def rtl?
    rtl_locales.include? current_locale
  end

  def orientation
    rtl? ? 'rtl' : 'ltr'
  end
end
{% endhighlight %}

### Helper to constructs body attributes

Using above helpers you can build a body attributes that you can use on your templates.

{% highlight ruby %}
module Aero::AttributeHelpers
  def namespace_name
    current_namespace.try(:downcase)
  end

  def body_attributes
    classes  = []
    classes << [controller_name, action_name].join('-')
    classes << namespace_name unless namespace_name.nil?
    classes << orientation

    { dir: orientation, class: classes.join(' ') }
  end
end
{% endhighlight %}

`body_attributes` usage on a `haml` template:

{% highlight haml %}
!!!
%html
  %body{body_attributes}
{% endhighlight %}

Or, in a `slim` template:

{% highlight html %}
doctype html
html
  body *body_attributes
{% endhighlight %}

### Arabic numerals converter

For those who want perform conversion between numerics and arabic numerals.

{% highlight ruby %}
module Aero::ConverterHelpers
  def convert_to_numeric(value)
    value.to_s.reverse.tr('٠١٢٣٤٥٦٧٨٩', '0123456789')
  end

  def convert_to_arabic(value)
    value.to_s.reverse.tr('0123456789', '٠١٢٣٤٥٦٧٨٩')
  end
end
{% endhighlight %}

