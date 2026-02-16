# 15. git reset - Moviendo Referencias

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 15. git reset - Moviendo Referencias

**¿Qué hace?**
Mueve HEAD y rama actual, opcionalmente modificando staging y working.

**Funcionamiento interno:**
```
Tres modos:
--soft:  Solo mueve HEAD/rama
--mixed: Mueve HEAD/rama + resetea staging
--hard:  Mueve HEAD/rama + resetea staging + working
```

**Uso práctico:**

```bash
# 1. Reset suave (mantiene cambios en staging)
git reset --soft HEAD~1
# → Deshace commit
# → Cambios vuelven a staging
# → Útil para rehacer commit

# 2. Reset mixto (default, cambios en working)
git reset HEAD~1
# o: git reset --mixed HEAD~1
# → Deshace commit
# → Cambios vuelven a working directory
# → Útil para reorganizar qué commitear

# 3. Reset duro (¡PIERDES CAMBIOS!)
git reset --hard HEAD~1
# → Deshace commit
# → BORRA todos los cambios
# → ⚠️ PELIGROSO: no recuperable sin reflog

# 4. Unstage archivo (quitar del staging)
git reset HEAD archivo.txt
# → Mueve archivo de staging a working
# → NO modifica el último commit

# 5. Reset a commit específico
git reset --soft abc123
git reset --mixed abc123
git reset --hard abc123

# 6. Reset a remoto
git reset --hard origin/main
# → Sincroniza con remoto, descartando cambios locales

# 7. Reset de un directorio específico
git reset HEAD directorio/
```

**FLUJO DE ESTADOS con reset:**

```bash
# ESTADOS EN GIT:
# Working Directory → Staging (Index) → Commit → Remote
#
# COMANDOS PARA AVANZAR:
# Working → Staging:   git add <archivo>
# Staging → Commit:    git commit
# Commit → Remote:     git push
#
# COMANDOS PARA RETROCEDER (reset):
# Staging → Working:   git reset HEAD <archivo>
# Commit → Staging:    git reset --soft HEAD~1
# Commit → Working:    git reset --mixed HEAD~1 (default)
# Commit → (borrado):  git reset --hard HEAD~1 (PELIGRO)
```

**Casos de uso prácticos:**

```bash
# Caso 1: Quitar un archivo del último commit
git reset --soft HEAD~1     # Deshace commit → archivos a staging
git reset HEAD archivo.txt  # Quita archivo del staging
git commit -m "Mensaje"     # Recommitea sin ese archivo

# Caso 2: Rehacer último commit con más cambios
git reset --soft HEAD~1     # Deshace commit → archivos a staging
git add mas-cambios.txt     # Añade más archivos
git commit -m "Mensaje completo"

# Caso 3: Deshacer commit y revisar cambios
git reset HEAD~1            # Cambios a working directory
git diff                    # Revisa qué cambiaste
git add -p                  # Añade selectivamente
git commit -m "Mejor mensaje"

# Caso 4: Unstage archivo antes de commit
git add .                   # Añadiste todo
git reset HEAD config.txt   # Quitas un archivo del staging
git commit -m "Mensaje"     # Commiteas sin config.txt

# Caso 5: Limpiar todo y empezar de nuevo
git reset --hard HEAD       # Descarta TODOS los cambios
git clean -fd               # Elimina archivos untracked

# Caso 6: Deshacer múltiples commits
git reset --soft HEAD~3     # Deshace 3 commits → staging
git commit -m "Squashed commit"  # Un solo commit
```

**Comparación de modos:**

```bash
git reset --soft HEAD~1
→ Commit deshecho
→ Cambios en staging ✓
→ Working intacto ✓

git reset HEAD~1  (mixed, default)
→ Commit deshecho
→ Cambios en working ✓
→ Staging limpio

git reset --hard HEAD~1
→ Commit deshecho
→ Staging limpio
→ Working limpio
→ ¡CAMBIOS PERDIDOS!
```

**Reset vs Revert:**

```bash
RESET (reescribe historia):
→ Mueve rama atrás
→ Commits "desaparecen"
→ Solo para commits locales

REVERT (preserva historia):
→ Crea nuevo commit que deshace
→ Historia intacta
→ Seguro para commits públicos
```

**Recuperación:**

```bash
# Si hiciste reset por error:
git reflog
git reset --hard HEAD@{1}
```

**Troubleshooting común:**

```bash
# Problema 1: Hice reset --hard por error
# Solución: Usar reflog para recuperar
git reflog                  # Encuentra el commit perdido
git reset --hard HEAD@{2}   # Vuelve a ese estado

# Problema 2: No sé qué modo de reset usar
# Solución:
# --soft:  Solo quieres rehacer el commit, mantener cambios en staging
# --mixed: Quieres revisar/reorganizar antes de commitear de nuevo
# --hard:  Quieres BORRAR todo (úsalo con cuidado)

# Problema 3: Reset no funciona como esperaba
# Solución: Verifica el estado antes y después
git log --oneline           # Ve dónde estás
git reset --soft HEAD~1
git status                  # Verifica que cambios están en staging

# Problema 4: Quiero deshacer reset
# Solución: Usar reflog
git reflog
git reset --hard HEAD@{1}   # Vuelve al estado anterior

# Problema 5: Reset en rama compartida
# Solución: NO hagas reset en ramas públicas
# Usa git revert en su lugar (ver sección de revert)
```

**Mejores prácticas:**

```bash
✓ Usa --soft para reorganizar commits
✓ Usa --mixed para unstage
✓ Usa --hard solo si estás seguro
✓ Recuerda: reflog es tu red de seguridad

✗ No uses reset --hard en commits públicos
✗ No uses reset en main/develop compartidos
✗ Evita reset --hard sin verificar cambios
```

---


---

## Navegación

- [⬅️ Anterior: git push](14-git-push.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git stash](16-git-stash.md)

