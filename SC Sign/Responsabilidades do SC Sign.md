# Responsabilidades do SC Sign

[Visão geral](../SC%20Ecossistema/Visão%20Geral%20do%20Ecossistema%20SC.md) · [Segurança compartilhada](../SC%20Ecossistema/Segurança%20Compartilhada.md)

## Propósito

O SC Sign produz documentos assináveis, registra manifestação de vontade e mantém evidências técnicas verificáveis. Seu produto é a integridade e a rastreabilidade do processo, não uma promessa jurídica genérica.

## É responsabilidade do SC Sign

- receber solicitação autorizada e dados mínimos do signatário;
- gerar ou receber documento canônico versionado;
- calcular e preservar hash SHA-256 dos bytes exatos;
- registrar consentimento, identidade autenticada, tempo e metadados permitidos;
- solicitar ancoragem temporal a TSA quando a modalidade exigir;
- manter trilha append-only e pacote de evidências;
- verificar integridade do documento, timestamp e trilha;
- publicar evento idempotente de consumo para o SC Billing;
- redigir dados sensíveis em logs e telemetria.

## Não é responsabilidade do SC Sign

- autenticar usuário ou decidir membership, contrato e permissão;
- declarar sozinho o enquadramento jurídico de uma modalidade;
- armazenar chaves privadas ICP-Brasil sem componente e política específicos;
- cobrar ou emitir fatura;
- alterar documento após o hash sem criar nova versão e novo processo.

## Invariantes

1. O hash refere-se aos bytes exatos preservados como documento canônico.
2. Evidência é append-only; correção gera novo evento, não sobrescrita silenciosa.
3. Signatário e tenant vêm de contexto autorizado.
4. Toda verificação informa o que foi tecnicamente validado e o que não foi.
5. Consumo financeiro não pode alterar a evidência da assinatura.

## Dependências

SC AG para entrada protegida, SC SSO para identidade, SC CP para autorização/contrato, TSA e armazenamento imutável para evidências e SC Billing para consumo.

## Terminologia pendente

A V4 chama o produto de “assinatura eletrônica qualificada” e simultaneamente descreve ICP-Brasil como elevação opcional. Essa combinação precisa de revisão jurídica. Até uma decisão formal, código e documentação devem descrever controles técnicos concretos sem prometer categoria ou presunção legal.
