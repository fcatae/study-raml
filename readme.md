RAML 1.0 Spec: https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md/

Tutorial 100: https://raml.org/developers/raml-100-tutorial

Tutorial 200: https://raml.org/developers/raml-200-tutorial

1. Define Resources

```
/users:
  /authors:
    /{authorname}:
  /books:
```

2. Define Methods

```
/users:
  /authors:
  /books:
    get:
    post:
```

Semantics: 
- Safe methods: GET, HEAD, OPTION, TRACE
- Idempotent methods: PUT, DELETE

3. Configure Default Media Type

```
mediaType: 
  - application/json
```

4. Response types

Status code:

- 1xx (Informational): The request was received, continuing process

- 2xx (Successful): The request was successfully received, understood, and accepted

- 3xx (Redirection): Further action needs to be taken in order to complete the request

- 4xx (Client Error): The request contains bad syntax or cannot be fulfilled

- 5xx (Server Error): The server failed to fulfill an apparently valid request

GET, HEAD:
HTTP 200 Ok: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200

POST:
HTTP 201 Created: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/201

HTTP 202 Accepted: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/202

PUT:
HTTP 204 No Content: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/204

```
/users:
  get:
    responses:
      200:
```

Error codes:

HTTP 404 Not Found
HTTP 410 Gone


5. Define body

```
body:
  properties:
    message: string
    status: number
```

6. Optional response type

```
headers:
  Accept?:

responses:
  200:
    body:
      application/json:
      application/xml:
  406:
    body:
      properties:
        message: string
```

7. Optional cache

```
responses:
  200:
    headers:
      Cache-Control:
      Expires: 
        type: datetime
```

8. Replace generic body with typed-body

```
types:
  messageType: !include message.raml

...

body:
  type: messageType
```

9. Validate data types

```
currency:
  type: string
  pattern: ^[A-Z]{3,3}$
```

10. Create Named Examples

```
#%RAML 1.0 NamedExample
value:
  nome: fabricio
```

Use both data type and example:

```
#%RAML 1.0 DataType
type: object
properties:
  nome: string
  idade: number
  altura?: number
example:
  nome: fabio
  idade: 15
```

Library
=========

1. Resource Type

```
#%RAML 1.0 ResourceType
get:
post?:
```

Uso:

```
#%RAML 1.0

resourceTypes:
  collection: !include resource.raml

/test:
  type: collection
```

