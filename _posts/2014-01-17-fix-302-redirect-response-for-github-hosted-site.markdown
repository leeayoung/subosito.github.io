---
title: Fix 302 Redirect Response For GitHub Hosted Site
layout: post
excerpt: Small mistake on setting up GitHub hosted site makes your site respond with 302 (Found) response instead of 200 (OK). This post show you how to fix that.
---

Blogging using [Jekyll](/notes/jekyll/) and hosted on GitHub is a free but absolutely great solution. In previous GitHub hosted site configuration, you need to set your domain to an A record to `207.97.227.245`.

```
$ dig subosito.com +nocomments +nocmd +nostats

; <<>> DiG 9.9.2-P2 <<>> subosito.com +nocomments +nocmd +nostats
;; global options: +cmd
;subosito.com.                  IN      A
subosito.com.           2929    IN      A       207.97.227.245
```

This is basically work, but if you take carefully about the response you'll see some weird behaviour.

```
$ curl -I http://subosito.com/notes/vim/
HTTP/1.1 302 Found
Connection: close
Pragma: no-cache
cache-control: no-cache
Location: /notes/vim/
```

As you can see, GitHub respond request with 302 (Found) instead of 200 (OK). This means each request need additional redirection. That's smell bad for your website SEO :(.

## Solution

Instead of using A record, you can use ALIAS record for your domain. What is ALIAS record? ALIAS record is simple record that enable to lookup the IP or the ALIAS value then returned that as the IP of your domain. Your user don't know or care how they got the IP address of your domain.

> Not all DNS server support ALIAS record. [DNSimple](https://dnsimple.com) and [PointHQ](https://pointhq.com/) are the one which known to support ALIAS record.

First, remove your A record which point to `207.97.227.245` (**you might have different IP**). Then create ALIAS record for your domain which point to `username.github.io`:

![PointHQ create ALIAS DNS record](/assets/posts/pointhq-create-alias-record.png)

You can wait a moment until DNS is propagated. Then you can verify it using `dig` command:

```
$ dig subosito.com +nocomments +nocmd +nostats

; <<>> DiG 9.8.1-P1 <<>> subosito.com +nocomments +nocmd +nostats
;; global options: +cmd
;subosito.com.                  IN      A
subosito.com.           3600    IN      A       185.31.18.133
```

If the IP value of your domain changes then you're ready to test your site. Otherwise you need to wait for more couple of minutes (or hours), please be patient ^^.

Finally you can now test your site:

```
$ curl -I http://subosito.com/notes/vim/
HTTP/1.1 200 OK
Server: GitHub.com
Content-Type: text/html
Last-Modified: Thu, 16 Jan 2014 16:09:53 GMT
Expires: Fri, 17 Jan 2014 11:31:27 GMT
Cache-Control: max-age=600
Content-Length: 5848
Accept-Ranges: bytes
Date: Fri, 17 Jan 2014 11:21:27 GMT
Via: 1.1 varnish
Age: 0
Connection: keep-alive
X-Served-By: cache-lcy1129-LCY
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1389957687.424375772,VS0,VE77
Vary: Accept-Encoding
```

See 200 (OK) response? Great!! Your website is on the right track.

You can always see [GitHub Pages with domain](https://help.github.com/articles/setting-up-a-custom-domain-with-pages) manual to get more details about GitHub address and ALIAS value.

