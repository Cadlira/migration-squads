# Integração Local (`.migration/`)

Este diretório é o ponto de integração local dos projetos consumidores da infraestrutura central.

## Estrutura sugerida
- `.migration/settings.local.example.json` → parâmetros locais (sem segredo real no repositório).
- `.migration/bridges/` → ponte/adaptador de skills para contexto do projeto.
- `.migration/outputs/` → relatórios, scripts e evidências geradas.

## Exemplo de fluxo local
1. Copiar `settings.local.example.json` para `settings.local.json` (local, não versionar segredos).
   - Adicionar `.migration/settings.local.json` no `.gitignore` **de cada projeto consumidor**.
2. Preencher path do projeto, módulos satélites e metadados de banco.
3. Rodar `discovery` usando a configuração local.
4. Executar skills na sequência recomendada pelo discovery.

## Adaptação de settings
Use aliases por skill para mapear particularidades locais:
- nomes de tabelas/colunas;
- padrões de diretórios de build;
- módulos que exigem exceção de migração.

## Adaptação de outputs
Padronize outputs para facilitar governança:
- `outputs/discovery-report.md`
- `outputs/sql/*.sql`
- `outputs/migration-plan.md`
- `outputs/validation-checklist.md`
