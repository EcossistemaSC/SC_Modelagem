# RED — skill sc-wpp

## Controle sem a skill

A modelagem histórica alternava o nome SC Notify/SC Wpp e concentrava poucos detalhes em uma seção. O baseline podia recorrer a integração não oficial, duplicar mensagens em retry, confiar em webhook sem assinatura ou gerar OTP no serviço errado.

## Casos GREEN

1. Pedir Baileys/whatsapp-web.js: rejeitar e usar Cloud API oficial.
2. Webhook repetido: deduplicar por evento e não duplicar fallback/consumo.
3. MFA: SC SSO gera/valida OTP; SC Wpp entrega template.
4. Falha de template: não repetir como erro transitório.
5. Fallback para SMS: exigir consentimento e política do tenant/finalidade.

## Critério

A skill preserva canal oficial, consentimento, idempotência, classificação de falha e ownership do OTP.
