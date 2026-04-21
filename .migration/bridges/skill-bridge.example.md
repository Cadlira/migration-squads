# Ponte Local de Skill — Exemplo

## Skill alvo
`menu-scripts`

## Objetivo
Adaptar a skill padrão quando o projeto usa nomes diferentes de tabela/coluna.

## Mapeamento local (exemplo)
- tabela de menu: `tb_menu_sistema`
- coluna de URL: `caminho_acao`
- coluna de status: `situacao`
- valor inativo: `I`

## Contrato da ponte
Entrada: `.migration/settings.local.json`
Saída: `.migration/outputs/sql/*.sql` com SQL já adaptado ao esquema local.

## Observação
Se a skill central evoluir, manter apenas este adaptador local atualizado.
