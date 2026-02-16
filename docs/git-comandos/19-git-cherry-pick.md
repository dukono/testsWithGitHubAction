# 19. git cherry-pick - Aplicando Commits Selectivos

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 19. git cherry-pick - Aplicando Commits Selectivos

**¿Qué hace?**
Aplica cambios de commit específico a rama actual.

**Funcionamiento interno:**
```
1. Lee commit a cherry-pick
2. Calcula diff
3. Aplica diff a rama actual
4. Crea NUEVO commit (hash diferente)
```

**Uso práctico:**

```bash
# Cherry-pick básico
git cherry-pick abc123

# Sin commit automático
git cherry-pick --no-commit abc123

# Múltiples commits
git cherry-pick abc123 def456 ghi789
git cherry-pick abc123..ghi789

# Con nota de origen
git cherry-pick -x abc123
# Añade: (cherry picked from commit abc123)

# Abortar/continuar
git cherry-pick --abort
git cherry-pick --continue
```

**Casos de uso:**

```bash
# Hotfix de producción
git checkout production
git cherry-pick abc123  # Fix de develop
git push origin production

# Backport a versión anterior
git checkout release-2.0
git cherry-pick def456  # Feature de main
git push origin release-2.0

# Mover commits entre ramas
git checkout rama-correcta
git cherry-pick abc123
git checkout rama-incorrecta
git reset --hard HEAD~1
```

**Cherry-pick vs Merge:**

```bash
MERGE:
→ Trae toda la rama
→ Merge commit
→ Historia completa

CHERRY-PICK:
→ Solo commits específicos
→ Sin merge commit
→ Commits duplicados

¿Cuándo usar?
MERGE: Feature completa
CHERRY-PICK: Hotfixes, backports
```

**Mejores prácticas:**

```bash
✓ Usa cherry-pick para fixes urgentes
✓ Usa -x para rastrear origen
✓ Usa --no-commit para combinar múltiples

✗ No uses como reemplazo de merge
✗ No cherry-picks en exceso
✗ Evita cherry-pick de merges sin -m
```

---


---

## Navegación

- [⬅️ Anterior: git revert](18-git-revert.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git clean](20-git-clean.md)

