---
title: From Wordpress to Jekyll
layout: post
category: ruby
---

_Hello?! Are you kidding to me? What's wrong with [Wordpress](http://www.wordpress.org/) ? Why did you need to migrate from Wordpress to ehm,, what do you said before, [Jekyll](http://github.com/mojombo/jekyll) ? what kind of animal is it?_

Wait, wait.. relax guys. This is just a matter of flavor, nothing else, I will tell you deeper. Hope you understand my motivation.

Before I dig the material, I want to say "Hello!" to everyone outside there, I miss you so much. It has been 6 months since my last update, and, wow, that a loooong time ago. Nevermind, glad to see you here again :).

## Some Thoughts Behind Migration

I have been Wordpress fan since a long time ago, I have created some plugins and themes based on it. And, I think I wouldn't never leave wordpress for any reasons. The idea of moving this blog from Wordpress to Jekyll, actually, because of my addiction to [Ruby](http://www.ruby-lang.org) and [Git](http://git-scm.com), a marvelous combinations.

Basically, when we use Wordpress, we store all posts and post metadatas on [Mysql database](http://www.mysql.com). Numerous plugins and themes outside there, make us easily to customize Wordpress as we want. Wordpress is a very simple and widely acceptable solution for whom who want blogging.

My personal technical point of views, Wordpress has limitations on how to manage posts. They are:

* *Posts stores as (almost auto generated) html*. If you frequently perform [diff](http://en.wikipedia.org/wiki/Diff), this is will be nightmare. You will find it easier if it based on 'manually typed' [textile](http://www.textism.com/tools/textile/), [markdown](http://daringfireball.net/projects/markdown/) or even plain HTML.
* *Posts stores on database*. Nothing wrong about this, but if your posts is on flat file, you will be easier to versioning it, then maybe post it on versioning service, like [Github](http://github.com/). The benefits is your posts are archiveable and versionable.
* _Another reason_ *No code faster than no code*. Derived from [Merb](http://merbivore.org/), philosophy, I totally agree with this phrase. If you are using wordpress, you need server that support [PHP](http://www.php.net/) and mysql, and each request is processing by PHP interpreters on the server. Now think about another scenario: if your pages is on HTML pages, your server will be easier and served it faster than PHP pages, because there are no backend processing anymore.

That's my personal thoughts. As I said, that's just limitations not _drawbacks_ ;). I love and still using Wordpress for my other blogs.

## What is Jekyll?

When I was learning Ruby, I found a nice idea, what if my blog is a simple as collection of HTML pages. Then a [Ruby Gem](http://rubygems.org/) comes to my mind, it called Jekyll from [Mojombo](http://wiki.github.com/mojombo). [According to their explanation](http://wiki.github.com/mojombo/jekyll), Jekyll is a blog aware, static pages generator. It uses [liquid](http://www.liquidmarkup.org/), as template, parse textile, mardown or plain HTML as posts. Benefits of using Jekyll is there is no database and no specific language on the server required. No pain, no hassle.

### Installation

Ok, now you are understand what Jekyll is. Our next step is installation. As other ruby gems, installation of Jekyll is fairly simple.

Make sure you have Ruby and Rubygem installed on your local computer. You can read [how to install Ruby](http://www.ruby-lang.org/en/downloads/), and [Rubygem](http://rubygems.org/pages/download), first if you haven't it installed.


{% highlight bash %}
gem install jekyll
{% endhighlight %}

Jekyll requires other gems like `liquid`, `maruku`, `open4`, `directory_watcher`, `classifier`, `maruku` and `RedCloth`. Don't worry, `gem install` command above will install other gems as well.

### Convert Wordpress Posts to Jekyll Posts Format

_Yeah, Jekyll installed on my computer.. now what?_

Let's do migration from Wordpress. Here is step by step:

1. Create temporary directory, like `_tmp`
2. Change to `_tmp`
3. Copy `csv.rb` and `wordpress.rb` from Jekyll gem directory, usually `lib/jekyll/converters`
4. execute `ruby -r 'wordpress' -e 'Jekyll::WordPress.process( "db_name", "db_user", "db_pass", "db_host")'` _remember to properly change parameters_
5. You will find new `_posts` directory that contains all of your existing wordpress posts. Congratulations ;)

### Basic File Skeleton and Usage

First create new directory, then move into this directory. Basically, Jekyll need at least file system structure like below to run properly:

* `_config.yml` Jekyll configuration and site wide settings will be stored here
* `_layouts` Directory to save layout templates
* `_posts` Directory that contains all posts
* `index.html` Homepage of the site
* `_includes` Stores all of reusable snippets

To generate static pages, we can simply issue `jekyll` and Jekyll will automatically creates a new directory `_site` that contains all of our static pages.

Let's dig deeper about this directory structures:

#### _config.yml

Jekyll configuration saved using [YAML](http://www.yaml.org), format. You can set jekyll configurations here and you can also add site specific configuration too. You can access this configuration via `site.[config_key]` in the layout or post.

Below is my current _trimmed_ `_config.yml`:

{% highlight yaml %}
# jekyll configuration

destination: ./_site
lsi: true # enable related posts
markdown: maruku
pygments: true # enable syntax highlight via Pygments
permalink: /:year/:month/:title/ # set custom permalink
exclude: # exclude some files from being copied into _site
  - rakefile.rb
  - .gitignore
  - README.textile

# miscellaneous configuration
title: "subosito"
url: "http://subosito.com"
feed: "http://feeds.feedburner.com/subosito/rss"
license:
  type: "Creative Commons Attribution 3.0 License"
  url: "http://creativecommons.org/licenses/by/3.0/"

# end of configuration
{% endhighlight %}

For more information about permalink in Jekyll, you can read [Jekyll documentation about permalink](http://wiki.github.com/mojombo/jekyll/permalinks).

#### _layouts

This directory contains web site templates or layouts. Layouts use [Liquid markup](http://www.liquidmarkup.org/). Post automatically injected to the layouts using Liquid tag `{{ "{{ content " }}}}`. Each post may have different layout by define it on YAML front matter.

Liquid is templating language that developed by folks at [Shopify](http://www.shopify.com/). Learn [how to using Liquid](http://wiki.shopify.com/UsingLiquid), from their Shopify's wiki.

#### _posts

This directory is the heart of your posts. All of files named with format `year-month-date-non_space_title.format`. For example, this post, published on 25 April 2010, titled "From Wordpress to Jekyll", and written in `textile` format, so I will put this post as `2010-04-25-from-wordpress-to-jekyll.textile`. That's it, Jekyll will understand how to parse this post ;).

#### _includes

_includes directory stores reusable snippets that can be included with `{{ "{% include filename.html " }}%}`. You will find it usable to store navigation menu, disqus-threads script, google analytics script, or many other things that you need.

### Take A Look Into YAML Front Matter

As I said before, each post may have different layout and properties, by define it on [YAML front matter](http://wiki.github.com/mojombo/jekyll/yaml-front-matter). So what is YAML front matter? YAML front matter is identifier for Jekyll to know that particular files are special files that need to be processed. Because this is based on YAML, so the format is:

{% highlight yaml %}
---
title: From Wordpress to Jekyll
layout: post # you can define different layout  here
---
{% endhighlight %}

*Remember* All files that has YAML front matter will always be processed by Jekyll.

## Let's Work!

So you are ready to build your website? Great, now let's create a basic layout. Create new file on `_layouts` directory, for example, I called `master.html`, this file will be our site wide template. Here's the code:

{% highlight html %}
<html lang="en">
  <head>
    <title>{{ "{{ page.title " }}}}  - {{ "{{ site.title " }}}}</title>
    <meta name="description" value="{{ "{{ page.description " }}}}" />
    <meta name="keywords" value="{{ "{{ page.keywords " }}}}" />
    <link rel="stylesheet" type="text/css" href="/stylesheets/style.css" media="screen" />
  </head>
  <body>
    <div id="container">
      {{ "{{ content " }}}}
    </div>
  </body>
</html>
{% endhighlight %}

Then, create unique layout for post, called `post.html`:

{% highlight html %}
---
layout: master
---

<h2>{{ "{{ post.title "}}}}</h2>
<div id="post-content">
  {{ "{{ content " }}}}
</div>

{{ "{% include disqus-threads.html " }}%}
{% endhighlight %}

Now, you have basic layout, now create `index.html` page as your homepage. Basically, your homepage will be contains all of your posts. Let's get your hand dirty.

{% highlight html %}
---
title: Put Your Homepage Title Here
layout: master
---

<h2>{{ "{{ page.title " }}}}</h2>
<h3>Recent Posts</h3>
<ul>
  {{ "{% for post in site.posts " }}%}
  <li><a href="{{ "{{ post.url " }}}}">{{ "{{ post.title " }}}}</a></li>
  {{ "{% endfor " }}%}
</ul>
{% endhighlight %}

After you created all files above, copy all of imported posts files above (`_tmp/_posts`), into `_posts`. Then on the console, just type:

{% highlight bash %}
$ jekyll --server --auto
{% endhighlight %}

Now, open your browser and point to [http://localhost:4000/](http://localhost:4000/), If you see all of your existing wordpress posts, *Congratulations!* your migration stage almost completed ;)

## Let's World See It

All static pages generated, now let's copy it to the server, and show the world. Since all of generated pages are plain HTML files, you can upload it directly using *FTP* with your [favorite FTP client](http://fireftp.mozdev.org/). Even better, I suggest you to using awesome tool like [rsync](http://samba.anu.edu.au/rsync/). By issue single command, your files will be transfered automatically to your server.

{% highlight bash %}
$ rsync -avz /path/to/_site username@remotehost:path/to/blog
{% endhighlight %}

*Congratulations, migration Completed*. Your site now live!. In the next few days, I will cover some tips and trick to make jekyll based pages more powerfull. For now you can take a look [source codes of this site](http://github.com/subosito/subosito.github.com) on the Github to get full overview about Jekyll usage for this blog.

If you have any questions or suggestions, don't be hesitate to put it on comment form below. *See ya! :D*

