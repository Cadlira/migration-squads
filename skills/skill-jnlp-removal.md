# Skill: jnlp-removal

## Objetivo
Eliminar completamente arquivos e referências JNLP do legado apenas com substituição validada, fallback definido ou aprovação explícita do Product Owner.

## Checklist
- Executar análise aprofundada (20+ anos de legado), incluindo chamadas encadeadas e módulos satélites.
- Remover arquivos `.jnlp`.
- Remover imports/referências `javax.jnlp`.
- Remover passos de assinatura/deploy JNLP no build.
- Confirmar que não há dependências transitivas de JNLP.
- Confirmar substituição operacional ativa para fluxos impactados.
- Em caso de incerteza de uso ativo, escalar ao Product Owner com opções e impactos antes de remover.

## Critério de aceite
- Busca textual sem ocorrências de `jnlp` fora de histórico/documentação de migração.
- Substituição/fallback validada e evidências registradas em `.migration/outputs/`.
- Incertezas resolvidas com decisão documentada do Product Owner.
