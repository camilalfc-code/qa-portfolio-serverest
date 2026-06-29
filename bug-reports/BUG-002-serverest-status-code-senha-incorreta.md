# BUG-002 — Status code incorreto para senha incorreta no login

## Informações gerais

| Campo | Valor |
|---|---|
| **ID** | BUG-002 |
| **Título** | API retorna status 400 para senha incorreta em vez de 401 |
| **Módulo** | Login |
| **Endpoint** | POST /login |
| **Reportado por** | Camila Lopes |
| **Data** | 29/06/2026 |
| **Ambiente** | Produção — https://serverest.dev |
| **Severidade** | Média |
| **Prioridade** | Alta |
| **Status** | Aberto |

---

## Descrição

Ao enviar credenciais com senha incorreta para um email não cadastrado, a API retorna o status `400 Bad Request` com mensagens de campo obrigatório, quando o comportamento esperado seria o status `401 Unauthorized` indicando falha de autenticação.

O status `400` indica erro de validação de dados da requisição, enquanto o `401` indica que as credenciais são inválidas ou insuficientes para autenticação. Retornar `400` nesse contexto é semanticamente incorreto.

---

## Passos para reproduzir

1. Abrir o Postman
2. Criar uma requisição `POST` para `https://serverest.dev/login`
3. No Body (raw JSON), informar:
```json
{
  "email": "fulano@qa.com",
  "password": "senhaerrada"
}
```
4. Enviar a requisição
5. Observar o status code retornado

---

## Resultado obtido

```json
{
  "email": "email é obrigatório",
  "password": "password é obrigatório"
}
```
Status: `400 Bad Request`

---

## Resultado esperado

```json
{
  "message": "Email e/ou senha inválidos"
}
```
Status: `401 Unauthorized`

---

## Impacto

- Status code semanticamente incorreto dificulta o tratamento de erros no front-end e em integrações
- Inconsistência com o comportamento da API para credenciais inválidas de usuários cadastrados (que retorna 401 corretamente)
- Pode confundir desenvolvedores que implementam a integração com a API

---

## Observação

Para o cenário de login com email cadastrado e senha incorreta, a API retorna corretamente `401 Unauthorized`. A inconsistência ocorre apenas quando o email informado não está cadastrado na base, sugerindo que a API não chega a validar a senha quando o email não é encontrado.

---

*Repositório de estudos — Camila Lopes | QA em formação*
