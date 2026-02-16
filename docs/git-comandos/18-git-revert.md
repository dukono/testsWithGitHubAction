# 18. git revert - Deshaciendo Commits Públicos

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 18. git revert - Deshaciendo Commits Públicos

**¿Qué hace?**
Crea NUEVO commit que deshace cambios de commit anterior.

**Funcionamiento interno:**
```
1. Lee commit a revertir
2. Calcula inverso de cambios
3. Aplica cambios inversos
4. Crea nuevo commit
5. Historia se mantiene intacta
```

**Uso práctico:**

```bash
# Revert de commit
git revert abc123
git revert abc123 --no-edit

# Revert de HEAD
git revert HEAD
git revert HEAD~3

# Revert múltiples
git revert HEAD~3..HEAD
git revert abc123 def456 ghi789

# Revert sin commit automático
git revert --no-commit abc123

# Revert de merge commit
git revert -m 1 abc123
# -m 1 = mantiene padre 1 (main line)

# Abortar/continuar
git revert --abort
git revert --continue
```

**Revert vs Reset:**

```bash
RESET (reescribe historia):
→ Mueve rama atrás
→ Commits desaparecen
→ Solo commits locales

REVERT (preserva historia):
→ Nuevo commit que deshace
→ Historia intacta
→ Seguro para commits públicos

¿Cuándo usar cada uno?
RESET: Commits locales no pusheados
REVERT: Commits ya pusheados/públicos
```

**Mejores prácticas:**

```bash
✓ Usa revert para commits públicos
✓ Usa --no-commit para múltiples como uno
✓ Incluye razón del revert en mensaje
✓ Usa -m 1 para revert de merges

✗ No uses revert para commits locales (usa reset)
✗ No omitas -m en revert de merge
```

---


---

## Navegación

- [⬅️ Anterior: git tag](17-git-tag.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git cherry-pick](19-git-cherry-pick.md)

