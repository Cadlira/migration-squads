# Skill: rmi-removal

## Objetivo
Remover completamente integrações RMI legadas sem impactar funcionalidades mantidas.
Aplicar substituição somente quando houver dependências vivas fora do contexto removido.

## Etapas
1. Mapear contratos e pontos de invocação RMI.
2. Executar análise aprofundada de chamadas encadeadas, módulos satélites e uso ativo.
3. Classificar cada dependência: exclusiva de RMI (remover) ou compartilhada com fluxo mantido (preservar/adaptar).
4. Se houver dependência viva fora do contexto RMI, definir padrão alvo (REST/gRPC/evento) e camada adaptadora quando necessário.
5. Se houver substituição, validar funcional/técnica e plano de fallback/rollback.
6. Remover classes/interfaces `java.rmi` legadas do fluxo descontinuado.
7. Em caso de incerteza sobre uso ativo cruzado, escalar ao Product Owner com opções e impactos antes de remover.

## Critérios de aceite
- Sem importações `java.rmi` no domínio alvo.
- Contratos de integração documentados em output local.
- Quando aplicável, substituição/fallback validada e evidências registradas em `.migration/outputs/`.
- Incertezas resolvidas com decisão documentada do Product Owner.
