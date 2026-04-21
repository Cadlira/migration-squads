# Skill: rmi-removal

## Objetivo
Remover dependências RMI de forma segura. A remoção pode ser completa e sem substituição quando não houver uso ativo fora do contexto RMI.

## Diretrizes obrigatórias
- Remover completamente artefatos RMI quando não houver dependência ativa em código que permanecerá no sistema.
- Não manter artefatos da tecnologia removida (classes/interfaces/utilitários) sem uso ativo comprovado em outros contextos.
- Se houver uso cruzado (ex.: DTO compartilhado com REST/outro módulo), decidir entre remover, substituir, adaptar ou escalar ao PO quando houver dúvida.
- É proibido afetar partes não relacionadas ao RMI.

## Etapas
1. Mapear contratos e pontos de invocação RMI.
2. Identificar artefatos compartilhados com código ainda ativo (DTOs, utilitários, modelos).
3. Definir ação por artefato compartilhado: remover, substituir, adaptar ou escalar ao PO em caso de dúvida.
4. Remover classes/interfaces `java.rmi` legadas e dependências correlatas sem impacto colateral.

## Critérios de aceite
- Sem importações `java.rmi` no domínio alvo.
- Nenhuma parte não relacionada ao RMI foi impactada.
- Decisões sobre artefatos compartilhados registradas em output local.
