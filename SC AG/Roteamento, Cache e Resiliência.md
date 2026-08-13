# Roteamento, Cache e Resiliência

[Responsabilidades](Responsabilidades%20do%20SC%20AG.md) · [Operação compartilhada](../SC%20Ecossistema/Observabilidade,%20Disponibilidade%20e%20Operação.md)

## Roteamento

Rotas apontam para serviços por identidade estável e health check, não por IP fixo. A configuração define métodos, paths, destino, audience, scopes, timeout, limite de corpo e política de retry.

## Cache de políticas

| Campo | Finalidade |
| --- | --- |
| Namespace | Ambiente, tenant, aplicação e tipo de política |
| Schema version | Impede interpretar estrutura desconhecida |
| Policy version | Ordena atualizações e invalidações |
| TTL | Limita vida de estado obsoleto |
| Loaded at | Permite medir idade do fallback local |

Redis é a fonte rápida; o SC CP continua fonte persistente. Cada réplica mantém último snapshot válido apenas para degradação temporária.

## Circuit breaker

- falhas e latência abrem o circuito por destino;
- half-open envia tráfego de prova limitado;
- fallback nunca simula sucesso de operação de negócio;
- `503` externo é preferível a concessão incorreta de acesso;
- métricas distinguem falha do destino, timeout, circuito aberto e fallback.

## Retry

GETs seguros podem repetir com jitter e orçamento curto. Mutações exigem idempotency key e contrato do destino. Não repetir indiscriminadamente login, assinatura, envio ou cobrança.

## Alta disponibilidade

Ao menos duas réplicas em zonas distintas, sem estado local indispensável. Balanceador remove instância não saudável. Deploy gradual verifica error rate, latência e incompatibilidade de schema de política.

## Cenários de caos

1. Perda de uma réplica sem impacto perceptível.
2. Redis indisponível com uso do snapshot dentro do grace period.
3. Snapshot expirado causando falha fechada e alerta.
4. SC SSO indisponível impedindo novos logins.
5. Serviço destino lento abrindo circuit breaker sem avalanche de retries.

## Decisões pendentes

Definir TTLs, grace periods, thresholds, orçamento de retry, formato de policy snapshot e mecanismo de distribuição antes da implementação produtiva.
