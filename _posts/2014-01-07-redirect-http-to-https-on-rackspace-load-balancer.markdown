---
title: Redirect HTTP to HTTPS on Rackspace Load Balancer
layout: post
excerpt: Rackspace load balancer service is great. But to automatically redirect HTTP to HTTPS is tricky. Let's get dirty with Rackspace API to get this done.
---

Rackspace Load Balancer is great service, it can properly redirect request to your servers based on availability of the servers or randomly. When setting up Load Balancer you need to select which protocol/port that LB would listen. Say you want LB to serve HTTPS (443) request to your servers, then you can set LB as below:

![Create Rackspace Load Balancer](/assets/posts/rackspace-create-load-balancer.png)

Once completed then you'll have fully working LB for your domain. Don't forget to set your domain to the LB's IP.

![Rackspace Load Balancers](/assets/posts/rackspace-load-balancers.png)

Check that your domain is already pointed to LB's IP:

```
$ nslookup example.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
Name:   example.com
Address: 120.1.23.45
```

Then you can open your browser and go to https://example.com/. Congratulations, everything is working now :)

You might ask what happend if you just type `example.com` on your browser, which is basically open http://example.com/, the standard HTTP of your website. You'll see something like this:

![Chromium Website Unavailable](/assets/posts/chromium-website-unavailable.png)

You don't like it? you want to auto redirect HTTP request to your HTTPS request? If so, then the rest of this post is for you.

### Modify LB to redirect HTTP request to HTTPS

Basically, to set up the LB to auto redirect HTTP to HTTPS request couldn't be done within Rackspace Control Panel. Yeah, at the time of this writing, you need to setting up your lB via API. Don't worry, it's not difficult.

Go to Account Settings on top right bar on Rackspace Control Panel. Then copy your username and API key as you can see here:

![Rackspace Account Settings](/assets/posts/rackspace-account-settings.png)

Before further processing you need to get access token. You can do:

```
POST https://identity.api.rackspacecloud.com/v2.0/tokens

Content-Type: application/json
Accept: application/json

{
    "auth": {
        "RAX-KSKEY:apiKeyCredentials": {
            "username": "subosito",
            "apiKey": "xu5oong3ohvoufiequeingu3eu2gohye"
        }
    }
}
```

You'll get response like this (I trimmed the output):

```json
{
    "access": {
        "token": {
            "id": "beinam9soch3laerie9eidopeu7ii5os",
            "expires": "2014-01-06T03:38:52.008Z",
            "tenant": {
                "id": "123456",
                "name": "123456"
            },
            "RAX-AUTH:authenticatedBy": [
                "APIKEY"
            ]
        },
        "serviceCatalog": [
            {
                "name": "cloudLoadBalancers",
                "endpoints": [
                    {
                        "region": "DFW",
                        "tenantId": "123456",
                        "publicURL": "https://dfw.loadbalancers.api.rackspacecloud.com/v1.0/123456"
                    },
                    {
                        "region": "HKG",
                        "tenantId": "123456",
                        "publicURL": "https://hkg.loadbalancers.api.rackspacecloud.com/v1.0/123456"
                    }
                ],
                "type": "rax:load-balancer"
            }
        ]
    }
}
```

As you can see each region has different `publicURL`, make sure you point to url which has same region with LB that you want configure. Because our LB is located on Dallas, then we can use `DFW` one.

Next you need to get list of LB that you have, you can do that by:

```
GET https://dfw.loadbalancers.api.rackspacecloud.com/v1.0/123456/loadbalancers

Content-Type: application/json
Accept: application/json
X-Auth-Token: ad95275a06384ccb979afb7797e90672
```

And the response is:

```json
{
    "loadBalancers": [
        {
            "name": "Tiger LB Production",
            "id": 5678,
            "protocol": "HTTPS",
            "port": 443,
            "algorithm": "LEAST_CONNECTIONS",
            "status": "ACTIVE",
            "virtualIps": [
                {
                    "address": "120.1.23.45",
                    "id": 77,
                    "type": "PUBLIC",
                    "ipVersion": "IPV4"
                }
            ],
            "nodeCount": 1
        }
    ]
}
```

Once you get the `id` of your LB then you can perform `PUT` request to update your LB attributes:

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

Upon successful request you'll get `202 Accepted` response. You can verify if your LB is handle HTTP redirect properly by typing `http://example.com` on your browser. Or alternatively you can perform `GET` request to get the details of your LB:

```
GET https://dfw.loadbalancers.api.rackspacecloud.com/v1.0/123456/loadbalancers/5678

Content-Type: application/json
Accept: application/json
X-Auth-Token: ad95275a06384ccb979afb7797e90672
```

The response will be something like this:

```json
{
    "loadBalancer": {
        "name": "Tiger LB Production",
        "id": 5678,
        "protocol": "HTTPS",
        "port": 443,
        "algorithm": "LEAST_CONNECTIONS",
        "status": "ACTIVE",
        "virtualIps": [
            {
                "address": "120.1.23.45",
                "id": 77,
                "type": "PUBLIC",
                "ipVersion": "IPV4"
            }
        ],
        "nodeCount": 1,
        "httpsRedirect": true,
        "halfClosed": false,
        "connectionLogging": {
            "enabled": false
        },
        "contentCaching": {
            "enabled": false
        }
    }
}
```

As you can see, now your LB has attribute `"httpsRedirect": true`, it means that your LB will redirect any HTTP request to HTTPS :)

