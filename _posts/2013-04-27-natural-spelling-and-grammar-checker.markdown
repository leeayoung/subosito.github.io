---
title: Natural spelling and grammar checker
layout: post
excerpt: A rubygem which corrects spelling and grammar mistakes based on the context of complete sentences.
---

Say, you have a text, "Edwards will be sck yesterday". How do you check validity of spelling and grammar for these texts?

Using google translate, you'll be getting correction of "sck" become "sick". But that's still invalid, right?

Thankfully, there are a spelling and grammar based on the context. Say hello to [Ginger Proofreader](http://www.gingersoftware.com/). Ginger Proofreader provides service for such natural spelling and grammar correction.

Currently Ginger Proofreader only provides the desktop application and demo on their website. If you want to use ginger ability inside Ruby application or on your shell, you can use `gingerice` gem. You can install it with:

{% highlight bash %}
$ gem install gingerice
{% endhighlight %}

Then, on your prompt you can type:

{% highlight bash %}
$ gingerice "Edwards will be sck yesterday"
Edwards was sick yesterday
{% endhighlight %}

You can refer to [gingerice README](https://github.com/subosito/gingerice) for more usages and sample outputs.

