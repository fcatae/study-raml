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

3. Configure Default Media Type

```
mediaType: 
  - application/json
```

