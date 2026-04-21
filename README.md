# Estratégia de Estrutura para Migration Squads

Este documento responde ao dilema de arquitetura para um ecossistema legado Java, com foco inicial em remover RMI/JNLP, modernizar build e manter evolução contínua de skills para futuras migrações.
No contexto analisado, o projeto principal é `atendimento` e os satélites incluem `empresa`, `common`, `controleacesso`, `controleacessoweb`, `utilweb` e `infra`.

## 1) Projeto separado (`migration-squads`) — vantagens e desvantagens

### Vantagens
- **Manutenção centralizada:** uma única base para atualizar playbooks, skills, questionários e fluxos de migração.
- **Governança clara:** mantém regras e processos sob curadoria de um grupo responsável por modernização.
- **Reuso alto:** mesma skill aplicada em múltiplos satélites sem copiar/colar.
- **Onboarding mais simples:** novo membro encontra padrões, templates e histórico de decisões em um local único.
- **Desacoplamento do runtime:** instruções e automações não poluem código de produção.
- **Evolução contínua:** fácil adicionar trilhas novas (upgrade Java, troca de framework, dependências, transações).

### Desvantagens
- **Integração de contexto:** precisa mecanismo para injetar dados locais (paths, módulos, tabelas, convenções de cada repo).
- **Dependência de integração entre repositórios:** pipelines e permissões podem exigir configuração adicional.
- **Risco de distância do código real:** se não houver sincronização periódica, as skills podem ficar genéricas demais.

## 2) Skills dentro do projeto principal/satélites — vantagens e desvantagens

### Vantagens
- **Contexto imediato:** instruções perto do código, com acesso direto a estrutura, build e particularidades.
- **Adoção local rápida:** time já trabalha no repositório e pode executar/ajustar skills sem dependências externas.
- **Versionamento junto da aplicação:** mudanças de migração ficam rastreáveis no histórico do projeto.

### Desvantagens
- **Manutenção espalhada:** cada satélite tende a divergir no tempo.
- **Governança fragmentada:** difícil impor padrão único de execução e qualidade.
- **Baixa reusabilidade global:** atualização de skill precisa replicação em vários repositórios.
- **Onboarding mais custoso:** time precisa navegar múltiplos repositórios para entender o processo completo.
- **Acoplamento indesejado:** risco de misturar diretrizes de migração com ciclo de vida do produto.

## 3) Recomendação para múltiplos satélites fortemente acoplados e evolução em ritmos diferentes

**Recomendação principal:** adotar **repositório central de orquestração** como fonte de verdade, com adaptadores mínimos por projeto.

Motivo:
- O acoplamento funcional entre satélites pede consistência de estratégia.
- A evolução em tempos diferentes pede autonomia local controlada.
- O centro define padrão; o local aplica parametrização.

## 4) Modelo híbrido recomendado (central + local)

### Camada central (`migration-squads`)
- Catálogo de skills base:
  - remoção de RMI e JNLP
  - modernização de build
  - atualização de dependências
  - migração de framework
  - migração de transações
- Questionários de discovery e templates de plano de migração.
- Padrões de governança (papéis, critérios de aceite, gates de qualidade).
- Versionamento semântico das skills e changelog.

### Camada local (em cada repo legado)
- Pasta enxuta (ex.: `.migration/`) com:
  - `context.yml` (módulos, paths, banco, tabelas relevantes)
  - overrides estritamente necessários
  - relatórios de execução local
- Sem duplicar skill completa quando for possível só parametrizar.

## 5) Extensão futura (lógica, dependências, frameworks, upgrades Java)

Para escalar no tempo:
- **Versionar skills** (ex.: `v1`, `v2`) com compatibilidade explícita.
- **Classificar skills por trilha**: build, arquitetura, persistência, transações, segurança, observabilidade.
- **Executar por fases**: discovery → plano → piloto em satélite menos crítico → rollout progressivo.
- **Medir impacto**: tempo de execução, cobertura de migração, falhas por etapa.
- **Manter rollback definido** em migrações de maior risco (ex.: transação proprietária para Spring + Hibernate).

---

## Decisão objetiva

Para este cenário legado multi-repositório, a melhor estratégia é **modelo híbrido com governança central**:
- **Centralizar** orquestrador e skills reutilizáveis no `migration-squads`.
- **Localizar** apenas contexto e ajustes específicos em cada satélite.

Assim você maximiza padronização, reduz retrabalho, facilita onboarding e preserva desacoplamento entre modernização e runtime de produção.
