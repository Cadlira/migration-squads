# Skill: jnlp-removal

## Objetivo
Remover completamente arquivos e referências JNLP do legado sem quebrar fluxos ativos.

## Regras de execução segura
- É permitido remover JNLP sem substituição quando não houver dependências vivas fora do contexto removido.
- Artefatos compartilhados com uso ativo em outros fluxos devem ser preservados ou adaptados.
- Se não for possível confirmar uso ativo com segurança, pausar e escalar ao Product Owner para decisão explícita.

## Checklist
- Executar análise aprofundada (20+ anos de legado), incluindo chamadas encadeadas e módulos satélites.
- Remover arquivos `.jnlp`.
- Remover imports/referências `javax.jnlp`.
- Remover passos de assinatura/deploy JNLP no build.
- Confirmar que não há dependências transitivas de JNLP.
- Confirmar substituição operacional apenas para fluxos que possuem dependência viva fora do contexto JNLP.
- Em caso de incerteza de uso ativo cruzado, escalar ao Product Owner com opções e impactos antes de remover.

## Critério de aceite
- Busca textual sem ocorrências de `jnlp` fora de histórico/documentação de migração.
- Quando aplicável, substituição/fallback validada e evidências registradas em `.migration/outputs/`.
- Incertezas resolvidas com decisão documentada do Product Owner.
