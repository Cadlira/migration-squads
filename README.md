# migration-squads

Repositório central para **orquestração de migração de Java legado** com foco em cenários como:
- remoção de **RMI**;
- remoção de **JNLP/Java Web Start**;
- modernização de build **ANT**;
- evolução contínua para novas frentes (upgrade de dependências, migração de transações proprietárias para Spring + Hibernate, etc.).

A proposta é funcionar como um **orquestrador mestre de skills e agentes**, reutilizável entre múltiplos projetos satélites (ex.: `atendimento`, `empresa`, `common`, `infra`, etc.), reduzindo retrabalho e padronizando execução.

## O que existe neste repositório

- **Orquestrador central**: `.github/copilot-instructions.md`
- **Prompt mestre**: `prompts/master-prompt.md`
- **Agentes por papel**: `agents/`
- **Skills reutilizáveis**: `skills/`
- **Template para novas skills**: `skills/TEMPLATE-new-skill.md`
- **Modelo de integração local**: `.migration/`

## Como usar a infraestrutura (modelo híbrido recomendado)

### 1) Infra central (este repositório)
Use este repositório para manter:
- padrões de execução;
- protocolo de agentes;
- skills genéricas de migração;
- governança e evolução da squad.

### 2) Adaptação local (em cada projeto consumidor)
Em cada projeto legado/satélite, crie e adapte o diretório `.migration/` com:
- `settings.local.json` (a partir de `.migration/settings.local.example.json`);
- `bridges/` para adaptar diferenças de tabela, colunas, paths e exceções;
- `outputs/` para evidências (discovery, SQL, plano, checklist).

> Segurança: não versionar credenciais. Use variável de ambiente (`database.passwordEnv`) e garanta no `.gitignore` do projeto consumidor:
> - `.migration/settings.local.json`
> - Exemplo: manter `"passwordEnv": "PGPASSWORD"` no `settings.local.json` e carregar `PGPASSWORD` por mecanismo seguro (secret manager/arquivo local não versionado). **Não** defina senha diretamente por `export ...` digitado no terminal para não vazar em histórico.

## Fluxo básico de orquestração das skills (passo a passo)

Para novos devs/squads, siga esta ordem:

1. **Discovery** (`skills/skill-discovery.md`)
   - inventaria RMI, JNLP, ANT, contexto de banco e riscos;
   - gera base de decisão em `.migration/outputs/`.
2. **Menu Scripts** (`skills/skill-menu-scripts.md`)
   - gera SQL de auditoria e ação (SELECT → UPDATE → DELETE) para menus JNLP.
3. **JNLP Removal** (`skills/skill-jnlp-removal.md`)
   - remove arquivos/referências JNLP e assinatura legada no build.
4. **RMI Removal** (`skills/skill-rmi-removal.md`)
   - mapeia chamadas RMI e substitui por integração moderna.
5. **ANT Migration** (`skills/skill-ant-migration.md`)
   - evolui ANT ou migra para Maven/Gradle com CI reproduzível.
6. **Encerramento**
   - consolida evidências, riscos residuais e próximos passos.

## Como rodar com Copilot (VS Code e IntelliJ)

> Objetivo: usar Copilot Chat para orquestrar as etapas e Copilot CLI para acelerar comandos e revisões no terminal.

### VS Code (Copilot Chat + Copilot CLI)

#### Setup básico
1. Instale **GitHub Copilot** e **GitHub Copilot Chat** no VS Code.
2. Faça login com sua conta GitHub licenciada.
3. Se for usar multi-root (recomendado para prompts com contexto cruzado), no VS Code faça: `File > Add Folder to Workspace...` e adicione o projeto legado + este repositório.
4. Alternativa: use duas janelas separadas (mais simples quando o time prefere separar consulta e implementação).
5. (Opcional) Instale GitHub CLI e extensão Copilot CLI (`gh` + comandos `gh copilot ...`).

> Dica: para melhor resultado no Chat, cite a skill por caminho (ex.: `skills/skill-discovery.md`) para ancorar o contexto.

#### Exemplos de prompts no Copilot Chat (VS Code)
- "Execute a skill `discovery` considerando este projeto e gere saída em `.migration/outputs/discovery-report.md`."
- "Com base no discovery, rode `menu-scripts` e gere SQL de auditoria, desativação e remoção."
- "Aplique `jnlp-removal` e liste evidências de que não restou referência ativa fora de documentação."

#### Exemplos de uso no terminal (Copilot CLI)
- `gh copilot suggest "listar arquivos .jnlp e imports javax.jnlp neste projeto"`
- `gh copilot suggest "mapear ocorrências java.rmi e sugerir estratégia de substituição por REST"`
- `gh copilot explain "ant -p"`

### IntelliJ IDEA (Copilot Chat)

#### Setup básico
1. Instale o plugin **GitHub Copilot** no IntelliJ (`Settings > Plugins`).
2. Faça login com sua conta GitHub.
3. Abra o projeto legado na janela principal e mantenha este repositório aberto em **segunda janela** (ou como projeto adicional) para consulta de skills.

#### Exemplos de prompts no Copilot Chat (IntelliJ)
- Dica: para melhor resultado no Chat, cite a skill por caminho (ex.: `skills/skill-discovery.md`) para ancorar o contexto.
- "Use o fluxo da squad: discovery → menu-scripts → jnlp-removal → rmi-removal → ant-migration. Comece pelo discovery."
- "Analise este módulo e gere checklist de remoção de RMI com critérios de aceite."
- "Sugira plano de migração de ANT para Maven sem quebrar artefatos atuais."

## Como estender para novas migrações

1. Crie nova skill a partir de `skills/TEMPLATE-new-skill.md`.
2. Defina claramente: **objetivo, entradas, protocolo, saídas e critérios de aceite**.
3. Se a necessidade for genérica, mantenha no repositório central (`skills/`).
4. Se for específica de um projeto, adicione adaptação em `.migration/bridges/` no projeto consumidor.
5. Garanta compatibilidade com outputs existentes para não quebrar o fluxo entre squads.

Exemplos de novas skills futuras:
- upgrade Java 8 → 17/21;
- Struts 1.1 → stack moderna;
- Hibernate 3 → Spring Data/Hibernate atualizado;
- transação proprietária → Spring Transaction Management.

## FAQ rápido (onboarding)

**1) Preciso usar tudo de uma vez?**
Não. Comece por `discovery` e avance por fases.

**2) Posso usar só no projeto principal?**
Pode, mas o ganho maior vem ao aplicar o padrão também nos satélites.

**3) Onde ficam scripts e evidências?**
No projeto consumidor, em `.migration/outputs/`.

**4) Como tratar credenciais de banco?**
Nunca commitar senha; use variável de ambiente no `settings.local.json`.

**5) Posso adaptar uma skill sem forkar tudo?**
Sim. Use ponte local em `.migration/bridges/` e mantenha a skill central reutilizável.

---

## Contribuição e evolução da squad

Esta squad é **extensível por design**. Times são encorajados a contribuir com novas skills, melhorias de fluxo e adaptações para migrações futuras.
