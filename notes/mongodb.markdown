---
title: Notes about MongoDB
layout: page
permalink: /notes/mongodb/
---

[MongoDB](http://www.mongodb.org/) is a open source, scalable, high-performance, schema-free, document-oriented database written in C++. It's designed to build and run applications today.

## GridFS

*Hint:* GridFS is convention and an api, not something that natively provide by server.

### Calculate file md5 on server

You can validate file md5 on server. It can be useful when ensuring saved file with actual file integrity.

{% highlight javascript %}
> db['fs.files'].runCommand({ filemd5: ObjectId("51de33e53b80a11401000001"), root: "fs" })
{% endhighlight %}

And the response is:

{% highlight json %}
{
    "numChunks" : 3,
    "md5" : "7fe6bd7215b2e324618117258ed3cec9",
    "ok" : 1
}
{% endhighlight %}
