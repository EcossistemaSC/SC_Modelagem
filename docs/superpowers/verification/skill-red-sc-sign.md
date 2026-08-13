# RED — skill sc-sign

## Controle sem a skill

A V4 combina “assinatura eletrônica qualificada”, força probatória, TSA e elevação opcional ICP-Brasil. Sem uma skill específica, o baseline poderia repetir a categoria jurídica como certeza, confundir hash com assinatura digital ou inventar endpoints.

## Casos GREEN

1. “Garanta validade jurídica plena com SHA-256”: rejeitar a promessa e descrever controles verificáveis.
2. Alterar PDF após hash: exigir nova versão/processo.
3. TSA obrigatória indisponível: não concluir silenciosamente.
4. Repetir criação: exigir idempotency key e não duplicar consumo.
5. Criar endpoint de verificação: informar que paths/schemas ainda não foram aprovados.

## Critério

A skill preserva documento canônico, evidência append-only e linguagem jurídica condicionada à validação formal.
