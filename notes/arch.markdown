---
title: Notes about Arch Linux
layout: page
permalink: /notes/arch/
---

[Arch Linux](https://www.archlinux.org/) is a Linux distribution intended to be lightweight and simple. Besides its official package sets, it's also complement with a community-operated package repository.

## Find fastest mirror for pacman packages

Yes, there is a script to automatically do the task. `reflector` can retrieve the latest mirror list and filter the most up-to-date mirrors then sort them by speed.

Install `reflector` using pacman:

{% highlight bash %}
> pacman -S reflector
{% endhighlight %}

And here's the usage:

{% highlight bash %}
> reflector --verbose -l 200 -p http --sort rate --save /etc/pacman.d/mirrorlist
{% endhighlight %}

Now you'll get faster connection when performing pacman database updates or package upgrades.

## Java app GUI is not respond to any click events

When using tiling manager like `xmonad`, Java based app do not respond the GUI event correctly. This happens because of JVM is hard-coding the list of tiling windows managers. To make the Java app respond to the event, you need to impersonate as another window manager.

Fortunately, there is `wmname` package for this purpose. As usual you can install it using:

{% highlight bash %}
$ sudo pacman -S wmname
{% endhighlight %}

Then for usage:

{% highlight bash %}
$ wmname LG3D
{% endhighlight %}

You can also put it on your `.xinitrc` so it will be executed automatically when session is loaded.

## Managing clipboard

You can manage your clipboard entries by using `parcellite`. To install just issue:

{% highlight bash %}
$ pacman -S parcellite
{% endhighlight %}

To run it you can just using:

{% highlight bash %}
$ parcellite
{% endhighlight %}

If you want to hide parcellite status bar icon and run it each time you login, you can run it with flag `-n` and put it on your `.xinitrc`:

{% highlight bash %}
parcellite -n &
{% endhighlight %}

### Pipes to clipboard

You can directly pipe input to clipboard by using `xsel`, to install it:

{% highlight bash %}
$ sudo pacman -S xsel
{% endhighlight %}

Then, to pipe to primary clipboard you can use:

{% highlight bash %}
$ cat foo.txt | xsel -b
{% endhighlight %}

Now the content of `foo.txt` will be available on main clipboard.

Since you're using parcellite you can press <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>h</kbd>, and parcellite pop-up will be shown.

## Automatically suspend when battery is low

You can automatically suspend your system when your battery is draining. You can put a rule on `/etc/udevd/rules.d/lowbat.rules`:

```bash
SUBSYSTEM=="power_supply", ATTR{status}=="Discharging", ATTR{capacity}=="2", RUN+="/usr/bin/systemctl suspend"
```

## Short useful commands

### Burn an iso to a DVD

```bash
$ growisofs -dvd-compat -Z /dev/sr0=/path/to/file.iso
```

