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
SELECT id, nome, url, status
FROM menu
WHERE url ILIKE '%jnlp%';

-- 002_desativacao.sql
UPDATE menu
SET status = 'INATIVO'
WHERE url ILIKE '%jnlp%';

-- 003_remocao.sql
DELETE FROM menu
WHERE url ILIKE '%jnlp%';
```

## Saída esperada
Arquivos SQL em `.migration/outputs/sql/` do projeto local.
