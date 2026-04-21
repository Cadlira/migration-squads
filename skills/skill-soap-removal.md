# Skill: soap-removal

## Objetivo
Remover completamente integrações SOAP/Web Services legadas com segurança e rastreabilidade.

## Premissas de segurança da migração
- Preservar operação em sistemas Java complexos e acoplados (20+ anos).
- É permitido remover SOAP sem substituição quando não houver dependências vivas fora do contexto removido.
- Artefatos compartilhados com uso ativo em fluxos mantidos devem ser preservados ou adaptados.
- Em incerteza de uso ativo entre módulos/fluxos diferentes do sistema, pausar e escalar ao Product Owner para decisão explícita.

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
    - Evidenciar dependências ocultas e chamadas encadeadas antes de qualquer remoção.
5. **Plano de substituição/migração (quando necessário)**
   - Se houver dependência viva fora do contexto SOAP, definir estratégia alvo (ex.: REST) e contrato equivalente.
   - Planejar coexistência (feature flag/adapter), fallback e rollback quando aplicável.
   - Definir testes de compatibilidade e validação de payload/erros.
6. **Execução controlada**
    - Migrar por operação/módulo com entregas pequenas.
    - Remover chamadas SOAP e dependências do fluxo descontinuado após validação de não impacto nos fluxos mantidos.
    - Se houver incerteza de uso ativo entre módulos/fluxos diferentes, interromper remoção e escalar ao Product Owner com opções e impactos.
7. **Fechamento técnico**
   - Consolidar evidências, pendências e riscos residuais.

## Checklist de revisão
- [ ] Todos os endpoints SOAP ativos foram inventariados.
- [ ] Todos os contratos WSDL/XSD críticos foram mapeados.
- [ ] Dependências Axis/JAX-WS e geração de stubs foram identificadas.
- [ ] Pontos de integração (sync/async, batch, satélites) foram analisados.
- [ ] Plano SOAP → REST (ou alvo definido) possui rollback.
- [ ] Testes funcionais e de contrato foram executados e documentados.
- [ ] Não restaram referências SOAP ativas no escopo migrado.

## Sugestões de prompts para análise de código
- "Liste endpoints SOAP deste módulo e onde são consumidos/publicados."
- "Mapeie classes geradas por WSDL e bibliotecas Axis/JAX-WS em uso."
- "Mostre pontos de acoplamento SOAP com impacto em módulos satélites."
- "Proponha plano incremental SOAP → REST com fallback e critérios de aceite."
- "Gere checklist de validação para remover dependências SOAP com segurança."

## Saídas esperadas (disco local)
- `.migration/outputs/soap-inventory.md`
- `.migration/outputs/migration-plan.md`
- `.migration/outputs/validation-checklist.md`

## Critérios de aceite
- Inventário SOAP completo e revisado pelo tech lead.
- Plano de migração aprovado com etapas reversíveis.
- Evidências de validação registradas em outputs locais.
- Sem referências SOAP ativas no escopo migrado e sem impacto em funcionalidades mantidas.
- Decisões de incerteza documentadas com aprovação do Product Owner.
