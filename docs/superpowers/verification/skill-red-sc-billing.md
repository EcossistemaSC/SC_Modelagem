# RED — skill sc-billing

## Controle sem a skill

O faturamento aparece na V4 junto de contratos, pagamento e motores. Sem skill específica, o baseline podia recalcular consumo antigo com preço novo, duplicar cobrança em webhook ou permitir ao Billing bloquear acesso sem coordenação do SC CP/SC AG.

## Casos GREEN

1. Reajuste de preço: criar nova vigência, não alterar consumo aceito.
2. Webhook `paid` duplicado: verificar, deduplicar e reconciliar.
3. Fechamento concorrente: impedir duas faturas da mesma competência.
4. Inadimplência: Billing publica estado; CP suspende contrato; AG aplica bloqueio.
5. Criar endpoint de consumo: manter conceitual até aprovação de path/schema.

## Critério

A skill preserva preço/moeda congelados, idempotência, fatura imutável e ownership entre finanças e autorização.
