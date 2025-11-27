# HTTP Crash Course

HTTP - hypertext transfer protocol
URL - Uniform resource locator
URI - Uniform resource identifier
URN - Uniform resource name

### What are http headers?

- metadata: key-value sent along with request and response
- caching, authentication, manage state
- x-prefix -> 2012 (x-deprecated)
- Request headers -> from client
- Response headers -> from server
- Representation headers -> encoding/compression
- payload headers -> data

## Most common headers

- Accept: application/json
- user-agent
- authorization
- content-type
- cookie
- cache-control

## Cors

- Access-control-allow-origin
- Access-control-allow-credentials
- Access-control-allow-method

## Security

- cross-origin-embedded-policy
- cross-origin-opener-policy
- content-security-policy
- x-xss-protection

## HTTP methods

Basic set of operations that can be used to interact with server.

- GET: retrieve a resource
- HEAD: No message body(response headers only)
- OPTIONS: what operations are available
- TRACE: lookback test(get some data)
- DELETE: remove a resource
- PUT: replace a resource
- PATCH: change part of a resource
- POST: interact with resource (mostly add)

## HTTP status code

```
1xx  -  Informational
2xx  -  Success
3xx  -  Redirection
4xx  -  Client error
5xx  -  Server error

100 - continue
102 - processing
200 - ok
201 - created
202 - accepted
307 - temporary redirect
308 - permanent redirect
400 - Bad request
401 - unauthorized
402 - payment required
404 - Not found
500 - Internal server error
504 - Gateway Timeout
```