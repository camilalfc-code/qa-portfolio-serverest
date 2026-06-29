# BUG-001 — Mensagem de erro incorreta para email inválido no login

## Informações gerais

| Campo | Valor |
|---|---|
| **ID** | BUG-001 |
| **Título** | API retorna "email é obrigatório" para email inválido em vez de "email deve ser um email válido" |
| **Módulo** | Login |
| **Endpoint** | POST /login |
| **Reportado por** | Camila Lopes |
| **Data** | 29/06/2026 |
| **Ambiente** | Produção — https://serverest.dev |
| **Severidade** | Baixa |
| **Prioridade** | Média |
| **Status** | Aberto |

---

## Descrição

Ao enviar o campo `email` com valor em letras maiúsculas (uppercase) ou com quantidade excessiva de caracteres, a API retorna a mensagem `"email é obrigatório"`, que é a mesma mensagem exibida quando o campo está vazio. A mensagem esperada nesses casos seria `"email deve ser um email válido"`, pois o campo está preenchido, porém com um formato inválido.

Esse comportamento pode confundir o consumidor da API, pois a mensagem não reflete corretamente o motivo da rejeição.

---

## Cenários afetados

### Cenário 1 — Email em uppercase

**Request:**
```json
{
  "email": "FULANO@QA.COM",
  "password": "senha123"
}
```

**Resultado obtido:**
```json
{
  "email": "email é obrigatório",
  "password": "password é obrigatório"
}
```
Status: `400 Bad Request`

**Resultado esperado:**
```json
{
  "email": "email deve ser um email válido"
}
```

---

### Cenário 2 — Email com muitos caracteres

**Request:**
```json
{
  "email": "aaaaaaaaa[...caracteres excessivos...]@qa.com",
  "password": "senha123"
}
```

**Resultado obtido:**
```json
{
  "email": "email é obrigatório",
  "password": "password é obrigatório"
}
```
Status: `400 Bad Request`

**Resultado esperado:**
```json
{
  "email": "email deve ser um email válido"
}
```

---

## Passos para reproduzir

1. Abrir o Postman
2. Criar uma requisição `POST` para `https://serverest.dev/login`
3. No Body (raw JSON), informar:
```json
{
  "email": "FULANO@QA.COM",
  "password": "senha123"
}
```
4. Enviar a requisição
5. Observar a mensagem retornada no campo `email`

---

## Resultado obtido

❌ API retorna `"email é obrigatório"` mesmo com o campo preenchido.

---

## Resultado esperado

✅ API deveria retornar `"email deve ser um email válido"`, indicando que o campo está preenchido mas com formato inválido.

---

## Impacto

- Mensagem de erro confusa para o consumidor da API
- Dificulta o diagnóstico do problema real (formato inválido vs campo vazio)
- Pode gerar comportamento inesperado em integrações que tratam os erros por mensagem

---

*Repositório de estudos — Camila Lopes | QA em formação*
