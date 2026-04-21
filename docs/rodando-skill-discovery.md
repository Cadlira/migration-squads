# Guia: rodando a skill discovery para projetos legados externos (ex.: Atendimento)

Este guia mostra como usar a skill central `skills/skill-discovery.md` do `migration-squads` para analisar um projeto legado externo/satélite (por exemplo, `Atendimento`) e integrar os outputs no repositório alvo.

## Quando usar este fluxo

Use este procedimento quando:
- o orquestrador central está neste repositório;
- o projeto real a ser analisado está em outro repositório/pasta;
- você quer manter a padronização da squad com adaptação local no projeto consumidor.

## Pré-requisitos

1. Repositório `migration-squads` clonado e aberto no Copilot Chat.
2. Projeto legado externo disponível em caminho absoluto (ex.: `/home/user/dev/Atendimento`).
3. Permissão local para criar/atualizar a pasta `.migration/` no projeto legado.

## Entradas, contexto e saídas esperadas

### Entradas (questionário base da discovery)
Use como referência o conteúdo de `skills/skill-discovery.md`:
1. Caminho local do projeto legado.
2. Lista de módulos/satélites acoplados.
3. Versão Java e servidor de aplicação.
4. Dados de conexão PostgreSQL (sem expor senha em texto plano).
5. Tabelas/colunas de menu e permissões.
6. Restrições de deploy e janela de corte.

### Contexto mínimo que deve ser informado no prompt
- que a skill é a central (`skills/skill-discovery.md`);
- qual é o projeto alvo (nome + caminho absoluto);
- onde salvar outputs no projeto consumidor (`.migration/outputs/`).

### Saídas esperadas
- relatório de discovery em `.migration/outputs/discovery-report.md` no projeto legado;
- riscos principais e ordem sugerida das próximas skills;
- inventário inicial de ocorrências ligadas a JNLP, RMI e ANT.

## Passo a passo

### 1) Abra o repositório central no chat
Abra este repositório (`migration-squads`) no VS Code, IntelliJ ou Eclipse com Copilot Chat habilitado.

### 2) Informe que o alvo é um projeto externo
Use prompt explícito com caminho absoluto do projeto legado:

```text
Execute a skill discovery usando skills/skill-discovery.md para o projeto Atendimento em /home/user/dev/Atendimento.
Considere esse projeto como alvo e gere os artefatos em /home/user/dev/Atendimento/.migration/outputs/.
```

### 3) Responda o questionário da discovery
Preencha os itens solicitados (módulos, Java/app server, banco, tabelas de menu, restrições de deploy).

> Segurança: não informe senha de banco em texto plano no chat. Use variável de ambiente (`database.passwordEnv`) e configuração local não versionada.

### 4) Valide os outputs no projeto legado
No projeto `Atendimento`, confira:
- `/home/user/dev/Atendimento/.migration/outputs/discovery-report.md`
- `/home/user/dev/Atendimento/.migration/outputs/migration-plan.md` (quando a análise já sugerir próximos passos)
- `/home/user/dev/Atendimento/.migration/outputs/validation-checklist.md` (quando houver checklist inicial de validação)

### 5) Integre os outputs com o fluxo local
No projeto consumidor:
1. Mantenha/adapte `.migration/settings.local.json` (não versionar segredo);
2. Mantenha `.migration/outputs/` como trilha de evidências;
3. Use o discovery report para iniciar as próximas skills (menu-scripts, jnlp-removal, rmi-removal, ant-migration).

## Exemplos de comandos/prompts

### Copilot Chat (VS Code / IntelliJ / Eclipse)
```text
Preciso rodar a skill discovery central (skills/skill-discovery.md) para o projeto Atendimento em /home/user/dev/Atendimento.
Me conduza pelo questionário e gere os outputs em /home/user/dev/Atendimento/.migration/outputs/.
```

### Copilot CLI
```sh
gh copilot suggest "Rode a skill discovery de skills/skill-discovery.md para /home/user/dev/Atendimento e liste entradas necessárias e outputs esperados"
```

### Prompt para múltiplos satélites
```text
Execute a discovery para /home/user/dev/Atendimento, considerando satélites /home/user/dev/empresa e /home/user/dev/infra.
Consolide riscos e ordem recomendada das skills em .migration/outputs/discovery-report.md no Atendimento.
```

## Checklist rápido

- [ ] Caminho absoluto do projeto alvo informado
- [ ] Questionário da discovery respondido
- [ ] Outputs gerados em `<projeto-alvo>/.migration/outputs/`
- [ ] Segredos fora de versionamento e sem senha em texto plano
- [ ] Próximas skills priorizadas com base no discovery report
