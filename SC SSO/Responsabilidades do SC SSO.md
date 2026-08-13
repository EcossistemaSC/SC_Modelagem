# Responsabilidades do SC SSO

[Visão geral](../SC%20Ecossistema/Visão%20Geral%20do%20Ecossistema%20SC.md) · [Segurança compartilhada](../SC%20Ecossistema/Segurança%20Compartilhada.md)

## Propósito

O SC SSO é a fonte de verdade de identidade e o Authorization Server do ecossistema. Ele autentica pessoas e clientes técnicos, aplica políticas de credencial e emite artefatos OAuth2/OIDC verificáveis.

## É responsabilidade do SC SSO

- cadastrar, verificar, ativar, bloquear e excluir logicamente identidades;
- armazenar senha somente como hash forte e administrar recuperação;
- orquestrar MFA e desafios de uso único;
- manter clientes OAuth2/OIDC, grants, redirects e chaves de assinatura;
- implementar OIDC `authorization_code` com PKCE e rotação de refresh token;
- emitir tokens de usuário, delegados e `client_credentials` conforme configuração;
- publicar discovery e JWKS;
- revogar sessões, famílias de refresh token e credenciais técnicas;
- emitir eventos de ciclo de vida para o SC CP;
- impedir enumeração e brute force em conjunto com o SC AG.

## Não é responsabilidade do SC SSO

- decidir contrato, liberação, catálogo, cargo ou permissão de negócio;
- armazenar tenant/membership como fonte de verdade;
- aplicar CORS ou rate limit de borda;
- manter cookie local de aplicações clientes;
- gerar cobrança, assinar documento ou entregar mensagem diretamente.

## Invariantes

1. Login de usuário exige identidade `ACTIVE` e e-mail verificado.
2. OTP é de uso único, expira e possui limite de tentativas.
3. Recuperação não revela se a identidade existe.
4. Token tem issuer, audience, expiração e `jti` verificáveis.
5. Contexto de negócio incluído no token vem de fonte autorizada do SC CP.
6. Segredos em claro aparecem somente no instante controlado de criação/rotação.

## Requisitos associados

REQ-001 a REQ-005, REQ-011, REQ-012 e parte de REQ-014.

## Decisões e lacunas

- A composição do contexto SC CP durante emissão/refresh precisa de contrato de disponibilidade e cache.
- Grants/scopes habilitados por tipo de cliente precisam de matriz formal.
- O host e a migração dos endpoints atuais de identidade dependem da separação física.
