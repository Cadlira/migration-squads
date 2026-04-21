# Skill: rmi-removal

## Objetivo
Substituir integrações RMI por um padrão moderno e remover completamente as dependências legadas após validação da substituição.

## Etapas
1. Mapear contratos e pontos de invocação RMI.
2. Executar análise aprofundada de chamadas encadeadas, módulos satélites e uso ativo.
3. Definir padrão alvo (REST/gRPC/evento).
4. Criar camada adaptadora para coexistência temporária quando necessário.
5. Validar substituição funcional/técnica e plano de fallback/rollback.
6. Remover classes/interfaces `java.rmi` legadas.
7. Em caso de incerteza sobre uso ativo, escalar ao Product Owner com opções e impactos antes de remover.

## Critérios de aceite
- Sem importações `java.rmi` no domínio alvo.
- Contratos de integração documentados em output local.
- Substituição/fallback validada e evidências registradas em `.migration/outputs/`.
- Incertezas resolvidas com decisão documentada do Product Owner.
