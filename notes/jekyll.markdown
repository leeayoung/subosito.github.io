---
title: Notes about Jekyll
layout: page
permalink: /notes/jekyll/
---

[Jekyll](http://jekyllrb.com/) is a simple, blog-aware, static site generator from dynamic components like Markdown and Liquid.

## Detecting Jekyll running mode

If you wanto to include some component like analytics or comments, it should be better if you check wether Jekyll running as `server` or not. This approach makes Jekyll to skip unnecessary component during local development.

{% highlight text %}
{{ "{% unless site.host " }}%}
  {{ "{{ include analytics.html " }}}}
{{ "{% endunless " }}%}
{% endhighlight %}

## Writing liquid tag within jekyll pages

To write liquid tag within jekyll pages you can write your liquid syntax like:

{% highlight text %}
{{ '{{ "{% for note in notes " ' }}}}%}
  {{ '{{ "{{ note.title " ' }}}}}}
{{ '{{ "{% endfor " ' }}}}%}
{% endhighlight %}

Which produces the output:

{% highlight text %}
{{ "{% for note in notes " }}%}
  {{ "{{ note.title " }}}}
{{ "{% endfor " }}%}
{% endhighlight %}

## Keep your Jekyll version match with GitHub Pages

When using Jekyll powered website and published it to the GitHub Pages, it's important to keep Jekyll version and its dependencies always same with GitHub Pages server version. You can use `Gemfile` bundler power to maintain that.

{% highlight ruby %}
source 'https://rubygems.org'

gem 'jekyll', '1.0.0'
gem 'liquid', '2.5.0'
gem 'redcarpet', '2.2.2'
gem 'maruku', '0.6.1'
gem 'rdiscount', '1.6.8'
gem 'RedCloth', '4.2.9'
{% endhighlight %}

Then you can run the jekyll using:

{% highlight bash %}
$ bundle exec jekyll
{% endhighlight %}

You can see on [GitHub Pages' help](https://help.github.com/articles/using-jekyll-with-pages#troubleshooting) about their Jekyll version.

## Generate pygments themes css

Since Jekyll depends on `pygments` to generate syntax highlights, you can use built-in pygments command to generate `css` syntax that can be used on your jekyll powered website. Here's how:

{% highlight bash %}
$ pygmentize -S monokai  -f html | sed 's/\.hll/\.highlight/g' > syntax.css
{% endhighlight %}

Above command will generate css which you can directly use on your site.

### List the available pygments themes

If you have no idea about pygments theme name, you can list them by:

{% highlight text %}
$ pygmentize -L styles
Pygments version 1.5, (c) 2006-2011 by Georg Brandl.

Styles:
~~~~~~~
* monokai:
    This style mimics the Monokai color scheme.
* autumn:
    A colorful style, inspired by the terminal highlighting style.
* rrt:
    Minimalistic "rrt" theme, based on Zap and Emacs defaults.
....
{% endhighlight %}

Then you can use previous command to generate your desired css style.

### Ensure Pygments and Gist styles in harmony

By default when you embed pygments and gist on the same page, they will be share same style declaration `.highlight`. To make both Pygments and Gist has dedicated style you can alter pygments generator code to:

{% highlight bash %}
$ pygmentize -S monokai -f html | sed 's/^\./div\.highlight \./g' | sed 's/\.hll//g' > syntax.css
{% endhighlight %}

You can see the sample of this in this blog's [syntax.css](/assets/css/syntax.css).

## Using GitHub Flavored Markdown (GFM)

If you're GitHub user, you might be familiar about additional syntax which used within GitHub markdown parser. You can find it almost everywhere from comments, issues, messages to readme. All of these things is called [GFM (GitHub Flavored Markdown)](https://help.github.com/articles/github-flavored-markdown)

You can now using GFM within your Jekyll website by setting it on your `_config.yml`:

```yaml
markdown: redcarpet
redcarpet:
  extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "tables", "with_toc_data"]
```

Then instead of using:

{% highlight text %}
{{ '{% highlight ruby ' }}%}
def foo
  puts "bar"
end
{{ '{% endhighlight ' }}%}
{% endhighlight %}

You can use:

    ```ruby
    def foo
      puts "bar"
    end
    ```

