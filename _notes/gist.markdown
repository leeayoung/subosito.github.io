---
title: Notes about Gist
layout: page
permalink: /notes/gist/
---

[Gist](https://gist.github.com/) is a simple tool by [Github](https://github.com/) to share snippets with others. All gists are repositories, so they are automatically versioned and forkable.

## Embed the Gist

To embed gist you can copy the embed code from the gist page. Typically it's something like:

{% highlight html %}
<script src="https://gist.github.com/subosito/1814236.js"></script>
{% endhighlight %}

Which produce output:

{% gist 1814236 %}

See, two files of the gist automatically embedded.

## Embed particular file of the Gist

If you want to embed particular file only you can add params `file=filename` to the script source.

{% highlight html %}
<script src="https://gist.github.com/subosito/1814236.js?file=shell"></script>
{% endhighlight %}

And the output will be:

{% gist 1814236 shell %}

## Embed Gist on Jekyll powered site

If you are using [Jekyll](http://jekyllrb.com/) to generate your site, you can use `gist` tag to embed the Gist:

```
{{ "{% gist 1814236 " }}%}
```

And to render particular file you can specify `filename` in the `gist` tag:

```
{{ "{% gist 1814236 shell " }}%}
```

## Hide bottom bar of the Gist output

If you think that bottom bar which have text "This Gist brought to you by GitHub." looks ugly, you can hide it by using simple css:

{% highlight css %}
.gist .gist-file .gist-meta {
  display: none;
}
{% endhighlight %}

And you'll see above gist file like this:

<figure class="gist-no-bar">
  {% gist 1814236 shell %}
</figure>

Much simpler right? :)

## Using Gist inside Vim

Gist integration inside Vim is now possible, thanks to `gist-vim` plugin by [Yasuhiro Matsumoto](http://mattn.kaoriya.net/). You can get it from [https://github.com/mattn/gist-vim](https://github.com/mattn/gist-vim).

You can install it via `Vundle`, add to your `.vimrc`:

{% highlight vim %}
Bundle 'mattn/webapi-vim'
Bundle 'mattn/gist-vim'
{% endhighlight %}

Restart your vim and thn run `:BundleInstall`.

To post current buffer to Gist you can use:

{% highlight vim %}
:Gist
{% endhighlight %}

You can refer to `gist-vim` readme for the more usage and options.

