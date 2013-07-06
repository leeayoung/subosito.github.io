---
title: Securing SSH, Preventing Nightmare
layout: post
category: ssh
excerpt: Configure SSH server to create secure server which makes attackers go away.
---

When managing server, SSH is the most important aspect. It's a door for in and out administrator. Making sure that SSH is secure is always important point when setting up a server.

By default, SSH installation is allow anyone to connect on post 22, using registered user and password. It's secure enough, but you can increase its security signicantly by tuning up SSH daemon configuration. Curious how to do that? Let's get your hand dirty :D

Basically, SSH daemon configuration located on `/etc/ssh/sshd_config`, Let's take a look line by line changes:

### Port

SSH usually run on port 22 which is the default port for ssh connection. For better security, you can change it into an unusual port.

{% highlight text %}
Port 22
{% endhighlight %}

become:

{% highlight text %}
Port 27389 # choose any number
{% endhighlight %}

*You can keep it to run on port under 1024 to ensure that only root can run the sshd*

### Root? Sorry

Allowing root login is a mistake, real mistake. You can turn the ability for root to login off. Change the line:

{% highlight text %}
PermitRootLogin yes
{% endhighlight %}

to:

{% highlight text %}
PermitRootLogin no
{% endhighlight %}

### Password is not on the keyboard

Always create user without password to ensure there is no chance for brute force attack or any other similar attacks (by guessing username and password pair). You can do that by:

{% highlight bash %}
% adduser --home /home/rico --shell /bin/bash --disabled-password rico
{% endhighlight %}

Since the user have no password, you can now disable password authentication for SSH which lead more secure server:

{% highlight text %}
PasswordAuthentication yes
{% endhighlight %}

become:

{% highlight text %}
PasswordAuthentication no
{% endhighlight %}

You might wondering, if user don't have any password, how you can log in as the user? The answer is [public key authentication](https://hkn.eecs.berkeley.edu/~dhsu/ssh_public_key_howto.html)

### Allowed Users

Next, you can whitelist users who able to log in using ssh by putting their name manually on configuration. If a user is not listed, he will not able to login.

{% highlight text %}
AllowUsers rico kowalski
{% endhighlight %}

Adding these line will make only rico and kowalski that able to log in. Other users will be unable to open the door.

### Conclusion

The small changes of `sshd_config` is dramatically increase our server security and prevent attacker for gain access into our server. Hope this help and please let me know if you have any addition to increase security of server related ssh daemon configuration.

