# Responsabilidades do SC Billing

[Visão geral](../SC%20Ecossistema/Visão%20Geral%20do%20Ecossistema%20SC.md) · [Roadmap e riscos](../SC%20Ecossistema/Roadmap%20e%20Riscos.md)

## Propósito

O SC Billing mede uso faturável, aplica preços versionados e consolida mensalidades e consumo em uma fatura por tenant/contrato master.

## É responsabilidade do SC Billing

- manter tabela de preços com vigência;
- registrar consumo idempotente com preço unitário congelado;
- calcular pró-rata de itens fixos;
- fechar competência de forma reproduzível;
- gerar fatura unificada e memória de cálculo;
- orquestrar PSP e emissão fiscal sem armazenar cartão;
- reconciliar pagamentos, estornos e NFS-e;
- informar inadimplência ao SC CP/SC AG;
- auditar reajustes e notificações.

## Não é responsabilidade do SC Billing

- decidir se usuário possui permissão;
- autenticar cliente ou aceitar tenant não validado;
- recalcular consumo histórico com preço novo;
- armazenar dados brutos de cartão;
- produzir assinatura ou entregar mensagem;
- bloquear diretamente banco de outro serviço.

## Invariantes

1. Registro de consumo congela preço e moeda vigentes no instante aceito.
2. Mesma idempotency key não gera cobrança duplicada.
3. Fatura fechada não é reescrita; ajustes usam lançamento compensatório.
4. Webhook externo nunca é autoridade sem assinatura e reconciliação.
5. Bloqueio por inadimplência é evento auditável e reversível.

## Dependências

SC CP para contratos, SC Sign/SC Wpp para consumo, PSP para cobrança, provedor fiscal para NFS-e e SC AG para aplicação de bloqueio.

## Lacunas

Moedas, tributos, arredondamento, timezone da competência, grace period e política de estorno precisam de definição financeira/jurídica.
