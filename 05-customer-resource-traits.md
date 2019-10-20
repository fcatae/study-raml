Resource Types e Traits
==========================

Definindo propriedades comuns.

Exemplo: coleção.

```yaml
resourceTypes:
  collection:
    get:
    post:

/customers:
  type: collection
```

Customiza as informações através de parâmetros de pré-compilação.

```yaml
resourceTypes:
  collection:
    get:
      displayName: Lista<<itemName | !singularize >>
    post:
      displayName: Incluir<<itemName | !singularize >>

/customers:
  type:
    collection:
      itemName: Clientes
```

É possível escrever de diferentes formas:

```yaml
/customers:
  type:
    collection:
      itemName: Clientes

  /{customer_id}:
    type: { collectionItem: { itemName : Cliente } }
```

De forma semelhante, é possível definir Traits:

```yaml
traits:
  hasAcceptHeader:
    headers:
      Accept: string
```

Os métodos podem ser marcados com `is`.

```yaml
/download:
  get:
    is: [ hasAcceptHeader ]
```

Outro exemplo é a propriedade Cacheable.

```yaml
cacheable:
  headers:
    Cache-Control:
      example: private, max-age=31536000
    Expires:
      type: datetime
      example: Sun, 20 Oct 2019 20:00:00 GMT
      format: rfc2616
```

Assim, podemos definir:

```yaml
/download:
  get:
    is:
      - hasAcceptHeader
      - cacheable
```

Igualmente podemos configurar a forma de autenticação/autorização:

```yaml
securitySchemes: 
  customTokenSecurity:
    type: x-customToken
    describedBy: 
      headers: 
        Authorization:
```

Depois configuramos para os recursos:

```yaml
securedBy:
  - customTokenSecurity
```