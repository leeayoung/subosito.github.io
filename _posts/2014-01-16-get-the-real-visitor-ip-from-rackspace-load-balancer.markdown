---
title: Get the Real Visitor IP From Rackspace Load Balancer
layout: post
excerpt: By default Rackspace load balancer replace visitor IP with its private IP. Here's how to get the visitor IP back!
redirect_from:
  - /2014/01/get-the-real-visitor-ip-from-rackspace-load-balancer
---

On my previous post, I show you about [the ability of Rackspace load balancer to redirect HTTP request to HTTPS one]({% post_url 2014-01-07-redirect-http-to-https-on-rackspace-load-balancer %}). One thing you might realized that the remote visitor IP that you get is only the private IP of your load balancer. If you have a load balancer that listen protocol HTTPS, you need to change it to listen HTTP (80).

Thankfully Rackspace support SSL Termination, it means, although you change your Load Balancer protocol to HTTP, it's still able to serve HTTPS request. The other benefits is you don't need to set SSL on your servers (nodes), just set it to listen port HTTP (80), and everything will be on the good way to go.

### Reset httpsRedirect Value

Before changing Load Balancer protocol from HTTPS to HTTP, you need to ensure that your LB has no `httpsRedirect: true` attributes. As you can see on my previous post, you can simply perform:

```
PUT https://dfw.loadbalancers.api.rackspacecloud.com/v1.0/123456/loadbalancers/5678

Content-Type: application/json
Accept: application/json
X-Auth-Token: ad95275a06384ccb979afb7797e90672

{
    "loadBalancer": {
        "httpsRedirect": false
    }
}
```

### Remove Load Balancer Nodes

After `httpsRedirect` value resetted, then you can remove nodes on your Load Balancer. You can use Rackspace control panel to do that:

![Remove Rackspace Load Balancer Nodes](/assets/posts/rackspace-load-balancer-nodes-https.png)

### Change Protocol:Port to HTTP

You can now set the Protocol:Port from HTTPS/443 to HTTP/80. So your Load Balancer will listen standard HTTP request.

![Update Rackspace Load Balancer Protocol](/assets/posts/rackspace-set-load-balancer-protocol.png)

### Setup the SSL Termination

To allow Load Balancer keep serving HTTPS request, you need to set SSL Termination. You can choose between "Allow secure and insecure traffic" or "Only allow secure traffic". If you plan only redirect HTTP to HTTPS, then you can choose "Only allow secure traffic".

![Setup Rackspace Load Balancer SSL Termination](/assets/posts/rackspace-set-load-balancer-ssl-termination.png)

### Add Load Balancer Nodes

You can now re-add your servers (nodes). As you can see, now they are now listen port 80 (HTTP).

![Rackspace Load Balancer HTTP Protocol](/assets/posts/rackspace-load-balancer-nodes-http.png)

Ensure you already remove SSL related configuration on your server, say you are using nginx, you might set:

```diff
server {
-        listen 443;
-
-        ssl on;
-        ssl_certificate /etc/ssl/example.com.crt;
-        ssl_certificate_key /etc/ssl/example.com.key;
+        listen 80;

         root /srv/bizarre/current;
         server_name example.com;
         index index.htm index.html;
```

### Put httpsRedirect Back

To have automatic redirection from HTTP to HTTPS, you need to enable `httpsRequest`, I am sure you already know how :)

```
PUT https://dfw.loadbalancers.api.rackspacecloud.com/v1.0/123456/loadbalancers/5678

Content-Type: application/json
Accept: application/json
X-Auth-Token: ad95275a06384ccb979afb7797e90672

{
    "loadBalancer": {
        "httpsRedirect": true
    }
}
```

Good job!

### Setup X-Forwarded-For

Until this step you might see that your nginx log still having Load Balancer private IP:

```
10.183.248.12 - - [15/Jan/2014:02:36:18 +0000] "GET / HTTP/1.1" 200 512 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.5; rv:16.0) Gecko/20100101 Firefox/16.0"
```

The last step, you need to tell your nginx server to show the real IP of your visitor. At this step, we need to utilize [nginx_http_realip_module](http://nginx.org/en/docs/http/ngx_http_realip_module.html). On Ubuntu system, you need to install at least `nginx-full` or `nginx-extras` to have this module.

Once installed then you can add these lines on your nginx section:

```
set_real_ip_from 10.183.248.0/22;
real_ip_header X-Forwarded-For;
```

The value of `set_real_ip_from` is differ for each Load Balancer locations.

| Load Balancer Region  | Address         |
|-----------------------|-----------------|
| Dallas                | 10.183.248.0/22 |
| Chicago               | 10.183.252.0/23 |
| London                | 10.190.254.0/23 |
| Hongkong              | 10.189.254.0/23 |

If you pretty sure, you can also using your Load Balancer IP directly like:

```
set_real_ip_from 10.183.248.12;
real_ip_header X-Forwarded-For;
```

Once you restart your nginx server, you'll see something like:

```
202.67.40.7 - - [15/Jan/2014:02:48:21 +0000] "GET / HTTP/1.1" 200 512 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.5; rv:16.0) Gecko/20100101 Firefox/16.0"
```

Yeah, you got the real visitor IP address. Please let me know if you have any question or correction :)

