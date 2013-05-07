---
title: "Gingerice: A rubygem for natural spelling and grammar checker"
date: 2013-04-28 07:59:36
layout: post
category: opensource
excerpt: Overview and usage of Gingerice, a rubygem for correcting spelling and grammar mistakes based on the context of complete sentences.
---

## Overview

Ruby wrapper of [Ginger Proofreader](http://www.gingersoftware.com/) which corrects spelling and grammar mistakes based on the context of complete sentences by comparing each sentence to billions of similar sentences from the web.

### Installation

You can add it on Gemfile:

{% highlight ruby %}
gem 'gingerice'
{% endhighlight %}

or using plain `gem install`:

{% highlight bash %}
$ gem install gingerice
{% endhighlight %}


### Usage

{% highlight ruby %}
require 'gingerice'

text = 'The smelt of fliwers bring back memories.'

parser = Gingerice::Parser.new
parser.parse text
{% endhighlight %}

Which produces output:

{% highlight text %}
{
           "text" => "The smelt of fliwers bring back memories.",
         "result" => "The smell of flowers brings back memories.",
    "corrections" => [
        [0] {
                  "text" => "smelt",
               "correct" => "smell",
            "definition" => nil,
                 "start" => 4,
                "length" => 5
        },
        [1] {
                  "text" => "fliwers",
               "correct" => "flowers",
            "definition" => "a plant cultivated for its blooms or blossoms",
                 "start" => 13,
                "length" => 7
        },
        [2] {
                  "text" => "bring",
               "correct" => "brings",
            "definition" => nil,
                 "start" => 21,
                "length" => 5
        }
    ]
}
{% endhighlight %}

### Command Line

Gingerice also has executable for your convenient:

{% highlight bash %}
$ gingerice "Edwards will be sck yesterday"
{% endhighlight %}

{% highlight text %}
Edwards was sick yesterday
{% endhighlight %}

Or if you want verbose output you can add `--verbose` or `-v` flag:

{% highlight bash %}
$ gingerice --verbose "Edwards will be sck yesterday"
{% endhighlight %}

{% highlight text %}
{
           "text" => "Edwards will be sck yesterday",
         "result" => "Edwards was sick yesterday",
    "corrections" => [
        [0] {
                  "text" => "will be",
               "correct" => "was",
            "definition" => nil,
                 "start" => 8,
                "length" => 7
        },
        [1] {
                  "text" => "sck",
               "correct" => "sick",
            "definition" => "affected by an impairment of normal physical or mental function",
                 "start" => 16,
                "length" => 3
        }
    ]
}
{% endhighlight %}

