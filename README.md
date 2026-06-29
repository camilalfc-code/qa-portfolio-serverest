# 🧪 ServeRest — Testes de API com Postman

Projeto de testes de API REST desenvolvido com Postman utilizando a API pública [ServeRest](https://serverest.dev). O objetivo é demonstrar conhecimentos em validação de endpoints, automação de testes, encadeamento de dados e análise de respostas HTTP.

---

## 📋 Sobre o projeto

Esta collection foi criada para validar funcionalidades de autenticação e gerenciamento de usuários da API ServeRest.

Foram implementados cenários positivos e negativos, incluindo validações de status code, mensagens de retorno, estrutura da resposta e utilização de variáveis de coleção para encadeamento de dados entre requisições.

---

## 📁 Estrutura da collection

```
ServeRest — Testes de API
├── 📁 Login
│   ├── POST Login — credenciais válidas
│   ├── POST Login — credenciais inválidas
│   ├── POST Login — email vazio
│   ├── POST Login — email incorreto
│   ├── POST Login — email sem @
│   ├── POST Login — email sem domínio
│   ├── POST Login — email tipo int
│   ├── POST Login — email uppercase
│   ├── POST Login — email com muitos caracteres
│   ├── POST Login — password vazio
│   ├── POST Login — password incorreto
│   ├── POST Login — password tipo int
│   ├── POST Login — password com muitos caracteres
│   └── POST Login — password uppercase
└── 📁 Usuários
    ├── GET Listar Usuários
    ├── POST Cadastrar Usuário
    ├── GET Buscar Usuário por ID
    └── GET Buscar Usuário por ID — ID inválido
```

---

## ✅ Cobertura de testes — Login

| # | Cenário (BDD) | Método | Status Esperado | Resultado |
|---|---------------|--------|-----------------|-----------|
| 01 | **Dado** que o usuário informa credenciais válidas<br>**Quando** realiza o login<br>**Então** deve receber status 200 e token de autenticação | POST | 200 | ✅ PASSED |
| 02 | **Dado** que o usuário informa credenciais inválidas<br>**Quando** realiza o login<br>**Então** deve receber status 401 e mensagem de erro | POST | 401 | ✅ PASSED |
| 03 | **Dado** que o usuário não informa o email<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 e mensagem "email não pode ficar em branco" | POST | 400 | ✅ PASSED |
| 04 | **Dado** que o usuário informa um email não cadastrado<br>**Quando** tenta fazer login<br>**Então** deve receber status 401 e mensagem de credenciais inválidas | POST | 401 | ✅ PASSED |
| 05 | **Dado** que o usuário informa um email sem @<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 e mensagem "email deve ser um email válido" | POST | 400 | ✅ PASSED |
| 06 | **Dado** que o usuário informa um email sem domínio (.com)<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 e mensagem "email deve ser um email válido" | POST | 400 | ✅ PASSED |
| 07 | **Dado** que o usuário informa um valor inteiro no campo email<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 indicando campo obrigatório | POST | 400 | ✅ PASSED |
| 08 | **Dado** que o usuário informa o email em letras maiúsculas<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 indicando campo obrigatório | POST | 400 | ✅ PASSED |
| 09 | **Dado** que o usuário informa um email com quantidade excessiva de caracteres<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 indicando campo obrigatório | POST | 400 | ✅ PASSED |
| 10 | **Dado** que o usuário não informa a senha<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 e mensagem "password não pode ficar em branco" | POST | 400 | ✅ PASSED |
| 11 | **Dado** que o usuário informa uma senha incorreta<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 e mensagem de erro | POST | 400 | ✅ PASSED |
| 12 | **Dado** que o usuário informa um valor inteiro no campo password<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 indicando campo obrigatório | POST | 400 | ✅ PASSED |
| 13 | **Dado** que o usuário informa uma senha com quantidade excessiva de caracteres<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 indicando campo obrigatório | POST | 400 | ✅ PASSED |
| 14 | **Dado** que o usuário informa a senha em letras maiúsculas<br>**Quando** tenta fazer login<br>**Então** deve receber status 400 indicando campo obrigatório | POST | 400 | ✅ PASSED |

---

## ✅ Cobertura de testes — Usuários

| # | Cenário (BDD) | Método | Status Esperado | Resultado |
|---|---|---|---|---|
| 15 | **Dado** que existem usuários cadastrados<br>**Quando** realiza a listagem<br>**Então** deve receber status 200 e lista de usuários | GET | 200 | ✅ PASSED |
| 16 | **Dado** que o usuário informa dados válidos<br>**Quando** realiza o cadastro<br>**Então** deve receber status 201 e mensagem de sucesso | POST | 201 | ✅ PASSED |
| 17 | **Dado** que existe um usuário cadastrado<br>**Quando** busca pelo ID correto<br>**Então** deve receber status 200 e dados do usuário | GET | 200 | ✅ PASSED |
| 18 | **Dado** que o ID informado não existe<br>**Quando** tenta buscar o usuário<br>**Então** deve receber status 400 e mensagem de erro | GET | 400 | ✅ PASSED |
| 19 | **Dado** que o email já está cadastrado<br>**Quando** tenta cadastrar novamente<br>**Então** deve receber status 400 e mensagem "Este email já está sendo usado" | POST | 400 | ✅ PASSED |
| 20 | **Dado** que os campos obrigatórios estão vazios<br>**Quando** tenta cadastrar usuário<br>**Então** deve receber status 400 com mensagens de campo obrigatório | POST | 400 | ✅ PASSED |
| 21 | **Dado** que o usuário existe<br>**Quando** edita com dados válidos<br>**Então** deve receber status 200 e mensagem "Registro alterado com sucesso" | PUT | 200 | ✅ PASSED |
| 22 | **Dado** que o usuário existe<br>**Quando** realiza o delete<br>**Então** deve receber status 200 ou 400 conforme comportamento da API pública | DELETE | 200/400 | ✅ PASSED |
---

## 📊 Resultado da execução

| Total de requisições | Total de testes | Aprovados | Reprovados |
|----------------------|-----------------|-----------|------------|
| 18 | 36 | 36 | 0 |

<img width="1917" height="982" alt="Captura de tela 2026-06-29 184430" src="https://github.com/user-attachments/assets/4706aa7e-1dc5-4b7e-b750-ddcaa340909b" />

---

## 🔗 Encadeamento de variáveis

Após o cadastro de um usuário, o valor do campo `_id` retornado pela API é armazenado automaticamente na variável de coleção:

```javascript
pm.collectionVariables.set("usuarioId", jsonData._id);
```

Essa variável é utilizada posteriormente na requisição **Buscar Usuário por ID**, simulando um fluxo real de testes integrados.

---

## 🔍 Observações sobre comportamento da API

Durante a execução dos testes, foram identificados comportamentos relevantes para documentação:

- **Email em uppercase**: a API trata letras maiúsculas no campo email como valor inválido, retornando "email é obrigatório" em vez de "email deve ser um email válido". Comportamento a ser questionado com o time de desenvolvimento.
- **Tipo int no lugar de string**: quando um valor inteiro é enviado nos campos email ou password, a API trata como campo vazio, retornando "campo é obrigatório".
- **Email com muitos caracteres**: emails com comprimento excessivo são rejeitados como campo obrigatório, sem mensagem específica de limite de caracteres.

---

## 🐛 Bug Reports

Durante a execução dos testes foram identificados e documentados bugs reais na API:

| ID | Título | Severidade | Status |
|---|---|---|---|
| [BUG-001](./bug-reports/BUG-001-serverest-mensagem-email-invalido.md) | Mensagem de erro incorreta para email inválido no login | Baixa | Aberto |
| [BUG-002](./bug-reports/BUG-002-serverest-status-code-senha-incorreta.md) | Status code incorreto para senha incorreta no login | Média | Aberto |


## 🛠️ Tecnologias utilizadas

- Postman
- ServeRest API
- JavaScript
- Collection Variables
- API REST
- JSON

---

## ▶️ Como executar

1. Baixe ou clone este repositório
2. Importe o arquivo `.postman_collection.json` no Postman
3. Abra o **Collection Runner**
4. Execute a collection completa
5. Verifique os resultados dos testes

---

## 🎯 Competências demonstradas

- Testes de API REST
- Automação de testes com Postman
- Escrita de cenários em BDD (Gherkin)
- Criação de cenários positivos e negativos
- Validação de respostas HTTP e status codes
- Testes de tipo de dado e limite de caracteres
- Manipulação de JSON
- Uso de variáveis de coleção
- Encadeamento de dados entre requisições
- JavaScript para testes automatizados



---

## 👩‍💻 Autora

**Camila Lopes**

Profissional em transição para a área de Quality Assurance (QA), desenvolvendo projetos práticos de testes manuais, APIs, SQL e automação.

🔗 [LinkedIn](https://www.linkedin.com/in/camila-lopes-ferreira-carvalho43235429/)
🔗 [GitHub](https://github.com/camilalfc-code)


