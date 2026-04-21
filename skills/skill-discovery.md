# Skill: discovery

## Objetivo
Coletar contexto técnico local para orientar as demais skills sem retrabalho, com análise aprofundada e cuidadosa em legados complexos (20+ anos).

## Questionário base
1. Caminho local do projeto.
2. Lista de módulos/satélites acoplados.
3. Versão Java e servidor de aplicação.
4. Dados de conexão PostgreSQL (uso local).
5. Tabelas/colunas de menu e permissões.
6. Mapa inicial de integrações SOAP (endpoints, WSDL e dependências conhecidas).
7. Restrições de deploy e janela de corte.

## Ações
- Inventariar arquivos `.jnlp`, uso de `javax.jnlp`, uso de `java.rmi`.
- Inventariar uso de SOAP: endpoints, clientes/servidores, WSDL/XSD, imports `javax.xml.ws`/`jakarta.xml.ws` e libs (Axis/JAX-WS).
- Inventariar targets ANT ligados a empacotamento/assinatura.
- Verificar se `.migration/settings.local.json` está no `.gitignore` do projeto consumidor.
- Registrar em `.migration/outputs/discovery-report.md` no projeto local.

## Segurança de credenciais
- Não persistir senha/token de banco no discovery report.
- Preferir variáveis de ambiente ou mecanismo seguro local para segredos.

## Entregável mínimo
- Resumo técnico
- Riscos principais
- Ordem recomendada de execução das skills (incluindo `skill-soap-removal` quando aplicável)
