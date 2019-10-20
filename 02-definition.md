Definição da API
==================

## 1. Resposta HTTP ##

Tradicionalmente, seguimos o Status Code padrão HTTP:

- 1xx (Info): Sinaliza que a requisição (parcial) foi recebida com sucesso
              e espera pelo resto das informações.

- 2xx (Sucesso): A requisição recebida teve sucesso no processamento

- 3xx (Redirect): Passos adicionais são necessários, normalmente corresponde
              a um redirecionamento da requisição

- 4xx (Erro Cliente): A requisição apresenta erros (ex: sintaxe) e falhou

- 5xx (Erro Servidor): A requisição falhou (temporariamente) por um erro do servidor

Referência:

  RFC 7231: Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content
  https://tools.ietf.org/html/rfc7231

Resumo:
- HTTP 200: Sucesso
- HTTP 400: Erro
- HTTP 500: Exceção

Identificamos os casos de sucesso/falha na hierarquia:

- GET /customers: Sucesso
- POST /customers: Sucesso
- GET /customers/{customer_id}: Sucesso/Falha
- PUT /customers/{customer_id}: Sucesso
- DELETE /customers/{customer_id}: Sucesso
- GET /customers/{customer_id}/accounts: Sucesso

Exemplo:

```
/customers:
  get:
    responses:
      200:
  /{customer_id}:
    get:
      responses:
        200:
        400:
```

## 2. Detalhamento do Sucesso ##

O retorno de sucesso é sempre 2xx. Podemos ser mais precisos com relação
ao resultado:

GET, HEAD:
- HTTP 200 Ok: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200

POST:
- HTTP 201 Created: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/201
- HTTP 202 Accepted: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/202

PUT:
- HTTP 201 Created: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/201
- HTTP 204 No Content: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/204

DELETE:
- HTTP 204 No Content: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/204

```

/customers:
  get:
    responses:
      200:
  post:
    responses:
      201:
  /{customer_id}:
    get:
      responses:
        200:
        400:
    put:
      responses:
        201:
        204:
    delete:
      responses:
        204:          
    /accounts:
      get:
        responses:
          200:
```

Na dúvida, podemos retornar o genérico HTTP 200 para todas as requisições com sucesso.


Error codes:

HTTP 404 Not Found
HTTP 410 Gone
