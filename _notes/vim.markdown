---
title: Notes about Vim
layout: page
permalink: /notes/vim/
---

[Vim](http://www.vim.org/) is an advanced text editor which highly configurable to enable efficient text editing.

<h2>
  <a class="anchor" name="automatic-text-translation" href="#automatic-text-translation">
    <span class="icon-link"></span>
  </a>
  <span>Automatic text translation</span>
</h2>

Yes, now you can translate text to any language inside Vim. You can use my [vim-translator](https://github.com/subosito/vim-translator). If you're using `Vundle` you can add to your `.vimrc`:

{% highlight vim %}
Bundle 'subosito/vim-translator'
{% endhighlight %}

Restart your vim and run `:BundleInstall` to install the plugin.

To configure the the plugin source and destination language, you can add line:

{% highlight vim %}
let g:goog_user_conf = { 'langpair': 'en|ar', 'v_key': 'T', 'charset': 'utf-8' }
{% endhighlight %}

Above line will configure the plugin to translate text from `en` to `ar` (*English* to *Arabic*).

Now you can just select text on visual mode and then press <kbd>T</kbd>. The selected text will be translated to your desired language automatically.

Here's the sample output:

```
Thank you, Sir!
شكرا لك ، يا سيدي!
```

