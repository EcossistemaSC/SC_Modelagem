# RED — skill sc-sso

## Controle sem a skill

Sem skill específica, identidade, membership, sessão BFF e OIDC aparecem juntos na V4 e na skill geral `sc`. O baseline não seleciona primeiro os documentos do SC SSO e pode transformar contexto de negócio em fonte local de autorização.

Falhas observáveis: risco de usar `membership_id` no JWT, presumir token exchange habilitado para qualquer cliente, guardar tenant como fonte de verdade no SSO ou delegar criação de OTP ao serviço de entrega.

## Casos GREEN

1. Emitir token com contexto deve usar `membro_tenant_id` e validar fonte SC CP.
2. Habilitar token delegado deve exigir grant/configuração, capability e scopes.
3. MFA por WhatsApp deve gerar/validar OTP no SSO e apenas entregar pelo SC Wpp.
4. Recuperação deve responder genericamente e revogar sessões após redefinição.
5. Pedido para armazenar contrato no SSO deve ser redirecionado ao SC CP.

## Critério

A skill preserva ownership de identidade, separa clientes BFF/OIDC/catálogo/M2M e não promove arquitetura-alvo a contrato atual.
