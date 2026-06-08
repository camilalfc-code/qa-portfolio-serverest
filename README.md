# 🧪 ServeRest — Testes de API com Postman

Projeto de testes de API REST desenvolvido com Postman utilizando a API pública ServeRest. O objetivo é demonstrar conhecimentos em validação de endpoints, automação de testes, encadeamento de dados e análise de respostas HTTP.

## 📋 Sobre o projeto

Esta collection foi criada para validar funcionalidades de autenticação e gerenciamento de usuários da API ServeRest.

Foram implementados cenários positivos e negativos, incluindo validações de status code, mensagens de retorno, estrutura da resposta e utilização de variáveis de coleção para encadeamento de dados entre requisições.

## ✅ Cobertura de testes

| Request                        | Método | Status Esperado | Testes |
| ------------------------------ | ------ | --------------- | ------ |
| Listar Usuários                | GET    | 200             | 1      |
| Login — credenciais válidas    | POST   | 200             | 2      |
| Login — credenciais inválidas  | POST   | 401             | 2      |
| Cadastrar Usuário              | POST   | 201             | 2      |
| Buscar Usuário por ID          | GET    | 200             | 3      |
| Buscar Usuário por ID inválido | GET    | 400             | 2      |

**Total:**

* 6 requisições
* 12 testes automatizados
* Cenários positivos e negativos
* Encadeamento de variáveis entre requests

## 🔗 Encadeamento de variáveis

Após o cadastro de um usuário, o valor do campo `_id` retornado pela API é armazenado automaticamente na variável de coleção:

```javascript
pm.collectionVariables.set("usuarioId", jsonData._id);
```

Essa variável é utilizada posteriormente na requisição **Buscar Usuário por ID**, simulando um fluxo real de testes integrados.

## 🧪 Validações implementadas

Os testes automatizados verificam:

* Status codes HTTP
* Mensagens de sucesso e erro
* Existência de campos obrigatórios
* Retorno do token de autenticação
* Persistência correta do ID do usuário
* Consistência dos dados retornados pela API

## 🛠️ Tecnologias utilizadas

* Postman
* ServeRest API
* JavaScript
* Collection Variables
* API REST
* JSON

## ▶️ Como executar

1. Baixe ou clone este repositório.
2. Importe o arquivo da collection no Postman.
3. Abra o Collection Runner.
4. Execute a collection completa.
5. Verifique os resultados dos testes.

## 📸 Evidências

### Execução da Collection

> Adicionar print do Collection Runner com todos os testes aprovados.

### Login com sucesso

> Adicionar print da resposta contendo token de autenticação.

### Cadastro de usuário

> Adicionar print da resposta contendo mensagem de sucesso e ID gerado.

## 🎯 Competências demonstradas

* Testes de API REST
* Automação de testes com Postman
* Criação de cenários positivos e negativos
* Validação de respostas HTTP
* Manipulação de JSON
* Uso de variáveis de coleção
* Encadeamento de dados entre requisições
* JavaScript para testes automatizados

## 👩‍💻 Autora

**Camila Lopes**

Profissional em transição para a área de Quality Assurance (QA), desenvolvendo projetos práticos de testes manuais, APIs, SQL e automação.

🔗 LinkedIn: (https://www.linkedin.com/in/camila-lopes-ferreira-carvalho43235429/)

🔗 GitHub: https://github.com/camilalfc-code
