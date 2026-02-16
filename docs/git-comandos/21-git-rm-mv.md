# 21. git rm y git mv - Eliminando y Moviendo Archivos

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 21. git rm y git mv - Eliminando y Moviendo Archivos
[⬆️ Top](#tabla-de-contenidos)

**¿Qué hace?**
Elimina o mueve archivos en Git y working directory.

**Funcionamiento interno:**
```
git rm:
1. Elimina archivo del filesystem
2. Actualiza .git/index
3. Cambio stageado (necesitas commitear)

git mv:
1. git rm old
2. git add new
3. Git detecta rename automáticamente
```

**Uso práctico - git rm:**

```bash
# Eliminar archivo (disk + Git)
git rm archivo.txt

# Eliminar solo de Git (mantener en disk)
git rm --cached archivo.txt

# Eliminar forzado
git rm -f archivo.txt

# Eliminar directorio
git rm -r directorio/

# Con wildcards
git rm '*.txt'
```

**Uso práctico - git mv:**

```bash
# Mover/renombrar archivo
git mv viejo.txt nuevo.txt

# Mover a directorio
git mv archivo.txt src/

# Renombrar directorio
git mv old-dir/ new-dir/
```

**rm/mv vs git rm/mv:**

```bash
# RM (comando shell):
rm archivo.txt
git add archivo.txt
→ 2 pasos

# GIT RM:
git rm archivo.txt
→ 1 paso, automáticamente stageado

# MV (comando shell):
mv old.txt new.txt
git rm old.txt && git add new.txt
→ Git detecta rename igual

# GIT MV:
git mv old.txt new.txt
→ Más claro, rename explícito
```

**Casos de uso:**

```bash
# Eliminar archivo sensible
git rm --cached .env
echo ".env" >> .gitignore
git add .gitignore
git commit -m "chore: Remove .env from tracking"

# Reorganizar proyecto
git mv lib/*.js src/lib/
git commit -m "refactor: Reorganize structure"

# Case-sensitive rename (macOS/Windows)
git mv readme.md temp
git mv temp README.md
git commit -m "docs: Fix README capitalization"
```

**Mejores prácticas:**

```bash
✓ Usa git rm en vez de rm (más claro)
✓ Usa git mv en vez de mv (detecta rename)
✓ Usa --cached para unstage sin eliminar
✓ Commitea después de rm/mv

✗ No uses rm -rf .git (NUNCA)
✗ No uses git rm -f sin revisar
✗ No olvides commitear después
```

---

## WORKFLOWS COMUNES

### Workflow 1: Feature Branch

```bash
# Crear feature
git checkout -b feature/user-auth
# ... desarrollo ...
git add .
git commit -m "feat: Add user authentication"
git push -u origin feature/user-auth

# PR en GitHub
# Tras aprobación:
git checkout main
git pull origin main
git merge --no-ff feature/user-auth
git push origin main
git branch -d feature/user-auth
git push origin --delete feature/user-auth
```

### Workflow 2: Fork Contribution

```bash
# Setup
git clone https://github.com/tu-fork/proyecto.git
cd proyecto
git remote add upstream https://github.com/original/proyecto.git

# Sincronizar
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

# Contribuir
git checkout -b fix/bug-123
# ... fixes ...
git commit -am "fix: Resolve issue #123"
git push -u origin fix/bug-123
# PR a upstream
```

### Workflow 3: Hotfix

```bash
# Hotfix urgente
git checkout main
git checkout -b hotfix/critical-bug
# ... fix ...
git commit -am "fix: Critical security issue"
git push -u origin hotfix/critical-bug

# Fast merge
git checkout main
git merge hotfix/critical-bug
git push origin main
git branch -d hotfix/critical-bug

# Tag
git tag -a v1.0.1 -m "Hotfix: Security patch"
git push origin v1.0.1
```

---

## TROUBLESHOOTING RÁPIDO

### Deshacer cambios

```bash
# Archivo modificado, no stageado
git restore archivo.txt

# Archivo stageado
git restore --staged archivo.txt

# Último commit (local)
git reset --soft HEAD~1

# Último commit (público)
git revert HEAD

# Múltiples commits (local)
git reset --hard HEAD~3

# Working directory completo
git reset --hard HEAD
git clean -fxd
```

### Recuperar trabajo perdido

```bash
# Ver reflog
git reflog

# Recuperar commit
git checkout abc123
git branch rescue-branch

# Recuperar después de reset
git reset --hard HEAD@{2}
```

### Conflictos de merge

```bash
# Durante merge
git status  # Ver conflictos
# Editar archivos
git add archivo-resuelto
git commit

# O abortar
git merge --abort

# Elegir versión completa
git checkout --ours archivo.txt
git checkout --theirs archivo.txt
```

### Problemas con push

```bash
# Push rechazado
git pull --rebase
git push

# Necesitas force push (feature branch)
git push --force-with-lease

# Remoto cambió
git fetch origin
git reset --hard origin/main
```

---


---

## Navegación

- [⬅️ Anterior: git clean](20-git-clean.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: Referencias y Placeholders](22-referencias-placeholders.md)

