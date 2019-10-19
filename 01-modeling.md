Modelagem
==========

## 1. Requisitos funcionais ##

Levantamento dos requisitos necessários:
- Listar todos clientes
- Registrar um novo cliente
- Obter informação de um cliente
- Atualizar informações de um cliente
- Remover informações de um cliente
- Obter a lista de todas as contas do cliente

## 2. Definição do recurso ##

Recurso: `/customers`

## 3. Definição dos métodos ##

Considerando a semântica associada aos métodos:
- Seguros (sem efeito colateral): GET, HEAD, OPTION, TRACE
- Idempotência em alteração: PUT, DELETE
- Alteração parcial: PATCH
- Inclusão/Execução: POST

Modelagem:
/customers (GET, POST)
/customers/{id} (GET, PUT, DELETE)
/customers/{id}/accounts (GET)

## 4. Criação da API ##

Definição inicial da API contendo título e versão, incluindo URI.

```yaml
title: Customer API
version: v1
baseUri: http://proj-customer.us-e2.cloudhub.io/api
mediaType:
  - application/json
```

## 5. Definição da hierarquia ##

Inicialmente, queremos representar o modelo usando recursos:

```yaml
/customers:
  /{customer_id}:
    /accounts:
```

Em seguida, definimos os métodos:

```yaml
/customers:
  get:
  post:
  /{customer_id}:
    get:
    put:
    delete:
    /accounts:
      get:
```
