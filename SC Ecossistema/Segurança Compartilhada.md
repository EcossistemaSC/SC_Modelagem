# Segurança Compartilhada

[Voltar à visão geral](Visão%20Geral%20do%20Ecossistema%20SC.md)

## Princípios obrigatórios

1. O navegador não recebe access token, refresh token, segredo técnico, credencial BFF ou `sessionId` remoto.
2. Cada serviço valida `iss`, `aud`, `exp`, `jti`, scopes e contexto exigido; presença de JWT não basta.
3. Autorização de negócio considera identidade, membership, tenant, aplicação, contrato/liberação, atribuição, cargo e permissões ativos.
4. Todo dado e canal são isolados por tenant. Identificador de tenant vindo do browser nunca é confiável por si só.
5. Segredos ficam em cofre e nunca em código, logs, traces, métricas, filas ou mensagens de erro.
6. Soft-delete e trilhas imutáveis preservam auditoria sem manter acesso ativo.

## Navegador, BFF e cookies

Cada aplicação cliente mantém sua própria sessão. A sessão do painel SC não compartilha cookie com aplicações integradas.

| Controle | Regra |
| --- | --- |
| Cookie | `HttpOnly`, `Secure` em HTTPS, `SameSite=Lax` ou mais restritivo |
| Escopo | Domínio mínimo necessário; nunca wildcard amplo entre aplicações |
| CSRF | Double-submit token ou proteção equivalente validada no backend/gateway para mutações |
| Tokens | Nunca persistir em `localStorage` ou `sessionStorage` |
| Logout | Revogar sessão remota quando aplicável e invalidar sessão/cookie local |
| Erros | Respostas genéricas para impedir enumeração de usuário e regras de acesso |

## Service-to-service

- mTLS autentica a carga de trabalho.
- OAuth2 `client_credentials` autoriza a ação por audience e scope mínimo.
- Certificados e segredos têm rotação automatizada e revogação verificável.
- O serviço destino rejeita token sem audience própria, mesmo que a assinatura seja válida.
- Credenciais de BFF, OIDC e catálogo são separadas e nunca reutilizadas.

## Isolamento multi-tenant

Toda consulta de domínio deve receber o tenant do contexto confiável, não de filtro opcional. Caches, chaves Redis, tópicos, métricas e canais WebSocket incluem tenant e aplicação quando necessário.

Uma aplicação exclusiva usa o tenant beneficiário cadastrado no SC CP. Uma aplicação não exclusiva exige resolução de `tenantSubdominio` contra tenant ativo e contrato válido.

## Matriz de ameaças

| Ameaça | Controle principal | Proprietário |
| --- | --- | --- |
| XSS exfiltra token | Tokens somente no BFF; CSP e cookie HttpOnly | Aplicação cliente e SC AG |
| CSRF | SameSite + token CSRF validado | BFF e SC AG |
| Replay de token | TTL curto, `jti`, audience e revogação | SC SSO e serviços destino |
| Segredo S2S vazado | mTLS, cofre, rotação e redação | Todos os serviços/SRE |
| Acesso cross-tenant | Contexto confiável e testes negativos | Todos os serviços |
| Enumeração de identidade | Respostas uniformes e `202` em recuperação | SC SSO |
| Brute force | Rate limit por aplicação, identidade normalizada e IP | SC AG e SC SSO |
| WebSocket cross-tenant | Autorização no handshake e tópico isolado | SC AG/observabilidade |
| Webhook forjado | Assinatura, timestamp, idempotência e replay protection | SC Wpp e SC Billing |
| Evidência de assinatura alterada | Hash canônico, TSA e auditoria append-only | SC Sign |

## Dados sensíveis e logs

Nunca registrar senha, OTP, token de ativação, `sessionId`, access/refresh token, segredo de cliente, chave privada ou conteúdo integral de documento assinado. Payloads observáveis devem ser redigidos; IDs técnicos não secretos podem ser usados apenas quando necessários à correlação.

## Respostas de erro

| Status | Semântica externa |
| --- | --- |
| `401` | Credencial ou sessão inválida; encerrar sessão local quando aplicável. |
| `403` | Ator autenticado sem acesso efetivo; não revelar a regra exata. |
| `429` | Limite excedido; respeitar `Retry-After`. |
| `503` | Dependência indispensável indisponível; oferecer alternativa apenas quando segura. |

## Checklist para mudanças

- Identifique a fonte de verdade e o tenant antes de consultar dados.
- Modele negações: tenant, contrato, membership, cargo e credencial inativos.
- Verifique se novos logs podem conter segredo ou PII.
- Defina rotação e revogação de qualquer nova credencial.
- Teste replay, idempotência e isolamento de cache/fila.
- Atualize [Arquitetura e Integrações](Arquitetura%20e%20Integrações%20Entre%20Serviços.md) se o limite entre serviços mudar.

## Decisões e lacunas

- O mecanismo exato de revogação imediata de JWT precisa de contrato formal que não dependa apenas de expiração.
- O procedimento da CA interna, CRL/OCSP e secret manager ainda precisa de runbook.
- A política de retenção de PII, evidências e logs precisa de validação jurídica e de privacidade.
