# Skill: soap-removal

## Objetivo
Remover integrações SOAP/Web Services legadas com segurança e rastreabilidade, preservando operação em sistemas Java complexos e acoplados (20+ anos).

## Diretrizes obrigatórias
- É permitido remover SOAP sem substituição quando o sistema não fizer mais uso dessa tecnologia.
- Não manter artefatos SOAP (stubs, proxies, utilitários e classes de suporte) sem uso ativo comprovado fora do contexto SOAP.
- Em uso cruzado por código ativo, decidir entre remover, substituir, adaptar ou escalar ao PO quando houver dúvida.
- É proibido afetar o funcionamento de partes não relacionadas ao SOAP.

## Quando usar
- Há uso de SOAP como cliente e/ou provedor no legado.
- O discovery identificou WSDLs, stubs gerados, handlers ou bibliotecas SOAP (Axis/JAX-WS).

## Entradas
- Discovery report com módulos e riscos.
- Lista de endpoints, contratos WSDL/XSD e operações críticas.
- Janela de deploy, restrições de coexistência e requisitos de compatibilidade.

## Fluxo detalhado de execução
1. **Identificação de endpoints**
   - Localizar URLs, serviços publicados/consumidos e ambientes (dev/hml/prod).
   - Relacionar endpoint com módulo, dono técnico e criticidade.
2. **Mapeamento de contratos**
   - Inventariar WSDL/XSD versionados e gerados em build.
   - Listar operações SOAP e campos críticos (headers, autenticação, fault contracts).
3. **Levantamento de dependências**
   - Identificar bibliotecas e plugins (`axis`, `axis2`, `jax-ws`, `javax.xml.ws`, `jakarta.xml.ws`).
   - Identificar geração de código (wsimport/wsdl2java) e artefatos gerados.
4. **Mapeamento de pontos de integração**
   - Encontrar stubs, proxies, gateways, serviços de borda, jobs e pontos batch.
   - Registrar impacto cruzado com satélites acoplados.
5. **Decisão de retirada por contexto**
   - Definir se o caso é remoção pura (sem substituição) ou migração/substituição parcial.
   - Para uso cruzado, definir adaptação/substituição por artefato compartilhado.
   - Em dúvidas de impacto, escalar ao PO com opções e efeitos esperados.
6. **Execução controlada**
   - Migrar por operação/módulo com entregas pequenas.
   - Remover chamadas SOAP e dependências sem impactar partes não relacionadas.
7. **Fechamento técnico**
   - Consolidar evidências, pendências e riscos residuais.

## Checklist de revisão
- [ ] Todos os endpoints SOAP ativos foram inventariados.
- [ ] Todos os contratos WSDL/XSD críticos foram mapeados.
- [ ] Dependências Axis/JAX-WS e geração de stubs foram identificadas.
- [ ] Pontos de integração (sync/async, batch, satélites) foram analisados.
- [ ] Decisão de remoção pura ou substituição parcial foi registrada por contexto.
- [ ] Testes funcionais e de contrato foram executados e documentados.
- [ ] Não restaram referências SOAP ativas no escopo migrado.
- [ ] Não houve regressão em código ativo não relacionado ao SOAP.

## Sugestões de prompts para análise de código
- "Liste endpoints SOAP deste módulo e onde são consumidos/publicados."
- "Mapeie classes geradas por WSDL e bibliotecas Axis/JAX-WS em uso."
- "Mostre pontos de acoplamento SOAP com impacto em módulos satélites."
- "Avalie se este módulo pode remover SOAP sem substituição e liste impactos."
- "Se houver uso cruzado de DTOs/utilitários, proponha remover, substituir ou adaptar com critérios de aceite."
- "Gere checklist de validação para remover dependências SOAP com segurança."

## Saídas esperadas (disco local)
- `.migration/outputs/soap-inventory.md`
- `.migration/outputs/migration-plan.md`
- `.migration/outputs/validation-checklist.md`

## Critérios de aceite
- Inventário SOAP completo e revisado pelo tech lead.
- Plano de migração aprovado com etapas reversíveis.
- Evidências de validação registradas em outputs locais.
