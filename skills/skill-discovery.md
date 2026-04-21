# Skill: discovery

## Objetivo
Coletar contexto técnico local para orientar as demais skills sem retrabalho, com análise aprofundada e cuidadosa em legados complexos (20+ anos).

## Questionário inicial de seleção (obrigatório)
Antes de iniciar qualquer análise/pesquisa, perguntar quais jornadas o usuário deseja executar agora (seleção única ou múltipla):
- `menu-scripts`
- `jnlp-removal`
- `rmi-removal`
- `soap-removal`
- `ant-migration`

Registrar no report a seleção confirmada. Somente as skills selecionadas podem disparar análise detalhada.

## Questionário base
1. Caminho local do projeto.
2. Lista de módulos/satélites acoplados.
3. Versão Java e servidor de aplicação.
4. Dados de conexão PostgreSQL (uso local).
5. Tabelas/colunas de menu e permissões.
6. Mapa inicial de integrações SOAP (endpoints, WSDL e dependências conhecidas).
7. Restrições de deploy e janela de corte.

## Ações
- Executar somente análises ligadas às skills selecionadas no questionário inicial.
- Se `jnlp-removal` estiver selecionada: inventariar arquivos `.jnlp` e uso de `javax.jnlp`.
- Se `rmi-removal` estiver selecionada: inventariar uso de `java.rmi` e pontos de chamada.
- Se `soap-removal` estiver selecionada: inventariar endpoints/clientes/servidores, WSDL/XSD, imports `javax.xml.ws`/`jakarta.xml.ws` e libs (Axis/JAX-WS).
- Se `ant-migration` estiver selecionada: inventariar targets ANT ligados a empacotamento/assinatura.
- Se `menu-scripts` estiver selecionada: mapear tabelas/colunas de menu e permissões para auditoria/ação.
- Verificar se `.migration/settings.local.json` está no `.gitignore` do projeto consumidor.
- Registrar em `.migration/outputs/discovery-report.md` no projeto local.

## Segurança de credenciais
- Não persistir senha/token de banco no discovery report.
- Preferir variáveis de ambiente ou mecanismo seguro local para segredos.

## Entregável mínimo
- Resumo técnico
- Riscos principais
- Skills selecionadas e justificativa
- Ordem recomendada de execução considerando somente as selecionadas
