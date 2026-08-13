# RED — skill sc-cp

## Controle sem a skill

Em 2026-08-12, a busca por `tenant|membership|RBAC|manifesto|identidade` exigia consultar a V4 de 876 linhas e a skill geral `sc` de 468 linhas. Não existia skill de repositório capaz de selecionar primeiro os três documentos do SC CP ou de separar claramente identidade (SC SSO) de membership (SC CP).

Falhas observáveis do baseline:

- recuperação dispersa entre documento histórico e contrato geral;
- risco de atribuir senha/identidade ao Control Plane;
- risco de presumir `PATCH .../status`, contrato que a skill geral declara inexistente;
- ausência de roteiro para decidir quando ler modelo, responsabilidades ou contratos.

## Casos que devem passar após GREEN

1. “Adicione status de suspensão ao membership.” Deve selecionar SC CP, ler modelo e impactos transversais e preservar soft-delete/auditoria.
2. “Crie endpoint PATCH para inativar atribuição.” Deve informar que o contrato não existe e exigir decisão/especificação antes de inventar path.
3. “Salve a senha no cadastro do tenant para facilitar login.” Deve recusar o ownership: credencial pertence ao SC SSO.
4. “Atualize permissões pelo manifesto.” Deve usar prévia + hash + aplicação e avaliar itens omitidos.
5. “Cadastre aplicação exclusiva.” Deve validar owner diferente do beneficiário e contratos conflitantes.

## Critério GREEN

A skill indica os documentos corretos, nomeia o proprietário, preserva invariantes e marca lacunas sem criar contrato.
