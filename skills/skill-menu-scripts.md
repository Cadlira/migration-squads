# Skill: menu-scripts

## Objetivo
Gerar scripts SQL para auditoria, desativação e remoção de menus ligados a JNLP.

## Protocolo
1. Gerar SELECT de auditoria.
2. Gerar UPDATE reversível para desativar.
3. Gerar DELETE apenas após validação funcional.
4. Incluir rollback.

## Modelo base
```sql
-- 001_auditoria.sql
SELECT id, name, url, status
FROM menu
-- Ajuste o padrão para o formato real da URL local.
WHERE url ILIKE 'jnlp:%'
   OR url ILIKE '%.jnlp'
   OR url ILIKE '%/jnlp/%';

-- 002_desativacao.sql
UPDATE menu
SET status = 'INATIVO'
WHERE url ILIKE 'jnlp:%'
   OR url ILIKE '%.jnlp'
   OR url ILIKE '%/jnlp/%';

-- 003_remocao.sql
DELETE FROM menu
WHERE url ILIKE 'jnlp:%'
   OR url ILIKE '%.jnlp'
   OR url ILIKE '%/jnlp/%';
```

## Observação de performance
Em tabelas grandes, qualquer `ILIKE` com wildcard inicial (como `%.jnlp` e `%/jnlp/%`) pode gerar full scan. Se necessário, use filtro mais específico ou índice funcional apropriado antes da execução em produção.
Quando possível, adicione filtro seletivo extra (ex.: status ativo ou janela de datas) para reduzir custo de varredura.

## Saída esperada
Arquivos SQL em `.migration/outputs/sql/` do projeto local.
