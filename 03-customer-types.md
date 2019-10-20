Parâmetros de Entrada e Saída
===============================

## 1. Definindo os parâmetros de entrada ##

As informações podem ser recebidas de diferentes formas:
- Cabeçalho (header)
- URI
- Query string
- Corpo da requisição (body)

Por exemplo:

    GET http://customer-api/api/customers/122?format=csv
    client_id: fabricio

Temos os parâmetros de entrada:
- 122
- format=csv
- client_id: fabricio

Essas informações podem ser representadas por:

```yaml
/customers:
  /{customer_id}:
    uriParameters:
      customer_id: number   # 122
    get:
      headers:
        client_id: string   # fabricio
      queryParameters:
        format?: string      # csv
```

## 2. Identificando tipos de dados ##

É possível enviar informações no corpo da mensagem:

```yaml
/customers:
  post:
    body:
      properties:
        customer_id: string
        name: string
        city: string
        state: string
        country: string
```

Embora seja possível fazer isso, podemos encapsular essa informação em um
tipo de dados específico:

```yaml
types:
  Customer:
    type: object
    properties:
      customer_id: string
      name: string
      city: string
      state: string
      country: string
```

Em seguida, podemos aproveitar a definição:

```yaml
/customers:
  post:
    body:
      type: Customer
```

## 3. Definindo tipos de dados ##

Podemos definir tipos complexos:

```yaml
types:
  Customer:
    type: object
```

Assim, especificamos os parâmetros complexos:

```yaml
/customers:
  get:
    responses:
      200:
        body: Customer[]
  post:
    body: Customer
    responses:
      201:
```

## 4. Especificando os dados ##

```yaml
prop:
  type: string
  pattern: ^[A-Z]+$
```

## 5. Criando exemplos ##



## 6. Organizando em arquivos ##

É possível separar as definições em arquivos distintos:

O arquivo DataType descreve o conteúdo do tipo, sem detalhar o nome do tipo.

```yaml
#%RAML 1.0 DataType
type: object
properties:
  customer_id: string
  name: string
  city: string
  state: string
  country: string
```

Outra forma de incorporar informações externas é através do Library:

```yaml
#%RAML 1.0 Library

types:
  Customer:
    type: object
    properties:
      customer_id: string
      name: string
      city: string
      state: string
      country: string
```

A referência é feita sem usar `!include`:

```yaml
uses:
  Types: 04-customer-library.raml
```
