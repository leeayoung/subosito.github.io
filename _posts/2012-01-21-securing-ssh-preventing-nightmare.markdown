---
title: Securing SSH, Preventing Nightmare
layout: post
category: ssh
---

<img src="/img/ssh.png" alt="ssh" class="right">

When managing server, SSH is the most important aspect. It's a door for in and out administrator. Making sure that SSH is secure is always comes into my mind when I first setup VPS. Setting it up first makes me confident that I am the only one who can manage the server.

By default, SSH installation is allow anyone to connect on post 22, using registered user and password. It's secure enough, but we can increase its security signicantly by tuning up SSH daemon configuration. Curious how to do that? Let's get our hand dirty :D

Basically, SSH daemon configuration located on `/etc/ssh/sshd_config`, Let's take a look line by line changes:

### Port

SSH usually run on port 22 which is the default port for ssh connection. For better security, I usually change it into an unusual port.

{% highlight cfg %}
Port 22
{% endhighlight %}

become:

{% highlight cfg %}
Port 27389 # choose any number
{% endhighlight %}

### Root? Sorry

Allowing root login is a mistake, real mistake. I turn the ability for root to login off. Change the line:

{% highlight cfg %}
PermitRootLogin yes
{% endhighlight %}

to:

{% highlight cfg %}
PermitRootLogin no
{% endhighlight %}

### Password is not on the keyboard

I always create user without password for my server which I think there is no chance for brute for attack or any other similar attacks (by guessing username and password pair). Here's my command when create user:

{% highlight bash %}
adduser --home /home/rico --shell /bin/bash --disabled-password rico
{% endhighlight %}

Since the user have no password, so I disable password authentication for SSH which lead more secure server:

{% highlight cfg %}
PasswordAuthentication yes
{% endhighlight %}

become:

{% highlight cfg %}
PasswordAuthentication no
{% endhighlight %}

You might wondering, if user don't have any password, how I could log in as the user? The answer is public key authentication. You can read on <a href="/2009/08/passwordless-ssh-on-site5/">my old post</a> to see how to implement it.

### Allowed Users

I usually whitelist users who able to log in using ssh. By putting their name manually on configuration. If a user is not listed, he will not able to login.

{% highlight cfg %}
AllowUsers rico kowalski
{% endhighlight %}

Adding these line will make only rico and kowalski that able to log in. Other users will be unable to open the door.

### Conclusion

The small changes of `sshd_config` is dramatically increase our server security and prevent attacker for gain access into our server. Hope this help and please let me know if you have any addition to increase security of server related ssh daemon configuration.
