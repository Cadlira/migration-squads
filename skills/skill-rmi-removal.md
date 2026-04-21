# Skill: rmi-removal

## Objetivo
Substituir dependências RMI por integração moderna e sustentável.

## Etapas
1. Mapear contratos e pontos de invocação RMI.
2. Definir padrão alvo (REST/gRPC/evento).
3. Criar camada adaptadora para coexistência temporária quando necessário.
4. Remover classes/interfaces `java.rmi` legadas.

## Critérios de aceite
- Sem importações `java.rmi` no domínio alvo.
- Contratos de integração documentados em output local.
