---
title: Svbtle's Missing Features
layout: post
excerpt: List of features that Svbtle's missing.
---

Svbtle is a nice blogging platform, it has enough features for anyone who want to write online. Having several days using Svbtle, here is my list of things that Svbtle could be improved:

## Support for analytics other than Google Analytics

Having support for another analytics providers will makes user like me to have better picture about users that visits my website. Personally, I prefer [Clicky](https://clicky.com/) for analytics purpose, while others might be Mixpanel, Piwik, or something else.

## Support table in markdown syntax

Sometimes we need to insert table into our article. Using markdown that's simply using:

```
| Table Header | Another Header |
|--------------|----------------|
| The Value    | Hello World    |
```

But that's is not valid within Svbtle. I think Svbtle need to update to support [markdown extra](http://en.wikipedia.org/wiki/Markdown_Extra), so we will have more feature to express ourself in writing.

## Writing markdown using fixed-size font

Using proxima-nova or sans-serif family for writing markdown is not pleasant, especially when we are dealing with `code`. See the differences:

[![Markdown sans-serif](https://d23f6h5jpj26xu.cloudfront.net/y3ki7b81b0c7gq_small.png)](http://img.svbtle.com/y3ki7b81b0c7gq.png)

and

[![Markdown fixed-size](https://d23f6h5jpj26xu.cloudfront.net/s3sq5j1faeowq_small.png)](http://img.svbtle.com/s3sq5j1faeowq.png)

Which do you think is better?

> Tips: You can using [userstyles](http://userstyles.org/) to customize your editor view. You can add new style with:

```
textarea#post_content {
  font-family: Consolas, "Liberation Mono", Courier, monospace !important;
}
```

## Syntax highlighting markdown editor

For someone who have experience with markdown, they will be easy to spot an error syntax. For who doesn't have such experience, that's pretty tricky. Adding syntax highlighting when writing markdown will be helpful to catch the error easily.

[![Markdown highlight](https://d23f6h5jpj26xu.cloudfront.net/lxzomqueaem4ua_small.png)](http://img.svbtle.com/lxzomqueaem4ua.png)

## Better OpenGraph implementation

If you test your Svbtle website using https://developers.facebook.com/tools/debug/. You'll see an error like:

[![OpenGraph Debug](https://d23f6h5jpj26xu.cloudfront.net/cvdoja6z9kwsmw_small.png)](http://img.svbtle.com/cvdoja6z9kwsmw.png)

The error exist because on each website hosted on Svbtle has `meta` entry:

```
<meta property="fb:app_id" content="346346195413177">
```

Which originally on its application setting, `App Domain` configured to points to svbtle.com _(not sure, I guess)_.

## Makes fonts from typekit loaded properly

Svbtle heading is using [freight sans pro](https://typekit.com/fonts/freight-sans-pro) and [proxima nova](https://typekit.com/fonts/proxima-nova) for its paragraph. Somehow those fonts don't show up on my website.

[![Svbtle Typekit Font Not Loaded](https://d23f6h5jpj26xu.cloudfront.net/g0lwvdynweevxa_small.png)](http://img.svbtle.com/g0lwvdynweevxa.png)

If you have same experience, then we need to let Svbtle to dig more about it.

## Export ideas into zip or any other form

Yeah, exporting our content from Svbtle is essential. We can have local copy on our computer for our archive. Having an exported zip with all our contents and its images would be a huge pleasure.

[![Svbtle export](https://d23f6h5jpj26xu.cloudfront.net/d08kqcmnlkhla_small.png)](http://img.svbtle.com/d08kqcmnlkhla.png)

## Don't track owner activity while logged in
Things that Could be Improved by
While you logged in and then you open your own article. Try to see your Google Analytics Realtime Dashboard:

[![Google Analytics Realtime Overview](https://d23f6h5jpj26xu.cloudfront.net/tamnddo59xlsa_small.png)](http://img.svbtle.com/tamnddo59xlsa.png)

There is one visitor, yeah, that's you. You're the visitor of your blog while you're logged in. That's seems bad for the analytics data.

> Tips: You can use Chrome extension [Block Yourself from Analytics](https://chrome.google.com/webstore/detail/block-yourself-from-analy/fadgflmigmogfionelcpalhohefbnehm?hl=en) to exclude your website for being tracked.

## Avatar as favicon

Uploading a white transparent png as avatar is nice feature of Svbtle. But the favicon do not reflect this avatar setting. Instead of black rounded logo as favicon, having user avatar as favicon seems a nice improvement.

[![Svbtle avatar setting](https://d23f6h5jpj26xu.cloudfront.net/vzekod0c48czva_small.png)](http://img.svbtle.com/vzekod0c48czva.png)

## Simple share button

Although we can share easily by copy paste title and URL. I am thinking to reduce copy/paste requirement, a simple click to share is nice time saving. Perhaps using format like `kudos`, wait a couple of seconds to share?

> I saw some users are embedding facebook's like and twitter's tweet into their post content.

-----

I will updates this post to reflect changes that made by Svbtle. If you have any other suggestion about Svbtle improvements, please let me know on [Twitter](https://twitter.com/subosito).

> Discuss on [Hacker News](https://news.ycombinator.com/item?id=7107574)
