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

```yaml
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

```yaml
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

## 3. Detalhamento do Erro ##

Existem dois tipos de erros: 400 e 500. Os erros HTTP 400 são causados pelo cliente
e não devem ser reenviados novamente ao servidor.

Os casos mais comuns:
- 400: erros de sintaxe ou com informações incorretas
- 404: não encontrado (Not Found)

Exemplo:

```yaml
/customers:
  /{customer_id}:
    get:
      responses:
        404:
```

Entretanto, é possível especificar respostas

- HTTP 403 Forbidden
- HTTP 409 Conflict
- HTTP 410 Gone

Na dúvida, podemos usar o HTTP 400 genérico para indicar erro na construção da
requisição e que não deve ser reenviada ao servidor.

Erros 500 são normalmente temporários e causados do lado do servidor. O erro
pode ser uma falha de rede ou mesmo do servidor. Em geral, o cliente pode reenviar
a requisição mais tarde.

Existem alguns casos específicos:
- HTTP 501 Not Implemented
- HTTP 502 Bad Gateway
- HTTP 503 Service Unavailable
- HTTP 504 Gateway Timeout


## 4. Modelagem CRUD ##

Um exemplo semelhante ao banco de dados:

1. "SELECT * FROM cliente WHERE id = 123" - não existe o cliente 123
- retornar NULO (HTTP404 Not Found)
- retornar array com 0 registros (HTTP200 Ok)
- disparar Exception (HTTP500)

2. "INSERT cliente"
- retornar sucesso (HTTP201 Created)
- retornar sucesso: cliente já existente (HTTP204 No Content)
- retornar falha: cliente já existente (HTTP409 Conflict)
- disparar Exception: chave duplicada (HTTP500)

3. "DELETE cliente"
- retornar sucesso (HTTP204 No Content)
- retornar falha: permissão negada (HTTP403 Forbidden)
- retornar falha: cliente inexistente (HTTP410 Gone)
- disparar Exception: cliente inexistente (HTTP500)

4. "EXEC procedure"
- retornar sucesso (HTTP200 Ok)
- retornar ok: rodando assíncronamente (HTTP202 Accepted)

Lembrando que sempre podemos usar os tipos genéricos HTTP 200 e 400 para
definir o retorno.
