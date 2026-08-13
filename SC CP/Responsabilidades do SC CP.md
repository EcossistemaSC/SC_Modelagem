# Responsabilidades do SC CP

[Visão geral](../SC%20Ecossistema/Visão%20Geral%20do%20Ecossistema%20SC.md) · [Arquitetura compartilhada](../SC%20Ecossistema/Arquitetura%20e%20Integrações%20Entre%20Serviços.md)

## Propósito

O SC CP é o Control Plane do ecossistema. Ele decide quais tenants, membros, aplicações e recursos existem, quais contratos liberam acesso e quais permissões podem ser atribuídas.

## É responsabilidade do SC CP

- administrar tenants, memberships, papéis administrativos e capabilities;
- manter aplicações, recursos, contratos, liberações e exclusividade;
- manter catálogo, permissões, rotas, cargos e atribuições;
- receber manifesto em prévia, calcular diferenças, aplicar por hash e exportar o estado ativo;
- provisionar acesso por administrador e coordenar ativação com o SC SSO;
- emitir e rotacionar credenciais BFF e de catálogo sem persistir segredo em claro;
- cadastrar clientes OIDC e origens CORS autorizadas;
- publicar políticas versionadas de CORS, rate limit, bloqueio e revogação para o SC AG;
- expor contexto de acesso somente após validar toda a cadeia de negócio;
- manter vínculo lógico e histórico com identidades do SC SSO.

## Não é responsabilidade do SC CP

- armazenar senha, OTP, fator MFA, refresh token ou chave privada de identidade;
- autenticar usuário diretamente como fonte de identidade;
- manter cookie da aplicação cliente;
- fazer proxy, aplicar rate limit ou decidir CORS na borda;
- assinar documentos, entregar WhatsApp ou calcular fatura;
- aceitar `applicationId` ou tenant do navegador como fonte confiável.

## Invariantes

1. Acesso só existe quando identidade, membership, tenant, aplicação, recurso, contrato/liberação, atribuição, cargo e permissões estão ativos e coerentes.
2. Aplicação exclusiva possui tenant beneficiário diferente do owner e não admite liberação ativa incompatível.
3. `OWNER` e `ADMIN` recebem capabilities administrativas; `MEMBER` não recebe por padrão.
4. Permissão é chave estável. Renomear cria uma permissão nova e inativa a anterior.
5. Exclusões de vínculos auditáveis usam soft-delete.

## Dependências

| Dependência | Uso |
| --- | --- |
| SC SSO | Identidade, tokens e eventos de ciclo de vida |
| SC AG | Aplicação das políticas de borda |
| Redis | Distribuição versionada de políticas e invalidações |
| SC Billing | Medição/faturamento de contratos e motores |
| Aplicações clientes | Manifestos, sessão BFF e gestão integrada de RBAC |

## Requisitos associados

REQ-006, REQ-007, REQ-008, REQ-010 e partes de REQ-011/REQ-015. Consulte [Requisitos e Rastreabilidade](../SC%20Ecossistema/Requisitos%20e%20Rastreabilidade.md).

## Decisões e lacunas

- A separação física dos endpoints hoje expostos pela SC precisa de ADR.
- Não há contrato publicado para eventos de sincronização de identidade.
- A API integrada atual não oferece inativação/reativação de cargo ou atribuição; não presumir `PATCH .../status`.
