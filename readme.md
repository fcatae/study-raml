RAML 1.0 Spec: https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md/

Tutorial: https://raml.org/developers/raml-100-tutorial

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

