---
title: Notes about Bootstrap
layout: page
permalink: /notes/bootstrap/
---

[Bootstrap](http://twitter.github.io/bootstrap/) is a open source front-end framework for faster and easier web development. It's sleek, intuitive, and powerful.

## Fix Google maps display

By default, you'll get messy controls when using Google maps within Bootstrap.

![Google maps bootstrap issue](/assets/img/bootstrap/google-maps-bootstrap-issue.png)

To fix Google maps controls display, you can add the style rules:

{% highlight css %}
.map_container img {
  max-width: none;
}

.map_container label {
  width: auto;
  display: inline;
}
{% endhighlight %}

Voila, the Google maps controls display will be:

![Google maps bootstrap](/assets/img/bootstrap/google-maps-bootstrap.png)

