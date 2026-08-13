# Responsabilidades do SC Wpp

[Visão geral](../SC%20Ecossistema/Visão%20Geral%20do%20Ecossistema%20SC.md) · [Segurança compartilhada](../SC%20Ecossistema/Segurança%20Compartilhada.md)

## Propósito

O SC Wpp orquestra notificações transacionais pela WhatsApp Business Platform — Cloud API oficial — e aplica canais alternativos previstos pela política.

## É responsabilidade do SC Wpp

- administrar configuração de canal e templates aprovados;
- validar consentimento e finalidade antes do envio;
- enfileirar mensagens com idempotência e prioridade;
- aplicar retry exponencial, limite de tentativas e dead-letter;
- consumir webhooks assinados e atualizar estados;
- acionar fallback autorizado para e-mail, SMS ou push;
- manter correlação sem registrar conteúdo sensível desnecessário;
- publicar consumo idempotente ao SC Billing.

## Não é responsabilidade do SC Wpp

- usar automações não oficiais como `whatsapp-web.js` ou Baileys;
- criar ou validar OTP de autenticação — isso pertence ao SC SSO;
- decidir contrato, permissão ou tenant;
- garantir entrega que o provedor não confirmou;
- cobrar ou emitir fatura.

## Invariantes

1. Envio exige tenant, template/finalidade e destinatário autorizados.
2. Um evento do provedor não pode regredir estado terminal sem regra explícita.
3. Webhook duplicado não duplica entrega, fallback ou consumo.
4. Fallback respeita consentimento e classificação da mensagem.
5. Segredo do provedor e conteúdo sensível não aparecem em logs.

## Dependências

SC AG, SC SSO, SC CP, filas, Cloud API, provedores de fallback e SC Billing.

## Lacunas

- Provedores de e-mail/SMS/push e regras de roteamento não estão escolhidos.
- Catálogo de templates, idiomas e política de opt-in/opt-out precisam de contrato.
- Limites e custos devem ser consultados no provedor durante implantação; não usar valores históricos da V4 como preço atual.
