Documentation
===============

## 1. Create a document ##

Criar um fragmento da documentação:

```yaml
#%RAML 1.0 DocumentationItem
title: Getting Started
content: |
  **Customer API** corresponde a api de cliente.
```

## 2. Link the documentation ##

Incluir a lista da documentação:

```yaml
documentation:
  - !include 04-documentation.raml
```
