# Contratos e Fluxos do SC CP

[Responsabilidades](Responsabilidades%20do%20SC%20CP.md) · [Modelo](Modelo%20de%20Domínio%20do%20SC%20CP.md)

## Contratos atuais confirmados

### RBAC integrado

| Método e path | Scope |
| --- | --- |
| `GET /api/v1/integracoes/rbac/aplicacoes/{idAplicacao}/membros` | `sc.members.read` |
| `GET /api/v1/integracoes/rbac/aplicacoes/{idAplicacao}/permissoes` | `sc.rbac.read` |
| `GET /api/v1/integracoes/rbac/aplicacoes/{idAplicacao}/cargos` | `sc.rbac.read` |
| `GET /api/v1/integracoes/rbac/aplicacoes/{idAplicacao}/atribuicoes` | `sc.rbac.read` |
| `POST /api/v1/integracoes/rbac/aplicacoes/{idAplicacao}/cargos` | `sc.rbac.write` |
| `PUT /api/v1/integracoes/rbac/aplicacoes/{idAplicacao}/cargos/{idCargo}` | `sc.rbac.write` |
| `POST /api/v1/integracoes/rbac/aplicacoes/{idAplicacao}/atribuicoes` | `sc.rbac.write` |

Token delegado exige audience `sc-management`, `application_id` igual ao path, scope adequado e capability `RBAC_MANAGE` na origem.

### Catálogo e manifesto

| Método e path | Função |
| --- | --- |
| `POST /api/v1/integracoes/catalogo/aplicacoes/{idAplicacao}/manifestos` | Cria prévia e retorna diferenças, ID e hash |
| `POST /api/v1/integracoes/catalogo/aplicacoes/{idAplicacao}/manifestos/{idManifesto}/aplicar` | Aplica a prévia com o mesmo hash |
| `GET /api/v1/integracoes/catalogo/aplicacoes/{idAplicacao}/manifesto` | Exporta estado ativo |
| `POST /api/v1/aplicacoes/{idAplicacao}/clientes-oidc` | Cadastra cliente, redirects e origens |

Catálogo usa `client_credentials`, audience `sc-catalog`, scope `sc.catalog.sync` e `application_id` vinculado.

### Provisionamento

`POST /api/v1/tenants/contexto-atual/provisionamentos` recebe e-mail, cargo inicial e aplicação de origem. O SC CP valida capability, tenant, aplicação e cargo, cria ativação de uso único e coordena envio. A conclusão cria/ativa membership e atribuição.

## Publicação de políticas

Alterações em contrato, aplicação, origem, rate limit ou bloqueio incrementam versão e publicam invalidação. O SC AG só aceita política com schema reconhecido e conserva último snapshot válido durante falha temporária.

## Erros

- `401`: token/credencial inválido;
- `403`: ator sem acesso efetivo ou fora da aplicação;
- `409`: regra de exclusividade, concorrência ou hash divergente;
- `422`: manifesto estruturalmente inválido;
- `429`: limite da integração;
- `503`: dependência indispensável indisponível.

## Limitações atuais

- Não existe solicitação pública de acesso; o fluxo nasce no administrador.
- Não existe contrato confirmado de `PATCH` para status de cargos/atribuições.
- Token exchange depende de configuração do cliente; não presumir grants `sc.*`.
- Paths pertencem ao contrato atual da Software Center; o host futuro do SC CP depende da separação arquitetural.
