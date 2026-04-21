# Skill: jnlp-removal

## Objetivo
Eliminar arquivos e referências JNLP do legado.

## Checklist
- Remover arquivos `.jnlp`.
- Remover imports/referências `javax.jnlp`.
- Remover passos de assinatura/deploy JNLP no build.
- Confirmar que não há dependências transitivas de JNLP.

## Critério de aceite
Busca textual sem ocorrências de `jnlp` fora de histórico/documentação de migração.
