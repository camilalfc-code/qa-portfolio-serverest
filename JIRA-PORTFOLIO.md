# Gestão de Testes com Jira - QA ServeRest API Testing

## Sobre este projeto
Simulação de um ciclo completo de QA utilizando Jira (Scrum), baseado nos testes 
da API ServeRest já documentados neste repositório.

## Estrutura do projeto no Jira

### Sprint 1 - ServeRest API
| ID | Story | Status |
|---|---|---|
| CAMILA-1 | [LOGIN] Autenticação de usuário via API | ✅ Feito |
| CAMILA-2 | [USUÁRIOS] Gerenciamento de usuários via API | 🔄 Fazendo |
| CAMILA-3 | [PRODUTOS] Gerenciamento de produtos via API | 📋 A Fazer |

### Casos de Teste (Subtarefas)
| ID | Caso de Teste | Story | Status |
|---|---|---|---|
| CAMILA-4 | CT-001 - Login com credenciais válidas | CAMILA-1 | ✅ Feito |
| CAMILA-5 | CT-002 - Login com email inválido | CAMILA-1 | ✅ Feito |
| CAMILA-6 | CT-003 - Login com senha incorreta | CAMILA-1 | ✅ Feito |
| CAMILA-7 | CT-004 - Login com campos vazios | CAMILA-1 | ✅ Feito |
| CAMILA-8 | CT-005 - Criar usuário com dados válidos | CAMILA-2 | 📋 A Fazer |
| CAMILA-9 | CT-006 - Criar usuário com email duplicado | CAMILA-2 | 📋 A Fazer |
| CAMILA-10 | CT-007 - Listar todos os usuários | CAMILA-2 | 📋 A Fazer |
| CAMILA-11 | CT-008 - Excluir usuário existente | CAMILA-2 | 📋 A Fazer |
| CAMILA-12 | CT-009 - Criar produto com dados válidos | CAMILA-3 | 📋 A Fazer |
| CAMILA-13 | CT-010 - Criar produto com nome duplicado | CAMILA-3 | 📋 A Fazer |
| CAMILA-14 | CT-011 - Listar todos os produtos | CAMILA-3 | 📋 A Fazer |
| CAMILA-15 | CT-012 - Excluir produto existente | CAMILA-3 | 📋 A Fazer |

### Bug Report
| ID | Título | Prioridade | Status | Vinculado a |
|---|---|---|---|---|
| CAMILA-16 | BUG-001 - Login com email inválido retorna mensagem de erro incorreta | 🔴 Alta | A Fazer | CAMILA-1 |

## Ferramentas utilizadas
- Jira Software (Scrum)
- Metodologia ágil com Sprints de 2 semanas
- Bug tracking com linkagem entre issues
- Subtarefas para rastreabilidade de casos de teste

## Evidências
📁 Prints do board disponíveis na pasta `/evidencias-jira`
