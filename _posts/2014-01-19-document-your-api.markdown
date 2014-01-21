---
title: Document Your API
layout: post
excerpt: Make documentation for your API to make your users or partners life easier.
---

Having API built and running is a pleasure. Next step is about making users or third parties consume the API. There is no other way other than documenting your API. Let them know how to consume your API and everything will be going wild _(for good reason)_.

Many tools are available for documenting your API. You can directly writing your API documentation within your code comments, like [Swagger](http://swagger.wordnik.com/) did. Or you can write a dedicated preformatted document with your API definitions and responses. This post is going to talk with later one.

One tool for writing preformatted document for your API easier is [API Blueprint](http://apiblueprint.org/). It's developed by smart folks at [Apiary](http://apiary.io/). Writing API documentation using API blueprint format makes you allow to export your documentation into browsable HTML page or JSON AST format. If you use Apiary service, you'll also get usable mocking server which you can use for your development.

## API Blueprint

Basically API Blueprint is extending markdown syntax to generate API documentation with custom parser called [Snowcrash](https://github.com/apiaryio/snowcrash). Snowcrash will transform API documentation in markdown into JSON/YAML AST format.

JSON/YAML AST form will be transformed into HTML by using tools like [iglo](https://github.com/subosito/iglo) or [aglio](https://github.com/danielgtaylor/aglio). Both tools are able to convert directly from API markdown and perform transforming into AST in the background. So you don't have to worry about that.

API Blueprint syntax is easy. If you already know about markdown then you just need to learn about the concepts. Apiary provides [tutorial about API Blueprint](http://apiary.io/blueprint) which you can read. I won't rewrite that here.

Here's some notes related to API Blueprint:

- You can have multiple responses for your `Response`. It's must have unique code for each response.

```
+ Response 200 (application/json)
    [Purchase][]

+ Response 402 (application/json)
    ```
    {"error": "purchase failure"}
    ```
```

- Don't return body when you intended body-less response code, eg: [204](http://httpstatus.es/204).

```
+ Response 204
+ Response 200
    [Model][]
```

- Use `Prefer` header when if you want to return specific response code on Apiary mocking server:

```
Prefer: 401
```

- Always design your API as RESTful. It will makes you documents your API using API Blueprint easier.

## Conclusion

Take some time to documents your API. Think like your API is first class citizen for your application. Let your users consume and experiment with your API confidently.





