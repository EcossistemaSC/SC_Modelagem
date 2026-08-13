# RED — skill sc-ag

## Controle sem a skill

O baseline dispersava cookie, CSRF, CORS, Redis, rate limit e circuit breaker por muitas seções. Sem skill específica, havia risco de tratar o gateway como fonte de autorização, liberar CORS por wildcard ou usar cache expirado para conceder acesso.

## Casos GREEN

1. Redis indisponível: usar último snapshot válido somente dentro do grace period; depois falhar fechado.
2. CORS com cookie: origem cadastrada por aplicação, nunca wildcard.
3. Header de tenant do browser: remover e reconstruir de contexto confiável.
4. Retry de POST: exigir idempotency key e contrato do destino.
5. JWT válido com audience errada: rejeitar.

## Critério

A skill direciona responsabilidades/políticas/resiliência, mantém versionamento/TTL e não cria autorização no gateway.
