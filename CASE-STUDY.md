# Case Study: Debugging de Falhas Intermitentes na Fase 2 (Produtos) — API ServeRest

## Contexto

Durante a expansão da minha suíte de testes automatizados no Postman para a API pública [ServeRest](https://serverest.dev), 3 de 60 testes da coleção começaram a falhar de forma consistente no Collection Runner, mesmo com os requests individuais parecendo corretos à primeira vista:

| Teste | Erro reportado |
|---|---|
| `GET Buscar Produto por ID` | `expected 200 but got 400` + `JSONError: Unexpected token '<'` |
| `POST Cadastrar Produto — nome duplicado` | `expected 400 but got 201` |

O objetivo deste documento é registrar o processo de investigação — não só a correção final, mas o raciocínio usado para descartar hipóteses erradas ao longo do caminho, já que isso é normalmente mais revelador do processo de QA do que o resultado isolado.

---

## Problema 1 — `POST Cadastrar Produto — nome duplicado` retornando 201 em vez de 400

**Sintoma:** o teste esperava que a API rejeitasse um cadastro com nome de produto já existente (status 400), mas recebia 201 (criado com sucesso).

**Investigação:**
- O body do request usava a variável `{{nomeProdutoEdit}}` — que pertencia a outro teste (`PUT Editar Produto`), não ao produto original criado no cadastro. Ou seja, o teste nunca estava de fato tentando duplicar um nome existente.
- Além disso, a ordem de execução da pasta *Produtos* colocava esse teste **depois** do `DELETE Deletar Produto`. Mesmo corrigindo a variável, o produto original já teria sido apagado antes do teste de duplicidade rodar — eliminando a duplicata que o teste dependia para existir.

**Causa raiz:** combinação de (1) variável errada referenciada no body e (2) dependência de ordem de execução não respeitada na estrutura da pasta.

**Correção:**
1. Reordenei a pasta *Produtos* para que o teste de nome duplicado rodasse logo após o cadastro original, antes do `DELETE`.
2. Corrigi o body para reutilizar `{{nomeProduto}}` (a mesma variável do cadastro original).

---

## Problema 2 — `GET Buscar Produto por ID` retornando 400 com corpo em HTML

Este foi o mais longo dos dois, porque o sintoma (`Unexpected token '<'`) sugeria múltiplas causas possíveis, e cada uma precisou ser testada e descartada individualmente.

### Hipóteses testadas (e descartadas)

| # | Hipótese | Como foi testada | Resultado |
|---|---|---|---|
| 1 | Nome da variável errado na URL (`{{produtold}}` vs `{{produtoId}}`) | Inspeção visual da URL, confirmação de que já estava correta | Descartada |
| 2 | `pm.collectionVariables.set()` não executando por estar dentro de um `pm.test()` que falha antes | Revisão do Test Script do `POST Cadastrar Produto` | Script já estava correto — descartada |
| 3 | Variável `produtoId` com valor incorreto/vazio | Inspeção do painel "Variables in request" do Postman, confirmando o valor salvo | Valor estava correto — descartada |
| 4 | Espaço/caractere invisível na URL | Digitação manual da URL do zero, sem colar | Descartada |
| 5 | Query params ocultos | Inspeção da aba Params | Vazia — descartada |
| 6 | Headers extras causando rejeição (`Content-Type` sem body em GET) | Comparação de headers com um request que funcionava (`GET Listar Produtos`) | Reduziu de 9 para 6 headers, mas erro persistiu — descartada |
| 7 | Autenticação malformada | Inspeção da aba Authorization | `No Auth` configurado — descartada |

### Causa raiz encontrada

Ao inspecionar a aba **Body** do request (não a aba Scripts), encontrei um **Test Script inteiro colado por engano no corpo da requisição**, com o modo `raw`/`JSON` selecionado. Um `GET` não deveria ter body — e a API do ServeRest roda sobre infraestrutura Google App Engine, cujo proxy (GFE) rejeita requisições GET malformadas com um body inválido **antes mesmo de repassar a chamada para a aplicação**. Isso explica por que a resposta de erro vinha em HTML (página de erro genérica do proxy) em vez de JSON (resposta da aplicação ServeRest).

**Correção:** Body alterado para `none`.

### Causa raiz secundária: dependência de autenticação

Depois de resolver o body, o teste ainda falhava — mas agora com uma resposta JSON legítima da API (`"message": "Produto não encontrado"`). Isso indicava que o problema real havia mudado de "requisição malformada" para "o produto realmente não existe".

Ao rodar a collection completa (não apenas a pasta *Produtos* isoladamente), ficou claro que:
- O `POST Cadastrar Produto` depende de um token Bearer (`{{token}}`), gerado pelo login.
- O usuário fixo usado para login (`qa.fixo@teste.com`) não existia mais no banco do ServeRest — a API é pública e reseta seus dados periodicamente.
- Sem login bem-sucedido, o cadastro do produto falhava com 401, `produtoId` nunca era salvo, e todos os testes seguintes falhavam em cascata (inclusive com URLs literalmente `/produtos/null`).

**Correção final:** adicionado um **Pre-request Script** no `POST Login — credenciais válidas`, que tenta recriar o usuário fixo via `pm.sendRequest()` antes de cada execução de login, ignorando o erro caso ele já exista:

```javascript
pm.sendRequest({
    url: 'https://serverest.dev/usuarios',
    method: 'POST',
    header: { 'Content-Type': 'application/json' },
    body: {
        mode: 'raw',
        raw: JSON.stringify({
            nome: "QA Fixo",
            email: "qa.fixo@teste.com",
            password: "teste123",
            administrador: "true"
        })
    }
}, function (err, res) {
    // Idempotente: se o usuário já existir, a API retorna 400 e seguimos normalmente.
});
```

---

## Resultado final

| Métrica | Antes | Depois |
|---|---|---|
| Testes passando | 57/60 | **60/60** |
| Fonte do erro visível | `Unexpected token '<'` (proxy) | — |
| Dependência de estado externo | Usuário fixo manual | Usuário auto-recriado a cada run |

---

## Principais aprendizados

- **Nem todo erro 400 vem da aplicação.** Comparar headers de resposta (`X-Google-GFE-*`) ajudou a identificar que a rejeição estava acontecendo na camada de infraestrutura, não no ServeRest — uma distinção que mudou completamente o rumo da investigação.
- **Testar hipóteses de forma isolada evita retrabalho.** Em vez de mudar várias coisas ao mesmo tempo, cada suspeita (variável, header, auth, body) foi validada individualmente antes de seguir para a próxima — o que tornou possível descartar 7 hipóteses com confiança até chegar na causa real.
- **APIs públicas de teste têm estado efêmero.** Depender de um usuário fixo cadastrado manualmente é frágil quando a base de dados pode ser resetada a qualquer momento. Tornar a suíte de testes auto-suficiente (criando suas próprias dependências de dados) é uma prática mais robusta para automação de testes.
- **Rodar a collection completa, não só a pasta isolada, revela dependências ocultas** entre pastas (Login → Usuários → Produtos) que não aparecem ao testar cada pasta separadamente.
