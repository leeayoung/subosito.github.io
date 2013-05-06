---
title: Make Your Coding Style More Consistent
layout: post
category: convention
excerpt: Learn how to produce more consistent codes across developers by utilizing small and readable configuration which works across editors.
---

## Overview

One of the difficulties during collaborative development is making sure that one developer and another one has same coding style. Having same and consistent coding style makes easier for tracking and diffing changes. In the world of full with IDEs, editors and various toolkits which can be used by developers to produce his codes, it's bit difficult to make all of these tools working in harmony to produce consistent coding style.

Some editor has tab as its default configuration, some others has four spaces. Asking developers to configure its IDE to match the desired coding style is a hard and manual way, uhh. Forcing them to use same editor for the project, discarding their personal taste of their lovely editor? It's more terrible way. I doubt they would be agree with that, duhh.

So how we make developers and bunch of editors produce similar taste of codes and make them work in more consistent way?

## Solution

Fortunately, there is a way to do that. Please welcome [EditorConfig](http://editorconfig.org/). According to their official explanation:

> EditorConfig helps developers define and maintain consistent coding styles between different editors and IDEs. The EditorConfig project consists of a file format for defining coding styles and a collection of text editor plugins that enable editors to read the file format and adhere to defined styles. EditorConfig files are easily readable and they work nicely with version control systems.

### Configuration

Yes, that is. EditorConfig project aims to make developers and tools working way more consistent by using small readable configuration which you can put on your project root directory. Here's example of `.editorconfig` file:

{% highlight ini %}
root = true

[*]
indent_style = space
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.rb]
indent_size = 2

[*.{js,php}]
indent_size = 4

[*.markdown]
indent_size = 4
trim_trailing_whitespace = false
{% endhighlight %}

Using above configuration you and your teammates will get:

- `UTF-8` charset for all files
- Using spaces (no tabs) with length eight spaces
- Using two spaces for Ruby file
- Using four spaces for Javascript, PHP and markdown
- Always remove unused trailing whitespace for all files but markdown
- Insert new line in the end of file
- Using `LF` (line feed) for line break presentation

You can customize that with your team coding style. You can learn more about its syntax on [file format details section](http://editorconfig.org/#file-format-details).

### Plugins

In order to make EditorConfig works across your team, each developer must have plugin installed on his editor. Currently EditorConfig Support most editors, like:

- [Vim](https://github.com/editorconfig/editorconfig-vim)
- [Sublime Text 2](https://github.com/sindresorhus/editorconfig-sublime)
- [Emacs](https://github.com/editorconfig/editorconfig-emacs)
- Others, see [supported editors](http://editorconfig.org/#download)

## Tips

I put an `.editorconfig` on my HOME directory, it's acts as a global `.editorconfig` which control general usage of my editor and having specific `.editorconfig` for the project on the project root directory.

