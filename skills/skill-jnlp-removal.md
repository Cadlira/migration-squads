# Skill: jnlp-removal

## Objetivo
Eliminar arquivos e referências JNLP do legado com remoção completa quando não houver uso ativo fora desse contexto.

## Diretrizes obrigatórias
- É permitido remover JNLP sem substituição quando o sistema não fizer mais uso dessa tecnologia.
- Não manter artefatos JNLP sem uso ativo comprovado em código que permanecerá no sistema.
- Se houver uso cruzado de entidades/utilitários por partes ativas, revisar se remove, substitui, adapta ou escala ao PO quando houver dúvida.
- É proibido afetar o funcionamento de partes não relacionadas ao JNLP.

## Checklist
- Remover arquivos `.jnlp`.
- Remover imports/referências `javax.jnlp`.
- Remover passos de assinatura/deploy JNLP no build.
- Confirmar que não há dependências transitivas de JNLP e que usos cruzados permaneceram íntegros.

## Critério de aceite
Busca textual sem ocorrências de `jnlp` fora de histórico/documentação de migração.
