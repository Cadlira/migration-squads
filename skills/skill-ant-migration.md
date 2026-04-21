# Skill: ant-migration

## Objetivo
Atualizar build legado ANT para fluxo sustentável de manutenção.

## Estratégias aceitas
- Evolução incremental no ANT (curto prazo), ou
- Migração para Maven/Gradle (médio prazo).

## Passos mínimos
1. Mapear targets atuais e equivalentes.
2. Retirar etapas específicas de JNLP.
3. Preservar geração de artefatos necessários.
4. Garantir execução em CI.

## Critério de aceite
Build reproduzível e documentado com comandos únicos de execução.
